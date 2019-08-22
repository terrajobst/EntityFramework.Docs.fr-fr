---
title: Bien démarrer avec ASP.NET Core - Base de données existante - EF Core
author: rowanmiller
description: Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565228"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante

Dans ce tutoriel, vous générez une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework Core. Vous rétroconcevez une base de données existante pour créer un modèle Entity Framework.

[Affichez l’exemple proposé dans cet article sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Prérequis

Installez les logiciels suivants :

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :
  * **ASP.NET et développement web** (sous **Web et cloud**)
  * **Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)
* [SDK .NET Core 2.1](https://www.microsoft.com/net/download/core)

## <a name="create-blogging-database"></a>Créer une base de données de création de blogs

Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante. Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre tutoriel, ignorez ces étapes.

* Ouvrir Visual Studio
* **Outils -> Connexion à une base de données...**
* Sélectionnez **Microsoft SQL Server** et cliquez sur **Continuer**.
* Entrez **(localdb)\mssqllocaldb** comme **nom du serveur**.
* Entrez **master** comme **nom de la base de données** et cliquez sur **OK**.
* La base de données master figure désormais sous **Connexions de données** dans l’**Explorateur de serveurs**.
* Cliquez avec le bouton de droite sur la base de données dans l’**Explorateur de serveurs** et sélectionnez l’option **Nouvelle requête**.
* Copiez le script ci-dessous dans l’éditeur de requêtes.
* Cliquez avec le bouton de droite sur l’éditeur de requêtes et sélectionnez l’option **Exécuter**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Créer un projet

* Ouvrez Visual Studio 2017.
* **Fichier > Nouveau > Projet...**
* Dans le menu de gauche, sélectionnez **Installé > Visual C# > Web**
* Sélectionnez le modèle de projet **Application web ASP.NET Core**.
* Entrez **EFGetStarted.AspNetCore.ExistingDb** comme nom (il doit correspondre exactement à l’espace de noms utilisé ultérieurement dans le code) et cliquez sur **OK** 
* Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.
* Vérifiez que la liste déroulante du framework cible a la valeur **.NET Core**, et que la liste déroulante de version a la valeur **ASP.NET Core 2.1**.
* Sélectionnez le modèle **Application web (Model-View-Controller)** .
* Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.
* Cliquez sur **OK**.

## <a name="install-entity-framework-core"></a>Installer Entity Framework Core

Pour installer EF Core, installez le package pour le ou les fournisseurs de bases de données EF Core à cibler. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md). 

Pour ce tutoriel, vous n’êtes pas obligé d’installer un package de fournisseur, car le tutoriel utilise SQL Server. Le package du fournisseur SQL Server est inclus dans le [métapackage Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Rétroconcevoir le modèle

Créons à présent le modèle EF à partir de votre base de données existante.

* **Outils –> Gestionnaire de package NuGet –> Console du Gestionnaire de package**
* Exécutez la commande suivante pour créer un modèle à partir de la base de données existante :

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Si vous recevez l’erreur `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, fermez et rouvrez Visual Studio.

> [!TIP]  
> Vous pouvez spécifier les tables pour lesquelles générer des entités en ajoutant l’argument `-Tables` à la commande ci-dessus. Par exemple, `-Tables Blog,Post`.

Le processus d’ingénierie à rebours créé des classes d’entité (`Blog.cs` & `Post.cs`) et un contexte dérivé (`BloggingContext.cs`) en fonction du schéma de la base de données existante.

 Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer. Voici les classes d’entité `Blog` et `Post` :

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Pour activer le chargement différé, vous pouvez créer des propriétés de navigation `virtual` (Blog.Post et Post.Blog).

 Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
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
}
```

## <a name="register-your-context-with-dependency-injection"></a>Inscrire le contexte avec l’injection de dépendances

Le concept d’injection de dépendances est essentiel dans ASP.NET Core. Des services (tels que `BloggingContext`) sont inscrits avec l’injection de dépendances au démarrage de l’application. Ces services sont affectés aux composants qui les nécessitent (par exemple, vos contrôleurs MVC) par le biais des propriétés ou paramètres du constructeur. Pour plus d’informations sur l’injection de dépendances, consultez l’article [Injection de dépendances](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) sur le site ASP.NET.

### <a name="register-and-configure-your-context-in-startupcs"></a>Inscrire et configurer le contexte dans Startup.cs

Pour rendre `BloggingContext` accessible aux contrôleurs MVC, inscrivez-le en tant que service.

* Ouvrez **Startup.cs**.
* Ajoutez les instructions `using` suivantes au début du fichier.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Vous pouvez à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.
* Localisez la méthode `ConfigureServices(...)`.
* Ajoutez le code en surbrillance suivant pour inscrire le contexte en tant que service.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> Dans une application réelle, vous placez généralement la chaîne de connexion dans un fichier de configuration ou une variable d’environnement. Par souci de simplicité, ce tutoriel la définit dans le code. Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Créer un contrôleur et des vues

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**
* Sélectionnez **Contrôleur MVC avec vues, utilisant Entity Framework** et cliquez sur **OK**.
* Définissez les paramètres **Classe de modèle** sur **Blog** et **Classe de contexte de données** sur **BloggingContext**.
* Cliquez sur **Ajouter**.

## <a name="run-the-application"></a>Exécuter l'application

Vous pouvez à présent exécuter l’application pour la voir en action.

* **Déboguer -> Exécuter sans débogage**
* L’application est générée et s’ouvre dans un navigateur web.
* Accédez à `/Blogs`.
* Cliquez sur **Créer nouveau**.
* Entrez l’**URL** du nouveau blog et cliquez sur **Créer**.

  ![Créer une page](_static/create.png)

  ![Page d’index](_static/index-existing-db.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la façon de structurer un contexte et des classes d’entité, consultez les articles suivants :
* [Reconstitution de la logique des produits](xref:core/managing-schemas/scaffolding)
* [Référence des outils Entity Framework Core - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Référence des outils Entity Framework Core - Console du Gestionnaire de Package](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
