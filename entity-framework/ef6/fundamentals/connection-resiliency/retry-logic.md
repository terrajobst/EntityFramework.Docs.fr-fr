---
title: La résilience et nouvelle tentative logique de connexion - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: d7e58abfa17c5537cdc9b0068e7c2a3c2e390038
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250515"
---
# <a name="connection-resiliency-and-retry-logic"></a>Logique de résilience et de nouvelle tentative de connexion
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Applications se connectant à un serveur de base de données ont toujours été vulnérables aux sauts de connexion en raison de défaillances back-end et une instabilité du réseau. Toutefois, dans un environnement de réseau local fonctionne avec les serveurs de base de données dédiée, ces erreurs sont assez rares pour une logique supplémentaire pour gérer ces échecs n’est pas souvent nécessaire. Avec l’essor du cloud en fonction des serveurs de base de données, tels que Windows Azure SQL Database et les connexions sur des réseaux moins fiables, qu'il est désormais plus courant pour les sauts de connexion. Cela peut être dû à des techniques défensives qui permet de garantir l’équité de service, tels que les limitations de connexion ou d’instabilité du réseau qui provoque des délais d’attente intermittentes et autres erreurs temporaires de bases de données en nuage.  

La résilience de connexion fait référence à la possibilité pour EF de réessayer automatiquement toutes les commandes qui échouent en raison de ces interruptions de connexion.  

## <a name="execution-strategies"></a>Stratégies d’exécution  

Nouvelle tentative de connexion est pris en charge par une implémentation de l’interface IDbExecutionStrategy. Les implémentations de la IDbExecutionStrategy sera responsables de l’acceptation d’une opération et, si une exception se produit, déterminer si une nouvelle tentative est appropriée et une nouvelle tentative s’il s’agit. Il existe quatre stratégies d’exécution fournis avec Entity Framework :  

1. **DefaultExecutionStrategy**: cette stratégie d’exécution ne retente pas toutes les opérations, il est la valeur par défaut pour les bases de données autres que sql server.  
2. **DefaultSqlExecutionStrategy**: il s’agit d’une stratégie d’exécution interne qui est utilisée par défaut. Cette stratégie ne retente pas du tout, toutefois, il encapsule toutes les exceptions qui peut être transitoires à informer les utilisateurs qu’ils veulent peuvent activer la résilience des connexions.  
3. **DbExecutionStrategy**: cette classe est appropriée comme classe de base pour les autres stratégies d’exécution, y compris vos propres modèles personnalisés. Il implémente une stratégie de nouvelle tentative exponentielle, où la nouvelle tentative initiale produit avec zéro délai et le délai augmente exponentiellement jusqu'à ce que le nombre maximal de tentatives est atteint. Cette classe possède une méthode abstraite ShouldRetryOn qui peut être implémentée dans les stratégies d’exécution dérivée pour contrôler les exceptions qui doivent être retentées.  
4. **SqlAzureExecutionStrategy**: cette stratégie d’exécution hérite DbExecutionStrategy et réessaie sur les exceptions qui sont connues pour être éventuellement temporaires lorsque vous travaillez avec la base de données SQL Azure.

> [!NOTE]
> Stratégies d’exécution 2 et 4 sont inclus dans le fournisseur Sql Server qui est fourni avec Entity Framework, ce qui se trouve dans l’assembly EntityFramework.SqlServer et sont conçus pour fonctionner avec SQL Server.  

## <a name="enabling-an-execution-strategy"></a>L’activation d’une stratégie d’exécution  

