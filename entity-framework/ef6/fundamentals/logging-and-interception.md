---
title: Journalisation et l’interception des opérations de base de données - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 2e16502abf54be3f3b2f63fe69d2605ef13dea27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994633"
---
# <a name="logging-and-intercepting-database-operations"></a>Journalisation et l’interception des opérations de base de données
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

En commençant par Entity Framework 6, chaque fois qu’Entity Framework envoie une commande à la base de données que cette commande peut être intercepté par code d’application. Cela est plus couramment utilisée pour la journalisation SQL, mais peut également servir à modifier ou à abandonner la commande.  

Plus précisément, EF inclut :  

- Une propriété de journal pour le contexte similaire à DataContext.Log dans LINQ to SQL  
- Un mécanisme permettant de personnaliser le contenu et la mise en forme de la sortie est envoyée dans le journal  
- Blocs de construction de bas niveau pour l’interception en donnant le contrôle flexibilité  

## <a name="context-log-property"></a>Propriété de contexte journal  

La propriété DbContext.Database.Log peut être définie à un délégué pour toute méthode qui prend une chaîne. Il est plus couramment utilisé avec n’importe quel TextWriter en lui affectant la méthode « Écriture » de ce TextWriter. Tout le SQL généré par le contexte actuel est enregistré pour que l’enregistreur. Par exemple, le code suivant consigne SQL à la console :  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Notez que ce contexte. Database.Log est défini sur Console.Write. Il s’agit tout ce qui est nécessaire pour ouvrir une session SQL dans la console.  

### <a name="example-output"></a>Résultat de l'exemple  

Nous allons ajouter un code de requête/insertion/mise à jour simple afin que nous pouvons voir une sortie :  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Cette opération génère la sortie suivante :  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Notez qu’il s’agit de la sortie en supposant que toute initialisation de la base de données a déjà eu lieu. Si l’initialisation de base de données n’était pas déjà passé, il serait beaucoup plus sortie montrant tout le travail Migrations n’en arrière-plan pour vérifier ou créer une base de données.)  

### <a name="what-gets-logged"></a>Ce qui obtient enregistré ?  

Lorsque la propriété du journal a la valeur toutes les conditions suivantes est journalisé :  

- SQL pour tous les différents types de commandes. Exemple :  
    - Requêtes, y compris les requêtes LINQ normales, les requêtes eSQL et requêtes brutes à partir de méthodes telles que SqlQuery  
    - Insertions, mises à jour et suppressions générées dans le cadre de SaveChanges  
    - Relation du chargement de requêtes telles que celles générées par le chargement différé  
- Paramètres  
- Si la commande est exécutée de façon asynchrone  
- Un horodatage indiquant quand la commande a démarré l’exécution  
- Si la commande s’est terminée avec succès, échec en levant une exception, ou, pour une opération asynchrone, a été annulée.  
- Des indications sur la valeur de résultat  
- La durée approximative que nécessaire pour exécuter la commande. Notez qu’il s’agit de l’heure à partir de l’envoi de la commande pour récupérer l’objet de résultat. Il n’inclut pas le temps de lire les résultats.  

En examinant la sortie de l’exemple ci-dessus, chacune des quatre commandes enregistrées sont :  

- La requête résultant de l’appel de contexte. Blogs.First  
    - Notez que la méthode ToString de l’obtention de SQL n’avaient pas fonctionné pour cette requête depuis "First" ne fournit pas d’IQueryable sur lequel ToString peut être appelée.  
- La requête résultant le chargement différé de blog. Billets  
    - Notez que les détails de paramètre pour la valeur de clé pour le chargement différé qui se passe  
    - Seules les propriétés du paramètre qui sont définies sur les valeurs par défaut sont enregistrées. Par exemple, la propriété de taille apparaît uniquement si elle est différente de zéro.  
- Deux commandes résultant SaveChangesAsync ; l’autre pour la mise à jour modifier un titre de la publication, l’autre pour une instruction insert ajouter un nouveau billet  
    - Notez les détails de paramètre pour les propriétés de clé étrangère et de titre  
    - Notez que ces commandes sont en cours d’exécution en mode asynchrone  

