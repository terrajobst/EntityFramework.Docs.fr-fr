---
title: Définition de DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39121728"
---
# <a name="defining-dbsets"></a>Définition de DbSets
Lors du développement avec le workflow de Code First que vous définissez un DbContext dérivé qui représente votre session avec la base de données et expose un DbSet pour chaque type dans votre modèle. Cette rubrique décrit les différentes méthodes que vous pouvez définir les propriétés DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext avec des propriétés DbSet  

Le cas courant indiqué dans les exemples de Code First consiste à avoir un DbContext avec les propriétés publiques DbSet automatique pour les types d’entités de votre modèle. Exemple :  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Lorsqu’il est utilisé en mode Code First, cette opération configure des Blogs et des publications en tant que types d’entité, comme la configuration est accessible à partir de ces autres types de. En outre, DbContext appelle automatiquement la méthode setter pour chacune de ces propriétés pour définir une instance de la DbSet appropriée.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext avec des propriétés IDbSet  

Il existe des situations, par exemple créer des objets fictifs ou fakes, où il est plus utile déclarer des propriétés de votre jeu à l’aide d’une interface. Dans ce cas le IDbSet interface peut être utilisée à la place de DbSet. Exemple :  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Ce contexte fonctionne dans exactement la même façon que le contexte qui utilise la classe DbSet pour ses propriétés de jeu.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext les propriétés définies en lecture seule  

Si vous ne souhaitez pas exposer des accesseurs Set publics pour vos propriétés DbSet ou IDbSet, vous pouvez créer des propriétés en lecture seule et créer les instances vous-même. Exemple :  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Notez que DbContext met en cache l’instance de DbSet retourné à partir de la méthode Set afin que chacune de ces propriétés retournera la même instance chaque fois qu’elle est appelée.  

Découverte de types d’entités pour Code First fonctionne de la même façon ici car il fait pour les propriétés avec accesseurs Get publique et Set.  
