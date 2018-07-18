---
title: Chaînes de connexion et de modèles - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121470"
---
# <a name="connection-strings-and-models"></a>Modèles et les chaînes de connexion
Cette rubrique décrit comment Entity Framework détecte la connexion de base de données à utiliser, et comment vous pouvez le modifier. Les modèles créés avec Code First et le Concepteur EF sont traités dans cette rubrique.  

En général, une application Entity Framework utilise une classe dérivée de DbContext. Cette classe dérivée appellera un des constructeurs de la classe DbContext de base au contrôle :  

- Comment le contexte doit se connecter à une base de données, autrement dit, comment une chaîne de connexion est trouvé/utilisé  
- Si le contexte utilisera calculer un modèle à l’aide de Code First ou charger un modèle créé avec le Concepteur EF  
- Options avancées supplémentaires  

Les fragments suivants illustrent que certaines des façons les constructeurs DbContext peuvent être utilisées.  

## <a name="use-code-first-with-connection-by-convention"></a>Utiliser Code First par convention avec connexion  

Si vous n’avez pas fait toute autre configuration dans votre application, puis en appelant le constructeur sans paramètre sur DbContext entraîne DbContext exécuter en mode Code First avec une connexion de base de données créée par convention. Exemple :  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

Dans cet exemple DbContext utilise le nom qualifié d’espace de noms de votre class—Demo.EF.BloggingContext—as de contexte dérivé du nom de la base de données et crée une chaîne de connexion pour cette base de données à l’aide de SQL Express ou LocalDB. Si les deux sont installés, SQL Express sera utilisé.  

Visual Studio 2010 inclut SQL Express par défaut et Visual Studio 2012 et versions ultérieures inclut LocalDB. Pendant l’installation, le package EntityFramework NuGet vérifie le serveur de base de données est disponible. Le package NuGet est puis mettez à jour le fichier de configuration en définissant le serveur de base de données par défaut qui utilise Code First lors de la création d’une connexion par convention. Si SQL Express est en cours d’exécution, il sera utilisé. Si SQL Express n’est pas disponible, base de données locale sera être enregistré en tant que la valeur par défaut à la place. Aucun modifications ne sont apportées au fichier de configuration si elle contient déjà un paramètre pour la fabrique de connexion par défaut.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Utiliser Code First avec connexion par convention et le nom de la base de données spécifiée  

Si vous n’avez pas fait toute autre configuration dans votre application, puis en appelant le constructeur string sur DbContext avec le nom de base de données que vous souhaitez utiliser entraîne DbContext exécuter en mode Code First avec une connexion de base de données créée par convention à la base de données Ce nom. Exemple :  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

Dans cet exemple DbContext utilise « BloggingDatabase » comme nom de base de données et crée une chaîne de connexion pour cette base de données à l’aide de SQL Express (installé avec Visual Studio 2010) ou base de données locale (installée avec Visual Studio 2012). Si les deux sont installés, SQL Express sera utilisé.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Utiliser Code First avec la chaîne de connexion dans le fichier de app.config/web.config  

Vous pouvez choisir de placer une chaîne de connexion dans votre fichier app.config ou web.config. Exemple :  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Il s’agit d’un moyen simple pour indiquer à DbContext d’utiliser un serveur de base de données autre que SQL Express ou de la base de données locale, l’exemple ci-dessus spécifie une base de données SQL Server Compact Edition.  

Si le nom de la chaîne de connexion correspond au nom de votre contexte (avec ou sans qualification d’espace de noms) puis il se trouve par DbContext lorsque le constructeur sans paramètre est utilisé. Si le nom de chaîne de connexion est différent du nom de votre contexte vous pouvez indiquer DbContext pour utiliser cette connexion en mode Code First en passant le nom de chaîne de connexion au constructeur DbContext. Exemple :  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Vous pouvez également utiliser la forme « nom =\<nom de chaîne de connexion\>» pour la chaîne passée au constructeur DbContext. Exemple :  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Ce formulaire rend explicite à laquelle la chaîne de connexion dans votre fichier de configuration. Une exception sera levée si une chaîne de connexion portant le nom spécifié est introuvable.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Base de données/Model First avec la chaîne de connexion dans le fichier de app.config/web.config  

Les modèles créés avec le Concepteur EF diffèrent des Code First dans la mesure votre modèle existe déjà et n’est pas généré à partir du code lorsque l’application s’exécute. Le modèle existe en général comme un fichier EDMX dans votre projet.  

Le concepteur ajouterez une chaîne de connexion EF à votre fichier app.config ou web.config. Cette chaîne de connexion est spéciale car elle contient des informations sur la façon de trouver les informations dans votre fichier EDMX. Exemple :  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

Le Concepteur EF génère également un code qui indique à DbContext pour utiliser cette connexion en passant le nom de chaîne de connexion au constructeur DbContext. Exemple :  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext sache qu’il peut pour charger le modèle existant (au lieu de Code First pour être calculé à partir du code), car la chaîne de connexion est une chaîne de connexion EF contenant les détails du modèle à utiliser.  

## <a name="other-dbcontext-constructor-options"></a>Autres options de constructeur DbContext  

La classe DbContext contient d’autres constructeurs et les modèles d’utilisation qui permettent des scénarios plus avancés. Certaines d'entre elles sont :  

- Vous pouvez utiliser la classe DbModelBuilder pour générer un modèle Code First sans instancier une instance de DbContext. Le résultat de ce est un objet DbModel. Vous pouvez ensuite passer cet objet DbModel à un des constructeurs DbContext lorsque vous êtes prêt à créer votre instance de DbContext.  
- Vous pouvez passer une chaîne de connexion complète de DbContext au lieu de simplement le nom de chaîne de base de données ou de connexion. Par défaut, cette chaîne de connexion est utilisée avec le fournisseur System.Data.SqlClient ; Cela peut être modifié en définissant une implémentation différente de IConnectionFactory sur le contexte. Database.DefaultConnectionFactory.  
- Vous pouvez utiliser un objet DbConnection existant en le passant à un constructeur DbContext. Si l’objet de connexion est une instance de EntityConnection, puis le modèle spécifié dans la connexion sera utilisé plutôt que calculer un modèle avec Code First. Si l’objet est une instance d’un autre type — par exemple, SqlConnection, le contexte de l’utilisera pour le mode Code First.  
- Vous pouvez passer un ObjectContext existant à un constructeur DbContext pour créer un DbContext encapsulant le contexte existant. Cela peut être utilisé pour les applications existantes qui utilisent ObjectContext, mais qui souhaitent tirer parti de DbContext dans certaines parties de l’application.  