### <a name="logging-to-different-places"></a>Journalisation dans différents emplacements  

Comme indiqué ci-dessus journalisation dans la console est très facile. Il est également facile de se connecter à la mémoire, le fichier, etc. à l’aide de différents types de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Si vous êtes familiarisé avec LINQ to SQL, vous pouvez remarquer que dans LINQ to SQL de la propriété de journal a vers le TextWriter objet en question (par exemple, Console.Out) lors de la dans EF, la propriété de journal est définie à une méthode qui accepte une chaîne (par exemple Console.Write ou Console.Out.Write). La raison à cela consiste à découpler EF à partir de TextWriter en acceptant un délégué qui peut agir en tant que récepteur pour les chaînes. Par exemple, imaginez que vous disposez déjà d’une infrastructure de journalisation et il définit une méthode de journalisation comme suit :  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Cela peut être rattachée à la propriété EF journal comme suit :  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>Journalisation des résultats  

L’enregistreur d’événements par défaut consigne le texte de commande (SQL), des paramètres et la ligne « Exécution » avec un horodatage avant la commande est envoyée à la base de données. Une ligne « terminée » qui contient le temps écoulé est connectée exécution suivante de la commande.  

Notez que pour les commandes asynchrones la ligne « terminée » n’est pas connectée jusqu'à ce que la tâche async effectivement terminée échoue ou est annulée.  

La ligne « terminée » contient des informations différentes selon le type de commande et si l’exécution a réussi.  

#### <a name="successful-execution"></a>Exécution réussie  

Pour les commandes qui aboutissent à la sortie est « terminé dans x ms avec résultat : » suivie des indications sur le résultat. Indication commandes qui retournent un lecteur de données du résultat est le type de [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) retourné. Pour les commandes qui retournent une valeur entière, tels que la mise à jour la commande présentée ci-dessus le résultat illustré est cet entier.  

#### <a name="failed-execution"></a>Exécution a échoué  

Pour les commandes qui échouent en levant une exception, la sortie contient le message de l’exception. Par exemple, à l’aide de SqlQuery à la requête sur une table qui existe sera le résultat dans le journal de sortie quelque chose comme ceci :  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>Exécution a été annulée  

Pour les commandes asynchrones où la tâche est annulée. le résultat peut être échec avec une exception, étant donné que c’est ce que le fournisseur ADO.NET sous-jacent souvent lorsqu’une tentative est effectuée pour annuler. Si cela ne se produit pas et que la tâche est annulée correctement la sortie se présente quelque chose comme ceci :  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Modification du contenu du journal et mise en forme  

### <a name="databaselogformatter"></a>DatabaseLogFormatter  

En coulisse le Database.Log propriété permet d’utiliser d’un objet DatabaseLogFormatter. Cet objet lie une implémentation IDbCommandInterceptor (voir ci-dessous) à un délégué qui accepte les chaînes et un DbContext. Cela signifie que les méthodes sur DatabaseLogFormatter sont appelées avant et après l’exécution de commandes par Entity Framework. Ces méthodes DatabaseLogFormatter rassemblent et formater la sortie de journal, puis envoyez-la au délégué.  

### <a name="customizing-databaselogformatter"></a>Personnalisation de DatabaseLogFormatter  

Modification de ce qui est enregistré et la mise en forme est possible en créant une nouvelle classe qui dérive de DatabaseLogFormatter et remplace les méthodes appropriées. Les méthodes les plus courantes pour substituer sont :  

- LogCommand – substituer cette méthode pour modifier la façon dont les commandes sont journalisées avant leur exécution. Par défaut LogCommand appelle LogParameter pour chaque paramètre ; Vous pouvez choisir de faire de même dans votre remplacement ou de gérer les paramètres différemment à la place.  
- LogResult – substituer cette méthode pour modifier la façon dont le résultat de l’exécution d’une commande est connecté.  
- LogParameter – substituer cette méthode pour modifier la mise en forme et le contenu de la journalisation de paramètre.  

