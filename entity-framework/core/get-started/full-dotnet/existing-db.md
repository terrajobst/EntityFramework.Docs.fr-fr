---
title: "Bien démarrer avec .NET Framework - Base de données existante - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Bien démarrer avec EF Core sur .NET Framework avec une base de données existante

Dans cette procédure pas à pas, vous allez générer une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework. Vous allez utiliser l’ingénierie à rebours pour créer un modèle Entity Framework à partir d’une base de données existante.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) sur GitHub.

## <a name="prerequisites"></a>Conditions préalables

Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Dernière version du Gestionnaire de package NuGet](https://dist.nuget.org/index.html)

* [Dernière version de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Base de données de création de blogs](#blogging-database)

### <a name="blogging-database"></a>Base de données de création de blogs

Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.

> [!TIP]  
> Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre didacticiel, vous pouvez ignorer ces étapes.

* Ouvrir Visual Studio

* Outils > Connexion à une base de données...

* Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.

* Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.

* Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.

* La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.

* Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.

* Copiez le script ci-dessous dans l’éditeur de requêtes.

* Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Créer un projet

* Ouvrir Visual Studio

* Fichier > Nouveau > Projet...

* Dans le menu de gauche, sélectionnez Modèles > Visual C# > Windows.

* Choisissez le modèle de projet **Application console**.

* Assurez-vous de bien cibler **.NET Framework 4.5.1** ou une version ultérieure.

* Nommez le projet et cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Pour activer l’ingénierie à rebours à partir d’une base de données existante, vous devez également installer deux autres packages.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="reverse-engineer-your-model"></a>Rétroconcevoir le modèle

Créons à présent le modèle EF à partir de votre base de données existante.

* Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package

* Exécutez la commande suivante pour créer un modèle à partir de la base de données existante.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Le processus d’ingénierie à rebours créé des classes d’entité et un contexte dérivé en fonction du schéma de la base de données existante. Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *Program.cs*.

* Remplacez le contenu du fichier par le code suivant.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Déboguer > Exécuter sans débogage

Vous verrez qu’un blog est enregistré dans la base de données et que les détails de tous les blogs s’affichent dans la console.

![image](_static/output-existing-db.png)
