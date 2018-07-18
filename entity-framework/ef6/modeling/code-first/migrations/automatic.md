---
title: Automatique les Migrations Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121115"
---
# <a name="automatic-code-first-migrations"></a>Automatique de Code First Migrations
Les Migrations automatiques vous permet d’utiliser Code First Migrations sans qu’un fichier de code dans votre projet pour chaque modification apportée. Toutes les modifications peuvent être appliquées automatiquement, par exemple renomme de colonne requiert l’utilisation d’une migration de type de code.

> [!NOTE]
> Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base. Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.

## <a name="recommendation-for-team-environments"></a>Recommandation pour les environnements d’équipe

Vous pouvez intercaler migrations automatiques et de code, mais cela n’est pas recommandée dans les scénarios de développement d’équipe. Si vous faites partie d’une équipe de développeurs qui utilisent le contrôle de code source vous devez utiliser les migrations automatiques purement ou purement basée sur les migrations. Étant donné les limitations de migrations automatiques, nous recommandons à l’aide des migrations basées sur le code dans les environnements d’équipe.

## <a name="building-an-initial-model--database"></a>Génération d’un modèle et d’une base de données initiaux

Avant de commencer à utiliser les migrations, nous avons besoin d’un projet et d’un modèle Code First. Pour cette procédure pas à pas nous utilisons le modèle canonique **Blog** et **Post**.

-   Créer un nouveau **MigrationsAutomaticDemo** application Console
-   Ajoutez la dernière version du package NuGet **EntityFramework** au projet
    -   **Outils -&gt; Gestionnaire de package de bibliothèque -&gt; Console du Gestionnaire de package**
    -   Exécutez la commande **Install-Package EntityFramework**
-   Ajoutez un fichier **Model.cs** avec le code indiqué ci-dessous. Ce code définit une seule classe **Blog** qui constitue notre modèle de domaine et une classe **BlogContext** qui est notre contexte EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Maintenant que nous avons un modèle, utilisons-le pour accéder aux données. Mettez à jour le fichier **Program.cs** avec le code indiqué ci-dessous.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Exécutez votre application et vous verrez qu’un **MigrationsAutomaticCodeDemo.BlogContext** base de données est créée pour vous.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Activation des migrations

Modifions un peu plus notre modèle.

-   Nous introduisons une propriété Url à la classe Blog.

``` csharp
    public string Url { get; set; }
```

