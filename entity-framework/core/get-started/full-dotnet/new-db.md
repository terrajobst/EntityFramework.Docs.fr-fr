---
title: Bien démarrer avec .NET Framework - Nouvelle base de données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Bien démarrer avec EF Core sur .NET Framework avec une nouvelle base de données

Dans cette procédure pas à pas, vous allez générer une application console qui exécute l’accès aux données de base d’une base de données Microsoft SQL Server à l’aide d’Entity Framework. Vous utiliserez des migrations pour créer la base de données à partir de votre modèle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) sur GitHub.

## <a name="prerequisites"></a>Prérequis

Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Dernière version du Gestionnaire de package NuGet](https://dist.nuget.org/index.html)

* [Dernière version de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Créer un projet

* Ouvrir Visual Studio

* Fichier > Nouveau > Projet...

* Dans le menu de gauche, sélectionnez Modèles > Visual C# > Bureau classique Windows.

* Sélectionnez le modèle de projet **Application console (.NET Framework)**.

* Assurez-vous de bien cibler **.NET Framework 4.5.1** ou une version ultérieure.

* Nommez le projet et cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Un peu plus loin dans cette procédure pas à pas, nous utiliserons également des outils Entity Framework pour gérer la base de données. Nous installerons donc également le package d’outils.

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-your-model"></a>Créer un modèle

Définissons à présent le contexte et les classes d’entité qui composeront le modèle.

* Projet > Ajouter une classe...

* Entrez le nom *Model.cs* et cliquez sur **OK**.

* Remplacez le contenu du fichier par le code suivant.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

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

> [!TIP]  
> Dans une application réelle, vous placeriez chaque classe dans un fichier distinct et la chaîne de connexion dans le fichier `App.Config` que vous liriez à l’aide de `ConfigurationManager`. Dans le cadre de ce didacticiel, par souci de simplicité, nous plaçons tout dans un même fichier de code.

## <a name="create-your-database"></a>Créer la base de données

Une fois que vous avez un modèle, vous pouvez utiliser des migrations pour créer une base de données.

* Outils -> Gestionnaire de package NuGet -> Console du Gestionnaire de package

* Exécutez `Add-Migration MyFirstMigration` pour générer automatiquement un modèle de migration pour créer l’ensemble initial de tables de votre modèle.

* Exécutez `Update-Database` pour appliquer la nouvelle migration à la base de données. Étant donné que votre base de données n’existe pas encore, elle sera créée pour vous avant l’application de la migration.

> [!TIP]  
> Si vous apportez des modifications ultérieures au modèle, vous pouvez utiliser la commande `Add-Migration` pour générer automatiquement un nouveau modèle de migration pour appliquer les modifications du schéma correspondantes à la base de données. Après avoir vérifié le code de modèle généré automatiquement (et effectué toutes les modifications nécessaires), vous pouvez utiliser la commande `Update-Database` pour appliquer les modifications à la base de données.
>
>EF utilise une table `__EFMigrationsHistory` dans la base de données pour identifier les migrations qui ont déjà été appliquées à la base de données.

## <a name="use-your-model"></a>Utiliser le modèle

Vous pouvez à présent utiliser le modèle pour accéder aux données.

* Ouvrez *Program.cs*.

* Remplacez le contenu du fichier par le code suivant.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
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

![image](_static/output-new-db.png)
