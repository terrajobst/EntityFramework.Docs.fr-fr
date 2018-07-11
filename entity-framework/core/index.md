---
title: Vue d’ensemble - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 103e5e069687950a8411f2d92c7b5a191844e0ae
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948988"
---
# <a name="entity-framework-core-quick-overview"></a>Vue d’ensemble d’Entity Framework Core

Entity Framework (EF) Core est une version légère, extensible et multiplateforme de la technologie d’accès aux données Entity Framework populaire.

EF Core peut servir de mappeur relationnel/objet (O/RM), permettant aux développeurs .NET de travailler avec une base de données à l’aide d’objets .NET, et éliminant la nécessité de la plupart du code d’accès aux données qu’ils doivent généralement écrire.

EF Core prend en charge de nombreux moteurs de base de données ; consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations.

Si vous voulez approfondir vos connaissances en écrivant du code, nous vous recommandons un de nos guides de [démarrage](get-started/index.md) pour commencer avec EF Core.

## <a name="what-is-new-in-ef-core"></a>Nouveautés d’EF Core

Si vous connaissez déjà EF Core et souhaitez vous plonger directement dans les détails des dernières versions :

- **[Nouveautés d’EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Mise à niveau d’applications existantes vers EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Obtenir Entity Framework Core

[Installez le package NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) pour le fournisseur de base de données que vous souhaitez utiliser. Par exemple, pour installer le fournisseur SQL Server dans un développement multiplateforme à l’aide de l’outil `dotnet` à partir de la ligne de commande :

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Ou, dans Visual Studio, à l’aide de la console du Gestionnaire de package :

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Consultez [Fournisseurs de base de données](providers/index.md) pour plus d’informations sur les fournisseurs disponibles et [Installation d’EF Core](get-started/install/index.md) pour découvrir la procédure d’installation détaillée.

## <a name="the-model"></a>Modèle

Avec EF Core, l’accès aux données est effectué à l’aide d’un modèle. Un modèle est composé de classes d’entité et d’un contexte dérivé qui représente une session avec la base de données, ce qui vous permet d’interroger et d’enregistrer des données. Pour en savoir plus, consultez [Création d’un modèle](modeling/index.md).

Vous pouvez générer un modèle à partir d’une base de données existante, coder manuellement un modèle en fonction de votre base de données ou utiliser EF Migrations pour créer une base de données à partir de votre modèle (et la faire évoluer au même rythme que celui-ci).

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a>Interrogation

Les instances de vos classes d’entité sont récupérées de la base de données à l’aide de LINQ (Language Integrated Query). Pour en savoir plus, consultez [Interrogation des données](querying/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Enregistrement de données

Les données sont créées, supprimées et modifiées dans la base de données à l’aide d’instances de vos classes d’entité. Pour en savoir plus, consultez [Enregistrement de données](saving/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