Si vous réexécutez l’application, vous obtenez une exception InvalidOperationException indiquant *Le modèle permettant la sauvegarde du contexte 'BlogContext' a changé depuis la création de la base de données. Utilisez Migrations Code First pour mettre à jour la base de données (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

Comme le suggère l’exception, utilisons donc Migrations Code First. Étant donné que nous souhaitons utiliser les migrations automatiques, nous allons spécifier le **– EnableAutomaticMigrations** basculer.

-   Exécutez le **Enable-Migrations – EnableAutomaticMigrations** commande dans le Package Manager Console cette commande a ajouté un **Migrations** dossier à notre projet. Ce nouveau dossier contient un fichier :

-   **La classe Configuration.** Cette classe vous permet de configurer le comportement des migrations pour votre contexte. Pour cette procédure pas à pas, nous utilisons simplement la configuration par défaut.
    *Comme il n’y a qu’un seul contexte Code First dans votre projet, Enable-Migrations a automatiquement renseigné le type de contexte auquel s’applique cette configuration.*

 

## <a name="your-first-automatic-migration"></a>Votre première Migration automatique

Migrations Code First a deux commandes principales que nous allons découvrir maintenant.

-   **Add-Migration** génère automatiquement la prochaine migration en fonction des changements de votre modèle depuis la création de la dernière migration
-   **Update-Database** applique toutes les migrations en attente à la base de données

Nous allons afin d’éviter à l’aide de Add-Migration (sauf s’il faut) et de vous concentrer sur ce qui permet des Migrations Code First automatiquement calculer et appliquer les modifications. Nous allons utiliser **Update-Database** pour obtenir les Migrations Code First pour transmettre les modifications apportées à notre modèle (le nouveau **Blog.Ur**propriété de l) à la base de données.

-   Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.

Le **MigrationsAutomaticDemo.BlogContext** base de données est désormais mis à jour pour inclure le **Url** colonne dans le **Blogs** table.

 

## <a name="your-second-automatic-migration"></a>Votre seconde Migration automatique

Nous allons effectuer une autre modifier et indiquer les Migrations Code First à transmettre automatiquement les modifications apportées à la base de données pour nous.

-   Ajoutons aussi une nouvelle classe **Post**

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Nous allons aussi ajouter une collection **Posts** à la classe **Blog** pour former l’autre extrémité de la relation entre **Blog** et **Post**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

À présent utiliser **Update-Database** pour mettre à jour de la base de données. Cette fois, nous spécifions l’indicateur **–Verbose** pour pouvoir voir le code SQL que Migrations Code First exécute.

-   Exécutez la commande **Update-Database –Verbose** dans la Console du Gestionnaire de Package.

## <a name="adding-a-code-based-migration"></a>Ajout d’un Code en fonction de Migration

Maintenant examinons quelque chose que nous pourrions utiliser une migration basée sur le code pour.

-   Nous allons ajouter un **évaluation** propriété le **Blog** classe

``` csharp
    public int Rating { get; set; }
```

Nous pourrions simplement exécuter **Update-Database** pour envoyer ces modifications à la base de données. Toutefois, nous ajoutons un non-nullable **Blogs.Rating** colonne, s’il existe toutes les données existantes dans la table de la valeur par défaut CLR du type de données pour la nouvelle colonne il est affectée (contrôle d’accès est entier, ce qui constitue **0**). Toutefois, nous voulons spécifier une valeur par défaut égale à **3** pour que les lignes existantes de la table **Blogs** commencent avec un classement correct.
Nous allons utiliser la commande Add-Migration pour écrire cette modification out pour une migration basée sur le code afin que nous pouvons le modifier. Le **Add-Migration** commande permet de nommer ces migrations, appelons nôtre **AddBlogRating**.

-   Exécutez le **Add-Migration AddBlogRating** commande dans la Console du Gestionnaire de Package.
-   Dans le **Migrations** dossier nous disposons désormais d’un nouveau **AddBlogRating** migration. Le nom de fichier de migration est déjà résolu avec un horodatage pour faciliter le classement. Nous allons modifier le code généré pour spécifier une valeur par défaut de 3 pour Blog.Rating (ligne 10 dans le code ci-dessous)

*La migration a également un fichier code-behind qui capture des métadonnées. Ces métadonnées permettra de Migrations Code First répliquer les migrations automatiques, que nous avons effectué avant cette migration basée sur le code. Ceci est important si un autre développeur souhaite exécuter notre migrations ou lorsqu’il est temps de déployer notre application.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Notre migration modifiée est prête, utilisons **Update-Database** pour mettre à jour la base de données.

-   Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.

## <a name="back-to-automatic-migrations"></a>Vers les Migrations automatiques

Nous sommes maintenant libres de revenir à des migrations automatiques pour nos modifications plus simples. Code First Migrations se chargera d’effectuer les migrations automatiques et basée sur le code dans l’ordre approprié en fonction des métadonnées stockées dans le fichier code-behind pour chaque migration basée sur le code.

-   Nous allons ajouter une propriété Post.Abstract à notre modèle

``` csharp
    public string Abstract { get; set; }
```

Maintenant, nous pouvons utiliser **Update-Database** pour obtenir les Migrations Code First pour envoyer cette modification à la base de données à l’aide d’une migration automatique.

-   Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, que vous avez vu comment utiliser les migrations automatiques pour transmettre le modèle change à la base de données. Vous avez également vu comment structurer et exécutez les migrations de base de code entre les migrations automatiques lorsque vous avez besoin de davantage de contrôle.