Pour demander à EF d’utiliser une stratégie d’exécution le plus simple est de la méthode SetExecutionStrategy de la [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe :  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Ce code indique à EF d’utiliser le SqlAzureExecutionStrategy lors de la connexion à SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Configuration de la stratégie d’exécution  

Le constructeur de SqlAzureExecutionStrategy peut accepter deux paramètres, MaxRetryCount et MaxDelay. Nombre de la valeur MaxRetry est le nombre maximal de tentatives de la stratégie. Le MaxDelay est un TimeSpan représentant le délai maximal entre les nouvelles tentatives qui utilise la stratégie d’exécution.  

Pour définir le nombre maximal de nouvelles tentatives à 1 et le délai maximal de 30 secondes, vous devez execue les éléments suivants :  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

Le SqlAzureExecutionStrategy réessaiera instantanément la première fois, une défaillance passagère se produit, mais plus retardera entre chaque tentative, jusqu'à ce que soit le nombre maximal de nombre limite de tentatives est dépassée ou le temps total atteint le délai maximal.  

Les stratégies d’exécution va réessayer uniquement un nombre limité d’exceptions qui sont généralement tansient, vous devez toujours gérer les autres erreurs ainsi intercepter l’exception RetryLimitExceeded pour le cas où une erreur n’est pas temporaire ou prend trop de temps à résoudre lui-même.  

Il existe certaines connus des limitations lorsque vous utilisez une stratégie d’exécution de nouvelle tentative :  

## <a name="streaming-queries-are-not-supported"></a>Requêtes de diffusion en continu ne sont pas pris en charge.  

Par défaut, EF6 et version ultérieure met en mémoire tampon les résultats de requête, plutôt que de diffusion en continu les. Si vous souhaitez avoir des résultats transmis en continu vous pouvez utiliser la méthode AsStreaming pour modifier un LINQ pour interroger des entités à la diffusion en continu.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Diffusion en continu n’est pas pris en charge lorsqu’une stratégie d’exécution de nouvelle tentative est inscrit. Cette limitation existe parce que la connexion peut supprimer la partie dans les résultats retournés. Lorsque cela se produit, EF doit exécuter à nouveau la requête entière, mais n’a aucun moyen fiable de savoir ce qui se traduit ont déjà été retournés (données peuvent avoir changé depuis la requête initiale a été envoyée, résultats peuvent revenir dans un ordre différent, résultats ne peuvent pas avoir un identificateur unique etc..).  

## <a name="user-initiated-transactions-are-not-supported"></a>Les transactions initiées par l’utilisateur ne sont pas pris en charge.  

Lorsque vous avez configuré une stratégie d’exécution qui résulte de nouvelles tentatives, il existe certaines limitations concernant l’utilisation de transactions.  

Par défaut, Entity Framework effectue des mises à jour de la base de données dans une transaction. Vous n’avez pas besoin de faire quelque chose pour ce faire, EF toujours fait automatiquement.  

Par exemple, dans le code suivant SaveChanges est effectuée automatiquement dans une transaction. Si SaveChanges tombe en panne après insertion parmi le nouveau Site puis la transaction est restaurée et aucune modification appliquée à la base de données. Le contexte est également laissé dans un état qui permet de SaveChanges à appeler à nouveau pour appliquer les modifications de nouvelle tentative.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Lorsque vous N'utilisez pas une stratégie d’exécution de nouvelle tentative, vous pouvez encapsuler plusieurs opérations dans une transaction unique. Par exemple, le code suivant encapsule les deux appels à SaveChanges dans une transaction unique. Si n’importe quelle partie de l’opération échoue, aucune des modifications sont appliquées.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Cela n’est pas pris en charge lors de l’utilisation d’une stratégie d’exécution de nouvelle tentative, car EF ne tient pas compte de toutes les opérations précédentes et comment les retenter. Par exemple, si le deuxième SaveChanges a échoué puis EF n’est plus contient les informations requises pour le premier appel de SaveChanges de nouvelle tentative.  

### <a name="workaround-suspend-execution-strategy"></a>Solution de contournement : Interrompre la stratégie d’exécution  

Une solution de contournement possible est de suspendre la stratégie d’exécution de nouvelle tentative pour la partie du code qui a besoin d’utiliser un utilisateur a lancé la transaction. Pour ce faire, le plus simple consiste à ajouter un indicateur SuspendExecutionStrategy à votre code basé sur la classe de configuration et modifier l’expression lambda à la stratégie d’exécution pour retourner la stratégie d’exécution par défaut (non retying) lorsque l’indicateur est défini.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Notez que nous utilisons CallContext pour stocker la valeur d’indicateur. Cela fournit une fonctionnalité similaire à un stockage local des threads, mais est déconseillé d’utiliser avec le code asynchrone - y compris la requête asynchrone et l’enregistrer avec Entity Framework.  

Nous pouvons désormais suspendre la stratégie d’exécution pour la section de code qui utilise une transaction lancée par l’utilisateur.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Solution de contournement : Appeler manuellement la stratégie d’exécution  

Une autre option consiste à utiliser la stratégie d’exécution manuellement et de lui donner l’ensemble de la logique à exécuter, afin qu’elle peut réessayer tout ce que si une des opérations échoue. Nous avons besoin de suspendre la stratégie d’exécution - à l’aide de la technique ci-dessus - afin que les contextes utilisées à l’intérieur du bloc de code renouvelable n’essayez pas de nouvelle tentative.  

Notez que les contextes doivent être construites dans le bloc de code, une nouvelle tentative. Cela garantit que nous avons commencé avec un état propre pour chaque nouvelle tentative.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