Par exemple, supposons que nous voulions pour se connecter à une seule ligne avant chaque commande est envoyée à la base de données. Cela est possible avec deux remplacements :  

- Substituer LogCommand pour mettre en forme et d’écriture de la ligne de SQL  
- Substituer LogResult pour ne rien faire.  

Le code ressemblerait à quelque chose comme ceci :

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Pour ouvrir une session de sortie appel simplement la méthode Write lequel envoyer la sortie au délégué écriture configuré.  

(Notez que ce code effectue une suppression simpliste de sauts de ligne à titre d’exemple. Il fonctionneront probablement pas bien pour l’affichage SQL complexe.)  

### <a name="setting-the-databaselogformatter"></a>Définition de la DatabaseLogFormatter  

Une fois qu’une nouvelle classe DatabaseLogFormatter a été créée, il doit être enregistré avec Entity Framework. Cette opération est effectuée à l’aide de la configuration basée sur le code. En bref, cela signifie que la création d’une nouvelle classe qui dérive de DbConfiguration dans le même assembly que votre classe DbContext, puis en appelant SetDatabaseLogFormatter dans le constructeur de cette nouvelle classe. Exemple :  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>À l’aide de la nouvelle DatabaseLogFormatter  

Cette nouvelle DatabaseLogFormatter serviront désormais à tout moment Database.Log est définie. Par conséquent, le code en cours d’exécution à partir de la partie 1 entraîne la sortie suivante :  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Blocs de construction de l’interception  

Jusqu'à présent, nous avons vu comment utiliser DbContext.Database.Log pour vous connecter le SQL généré par EF. Mais ce code est en fait une façade relativement mince sur certains blocs de construction de bas niveau pour l’interception plus générale.  

### <a name="interception-interfaces"></a>Interfaces de l’interception  

Le code de l’interception est construit autour du concept d’interfaces de l’interception. Ces interfaces héritent IDbInterceptor et définissent des méthodes qui sont appelées lorsque EF exécute une action. L’objectif est de disposer d’une interface par type d’objet intercepté. Par exemple, l’interface IDbCommandInterceptor définit des méthodes qui sont appelées avant EF effectue un appel à la méthode ExecuteNonQuery, ExecuteScalar, ExecuteReader et méthodes associées. De même, l’interface définit des méthodes qui sont appelées lorsque chacune de ces opérations est terminée. La classe DatabaseLogFormatter que nous avons vu ci-dessus implémente cette interface de journaliser les commandes.  

### <a name="the-interception-context"></a>Le contexte de l’interception  

Examinez les méthodes définies sur les interfaces de l’intercepteur, il est évident que chaque appel étant donné un objet de type DbInterceptionContext ou un type dérivé de cet telles que DbCommandInterceptionContext\<\>. Cet objet contient des informations contextuelles sur l’action EF est en cours. Par exemple, si l’action est prise pour le compte d’un DbContext, DbContext est inclus dans le DbInterceptionContext. De même, pour les commandes qui sont exécutées de façon asynchrone, l’indicateur IsAsync est définie sur DbCommandInterceptionContext.  

### <a name="result-handling"></a>Traitement des résultats  

Le DbCommandInterceptionContext\< \> classe contient un propriétés appelées résultat, OriginalResult, Exception et exception d’origine. Ces propriétés sont définies sur null/zéro pour les appels aux méthodes qui sont appelées avant l’exécution de l’opération de l’interception, autrement dit, pour le... Exécution de méthodes. Si l’opération est exécutée et réussit, résultat et OriginalResult sont définies pour le résultat de l’opération. Ces valeurs peuvent ensuite être observées dans les méthodes de l’interception qui sont appelées après l’exécution de l’opération, autrement dit, dans la... Méthodes exécutées. De même, si l’opération lève, les propriétés de l’Exception et l’exception d’origine seront définies.  

