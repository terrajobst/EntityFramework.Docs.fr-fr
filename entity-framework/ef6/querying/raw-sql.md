---
title: Requêtes SQL brutes - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283782"
---
# <a name="raw-sql-queries"></a>Requêtes SQL brutes
Entity Framework vous permet d’interroger à l’aide de LINQ avec vos classes d’entité. Toutefois, il peut arriver que vous souhaitez exécuter des requêtes à l’aide de requêtes SQL brutes directement par rapport à la base de données. Cela inclut l’appel de procédures stockées, qui peuvent être utiles pour les modèles de Code First qui ne prennent actuellement pas en charge le mappage à des procédures stockées. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Écriture de requêtes SQL pour les entités  

La méthode SqlQuery sur DbSet permet à une requête SQL brute à écrire qui retournera des instances d’entité. Les objets retournés sont suivis par le contexte comme ils le seraient si elles ont été retournées par une requête LINQ. Exemple :  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Notez que, comme pour les requêtes LINQ, la requête n'est pas exécutée jusqu'à ce que les résultats sont énumérés, dans l’exemple ci-dessus, cela est effectué avec l’appel à ToList.  

Soyez attentif à chaque fois que les requêtes SQL brutes sont écrits pour deux raisons. Tout d’abord, la requête doit être écrite pour vous assurer qu’elle renvoie uniquement les entités qui sont réellement du type demandé. Par exemple, lors de l’utilisation des fonctionnalités telles que l’héritage, il est facile d’écrire une requête qui crée des entités qui ont le type CLR approprié.  

En second lieu, certains types de requêtes SQL brutes exposent à des risques de sécurité potentiels, notamment concernant les attaques par injection SQL. Assurez-vous que vous utilisez des paramètres dans votre requête dans la méthode correcte pour protéger contre ces attaques.  

### <a name="loading-entities-from-stored-procedures"></a>Chargement d’entités à partir de procédures stockées  

Vous pouvez utiliser DbSet.SqlQuery pour charger des entités à partir des résultats d’une procédure stockée. Par exemple, le code suivant appelle le dbo. Procédure GetBlogs dans la base de données :  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Vous pouvez également transmettre des paramètres à une procédure stockée à l’aide de la syntaxe suivante :  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Écriture de requêtes SQL pour les types de non-entité  

Une requête SQL renvoie des instances de n’importe quel type, y compris les types primitifs, peut être créée à l’aide de la méthode SqlQuery sur la classe de base de données. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Les résultats retournés par SqlQuery sur la base de données ne sont jamais suivis par le contexte même si les objets sont des instances d’un type d’entité.  

## <a name="sending-raw-commands-to-the-database"></a>Envoi de commandes à la base de données brutes  

Commandes de non-requête peuvent être envoyés à la base de données à l’aide de la méthode ExecuteSqlCommand sur la base de données. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Notez que toutes les modifications apportées aux données dans la base de données à l’aide de ExecuteSqlCommand sont opaques pour le contexte jusqu'à ce que les entités chargées ou rechargées à partir de la base de données.  

### <a name="output-parameters"></a>Paramètres de sortie  

Si les paramètres de sortie sont utilisés, leurs valeurs ne sera pas disponibles jusqu'à ce que les résultats ont été lues entièrement. Il s’agit en raison du comportement sous-jacent de DbDataReader, consultez [extraction de données à l’aide d’un DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) pour plus d’informations.  
