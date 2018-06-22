---
title: Bien démarrer avec ASP.NET Core - Base de données existante - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151012"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Bien démarrer avec EF Core sur ASP.NET Core avec une base de données existante

Dans cette procédure pas à pas, vous allez générer une application ASP.NET Core MVC exécutant l’accès aux données de base à l’aide d’Entity Framework. Vous allez utiliser l’ingénierie à rebours pour créer un modèle Entity Framework à partir d’une base de données existante.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) sur GitHub.

## <a name="prerequisites"></a>Prérequis

Pour effectuer cette procédure pas à pas, vous devez satisfaire les prérequis suivants :

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) avec les charges de travail suivantes :
  * **ASP.NET et développement web** (sous **Web et cloud**)
  * **Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)
* [Kit SDK .NET Core 2.0](https://www.microsoft.com/net/download/core)
* [Base de données de création de blogs](#blogging-database)

### <a name="blogging-database"></a>Base de données de création de blogs

Ce didacticiel utilise une base de données de **création de blogs** sur votre instance LocalDb en tant que base de données existante.

> [!TIP]  
> Si vous avez déjà créé la base de données de **création de blogs** dans le cadre d’un autre didacticiel, vous pouvez ignorer ces étapes.

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
* Dans le menu de gauche, sélectionnez **Installé > Modèles > Visual C# > Web**.
* Sélectionnez le modèle de projet **Application web ASP.NET Core (.NET Core)**.
* Entrez le nom **EFGetStarted.AspNetCore.ExistingDb** et cliquez sur **OK**.
* Patientez jusqu’à l’affichage de la boîte de dialogue **Nouvelle application web ASP.NET Core**.
* Dans la liste de **modèles ASP.NET Core 2.0**, sélectionnez le modèle de projet **Application web (Model-View-Controller)**.
* Vérifiez que le paramètre **Authentification** est défini sur **Aucune authentification**.
* Cliquez sur **OK**.

## <a name="install-entity-framework"></a>Installer Entity Framework

Pour utiliser EF Core, installez le package pour le ou les fournisseurs de bases de données à cibler. Cette procédure pas à pas utilise SQL Server. Pour obtenir la liste des fournisseurs disponibles, consultez [Fournisseurs de bases de données](../../providers/index.md).

* **Outils > Gestionnaire de package NuGet > Console du Gestionnaire de package**

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Nous utiliserons des outils Entity Framework pour créer un modèle à partir de la base de données. Nous installerons donc également le package d’outils :

* Exécutez `Install-Package Microsoft.EntityFrameworkCore.Tools`.

Nous utiliserons des outils de génération de modèles automatique ASP.NET Core pour créer par la suite des contrôleurs et des vues. Nous installerons donc également ce package de conception :

* Exécutez `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.

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

 Les classes d’entité sont des objets C# simples qui représentent les données que vous allez interroger et enregistrer.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Le contexte représente une session avec la base de données et vous permet d’interroger et d’enregistrer les instances des classes d’entité.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

### <a name="remove-inline-context-configuration"></a>Supprimer la configuration contextuelle inline

Dans ASP.NET Core, la configuration s’effectue généralement dans **Startup.cs**. Pour respecter ce modèle, nous allons transférer la configuration du fournisseur de base de données dans **Startup.cs**.

* Ouvrez `Models\BloggingContext.cs`.
* Supprimez la méthode `OnConfiguring(...)`.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Ajoutez le constructeur suivant pour pouvoir transférer la configuration dans le contexte par injection de dépendances.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Inscrire et configurer le contexte dans Startup.cs

Afin de permettre à nos contrôleurs MVC d’utiliser `BloggingContext`, nous allons inscrire ce dernier en tant que service.

* Ouvrez **Startup.cs**.
* Ajoutez les instructions `using` suivantes au début du fichier.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Nous pouvons à présent utiliser la méthode `AddDbContext(...)` pour l’inscrire en tant que service.
* Localisez la méthode `ConfigureServices(...)`.
* Ajoutez le code suivant pour inscrire le contexte en tant que service.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> Dans une application réelle, vous placeriez généralement la chaîne de connexion dans un fichier de configuration. Par souci de simplicité, nous allons la définir dans le code. Pour plus d’informations, consultez [Chaînes de connexion](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Créer un contrôleur

Nous allons à présent activer la génération de modèles automatique dans notre projet.

* Dans l’**Explorateur de solutions**, cliquez avec le bouton de droite sur le dossier **Contrôleurs** et sélectionnez **Ajouter -> Contrôleur...**
* Sélectionnez **Dépendances complètes** et cliquez sur **Ajouter**.
* Vous pouvez ignorer les instructions du fichier `ScaffoldingReadMe.txt` qui s’ouvre.

Une fois activée la génération de modèles automatique, nous pouvons générer automatiquement un modèle de contrôleur pour l’entité `Blog`.

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

![image](_static/create.png)

![image](_static/index-existing-db.png)