#### <a name="suppressing-execution"></a>Suppression de l’exécution  

Si un intercepteur définit la propriété Result avant l’exécution de la commande (dans un du... Exécution de méthodes) puis EF ne tentera pas exécuter la commande, mais au lieu de cela utilisent simplement le jeu de résultats. En d’autres termes, l’intercepteur peut supprimer l’exécution de la commande, mais avez EF continuer comme si la commande avait été exécutée.  

Un exemple de comment cela peut être utilisée est la commande que le traitement par lots qui s’effectue traditionnellement avec un fournisseur d’habillage. L’intercepteur contiendrait la commande pour une exécution ultérieure en tant que lot mais serait « prétend « EF la commande avait été exécutées comme d’habitude. Notez qu’elle nécessite plus de cela pour implémenter le traitement par lot, mais il s’agit d’un exemple d’utilisation de modifier le résultat de l’interception peut être.  

L’exécution peut également être supprimée en définissant la propriété d’Exception dans un du... Exécution de méthodes. Par conséquent, EF continuer comme si l’exécution de l’opération a échoué en levant l’exception donnée. Cela peut, bien sûr, incident, mais il peut également être une exception temporaire ou une autre exception est gérée par Entity Framework. Par exemple, cela peut servir dans des environnements de test pour tester le comportement d’une application en cas d’échec de l’exécution de la commande.  

#### <a name="changing-the-result-after-execution"></a>Modifier le résultat après l’exécution  

Si un intercepteur définit la propriété de résultat après l’exécution de la commande (dans un du... Exécuter des méthodes) puis EF utilise le résultat modifié au lieu du résultat a effectivement été renvoyé à partir de l’opération. De même, si un intercepteur définit la propriété d’Exception une fois la commande exécutée, puis EF lève l’exception de l’ensemble comme si l’opération a levé l’exception.  

Un intercepteur peut également définir la propriété d’Exception à la valeur null pour indiquer qu’aucune exception ne doit être levée. Cela peut être utile si l’exécution de l’opération a échoué, mais l’intercepteur souhaite EF pour continuer comme si l’opération s’est déroulée correctement. Cela généralement également implique de définir le résultat afin qu’EF a une valeur de résultat à utiliser comme il se poursuit.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult et l’exception d’origine  

Une fois que EF a exécuté une opération il définit les propriétés de résultat et OriginalResult si l’exécution n’a pas échoué ou les propriétés de l’Exception et l’exception d’origine si l’exécution a échoué avec une exception.  

Les propriétés OriginalResult et l’exception d’origine sont en lecture seule et sont définies uniquement par Entity Framework après réellement l’exécution d’une opération. Ces propriétés ne peuvent pas être définies par les intercepteurs. Cela signifie que tout intercepteur peut faire la distinction entre une exception ou un résultat qui a été définie par certains autres intercepteur par opposition à l’exception réelle ou qui s’est produite lors de l’opération a été exécutée.  

### <a name="registering-interceptors"></a>L’inscription des intercepteurs  

Une fois qu’une classe qui implémente une ou plusieurs des interfaces de l’interception a été créée, il peut être inscrit avec Entity Framework à l’aide de la classe DbInterception. Exemple :  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Intercepteurs peuvent également être inscrits au niveau du domaine d’application en utilisant le mécanisme de configuration basée sur le code DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Exemple : La journalisation NLog  

Nous allons rassemblé tout cela dans un exemple que l’utilisation IDbCommandInterceptor et [NLog](http://nlog-project.org/) à :  

- Consigne un avertissement pour n’importe quelle commande est exécutée de façon non asynchrone  
- Consigner une erreur pour n’importe quelle commande lève lors de l’exécution  

Voici la classe qui effectue la journalisation, qui doit être enregistrée comme indiqué ci-dessus :  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Notez la façon dont ce code utilise le contexte de l’interception pour détecter quand une commande est exécutée de façon non asynchrone et pour découvrir qu’il y a une erreur d’exécution d’une commande.  
