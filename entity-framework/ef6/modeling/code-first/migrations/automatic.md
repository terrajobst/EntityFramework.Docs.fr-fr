---
title: Automatique les Migrations Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283912"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="6daf1-102">Automatique de Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="6daf1-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="6daf1-103">Les Migrations automatiques vous permet d’utiliser Code First Migrations sans qu’un fichier de code dans votre projet pour chaque modification apportée.</span><span class="sxs-lookup"><span data-stu-id="6daf1-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="6daf1-104">Toutes les modifications peuvent être appliquées automatiquement, par exemple renomme de colonne requiert l’utilisation d’une migration de type de code.</span><span class="sxs-lookup"><span data-stu-id="6daf1-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="6daf1-105">Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base.</span><span class="sxs-lookup"><span data-stu-id="6daf1-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="6daf1-106">Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="6daf1-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="6daf1-107">Recommandation pour les environnements d’équipe</span><span class="sxs-lookup"><span data-stu-id="6daf1-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="6daf1-108">Vous pouvez intercaler migrations automatiques et de code, mais cela n’est pas recommandée dans les scénarios de développement d’équipe.</span><span class="sxs-lookup"><span data-stu-id="6daf1-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="6daf1-109">Si vous faites partie d’une équipe de développeurs qui utilisent le contrôle de code source vous devez utiliser les migrations automatiques purement ou purement basée sur les migrations.</span><span class="sxs-lookup"><span data-stu-id="6daf1-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="6daf1-110">Étant donné les limitations de migrations automatiques, nous recommandons à l’aide des migrations basées sur le code dans les environnements d’équipe.</span><span class="sxs-lookup"><span data-stu-id="6daf1-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="6daf1-111">Génération d’un modèle et d’une base de données initiaux</span><span class="sxs-lookup"><span data-stu-id="6daf1-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="6daf1-112">Avant de commencer à utiliser les migrations, nous avons besoin d’un projet et d’un modèle Code First.</span><span class="sxs-lookup"><span data-stu-id="6daf1-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="6daf1-113">Pour cette procédure pas à pas nous utilisons le modèle canonique **Blog** et **Post**.</span><span class="sxs-lookup"><span data-stu-id="6daf1-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="6daf1-114">Créer un nouveau **MigrationsAutomaticDemo** application Console</span><span class="sxs-lookup"><span data-stu-id="6daf1-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="6daf1-115">Ajoutez la dernière version du package NuGet **EntityFramework** au projet</span><span class="sxs-lookup"><span data-stu-id="6daf1-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="6daf1-116">**Outils -&gt; Gestionnaire de package de bibliothèque -&gt; Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="6daf1-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="6daf1-117">Exécutez la commande **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="6daf1-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="6daf1-118">Ajoutez un fichier **Model.cs** avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6daf1-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="6daf1-119">Ce code définit une seule classe **Blog** qui constitue notre modèle de domaine et une classe **BlogContext** qui est notre contexte EF Code First</span><span class="sxs-lookup"><span data-stu-id="6daf1-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="6daf1-120">Maintenant que nous avons un modèle, utilisons-le pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="6daf1-121">Mettez à jour le fichier **Program.cs** avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6daf1-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="6daf1-122">Exécutez votre application et vous verrez qu’un **MigrationsAutomaticCodeDemo.BlogContext** base de données est créée pour vous.</span><span class="sxs-lookup"><span data-stu-id="6daf1-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Base de données de base de données locale](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="6daf1-124">Activation des migrations</span><span class="sxs-lookup"><span data-stu-id="6daf1-124">Enabling Migrations</span></span>

<span data-ttu-id="6daf1-125">Modifions un peu plus notre modèle.</span><span class="sxs-lookup"><span data-stu-id="6daf1-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="6daf1-126">Nous introduisons une propriété Url à la classe Blog.</span><span class="sxs-lookup"><span data-stu-id="6daf1-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="6daf1-127">Si vous réexécutez l’application, vous obtenez une exception InvalidOperationException indiquant *Le modèle permettant la sauvegarde du contexte 'BlogContext' a changé depuis la création de la base de données. Utilisez Migrations Code First pour mettre à jour la base de données (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span><span class="sxs-lookup"><span data-stu-id="6daf1-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="6daf1-128">Comme le suggère l’exception, utilisons donc Migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="6daf1-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="6daf1-129">Étant donné que nous souhaitons utiliser les migrations automatiques, nous allons spécifier le **– EnableAutomaticMigrations** basculer.</span><span class="sxs-lookup"><span data-stu-id="6daf1-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="6daf1-130">Exécutez le **Enable-Migrations – EnableAutomaticMigrations** commande dans le Package Manager Console cette commande a ajouté un **Migrations** dossier à notre projet.</span><span class="sxs-lookup"><span data-stu-id="6daf1-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="6daf1-131">Ce nouveau dossier contient un fichier :</span><span class="sxs-lookup"><span data-stu-id="6daf1-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="6daf1-132">**La classe Configuration.**</span><span class="sxs-lookup"><span data-stu-id="6daf1-132">**The Configuration class.**</span></span> <span data-ttu-id="6daf1-133">Cette classe vous permet de configurer le comportement des migrations pour votre contexte.</span><span class="sxs-lookup"><span data-stu-id="6daf1-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="6daf1-134">Pour cette procédure pas à pas, nous utilisons simplement la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="6daf1-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="6daf1-135">*Comme il n’y a qu’un seul contexte Code First dans votre projet, Enable-Migrations a automatiquement renseigné le type de contexte auquel s’applique cette configuration.*</span><span class="sxs-lookup"><span data-stu-id="6daf1-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="6daf1-136">Votre première Migration automatique</span><span class="sxs-lookup"><span data-stu-id="6daf1-136">Your First Automatic Migration</span></span>

<span data-ttu-id="6daf1-137">Migrations Code First a deux commandes principales que nous allons découvrir maintenant.</span><span class="sxs-lookup"><span data-stu-id="6daf1-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="6daf1-138">**Add-Migration** génère automatiquement la prochaine migration en fonction des changements de votre modèle depuis la création de la dernière migration</span><span class="sxs-lookup"><span data-stu-id="6daf1-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="6daf1-139">**Update-Database** applique toutes les migrations en attente à la base de données</span><span class="sxs-lookup"><span data-stu-id="6daf1-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="6daf1-140">Nous allons afin d’éviter à l’aide de Add-Migration (sauf s’il faut) et de vous concentrer sur ce qui permet des Migrations Code First automatiquement calculer et appliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="6daf1-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="6daf1-141">Nous allons utiliser **Update-Database** pour obtenir les Migrations Code First pour transmettre les modifications apportées à notre modèle (le nouveau **Blog.Ur**propriété de l) à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="6daf1-142">Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="6daf1-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="6daf1-143">Le **MigrationsAutomaticDemo.BlogContext** base de données est désormais mis à jour pour inclure le **Url** colonne dans le **Blogs** table.</span><span class="sxs-lookup"><span data-stu-id="6daf1-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="6daf1-144">Votre seconde Migration automatique</span><span class="sxs-lookup"><span data-stu-id="6daf1-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="6daf1-145">Nous allons effectuer une autre modifier et indiquer les Migrations Code First à transmettre automatiquement les modifications apportées à la base de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="6daf1-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="6daf1-146">Ajoutons aussi une nouvelle classe **Post**</span><span class="sxs-lookup"><span data-stu-id="6daf1-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="6daf1-147">Nous allons aussi ajouter une collection **Posts** à la classe **Blog** pour former l’autre extrémité de la relation entre **Blog** et **Post**</span><span class="sxs-lookup"><span data-stu-id="6daf1-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="6daf1-148">À présent utiliser **Update-Database** pour mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="6daf1-149">Cette fois, nous spécifions l’indicateur **–Verbose** pour pouvoir voir le code SQL que Migrations Code First exécute.</span><span class="sxs-lookup"><span data-stu-id="6daf1-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="6daf1-150">Exécutez la commande **Update-Database –Verbose** dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="6daf1-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="6daf1-151">Ajout d’un Code en fonction de Migration</span><span class="sxs-lookup"><span data-stu-id="6daf1-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="6daf1-152">Maintenant examinons quelque chose que nous pourrions utiliser une migration basée sur le code pour.</span><span class="sxs-lookup"><span data-stu-id="6daf1-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="6daf1-153">Nous allons ajouter un **évaluation** propriété le **Blog** classe</span><span class="sxs-lookup"><span data-stu-id="6daf1-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="6daf1-154">Nous pourrions simplement exécuter **Update-Database** pour envoyer ces modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="6daf1-155">Toutefois, nous ajoutons un non-nullable **Blogs.Rating** colonne, s’il existe toutes les données existantes dans la table de la valeur par défaut CLR du type de données pour la nouvelle colonne il est affectée (contrôle d’accès est entier, ce qui constitue **0**).</span><span class="sxs-lookup"><span data-stu-id="6daf1-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="6daf1-156">Toutefois, nous voulons spécifier une valeur par défaut égale à **3** pour que les lignes existantes de la table **Blogs** commencent avec un classement correct.</span><span class="sxs-lookup"><span data-stu-id="6daf1-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="6daf1-157">Nous allons utiliser la commande Add-Migration pour écrire cette modification out pour une migration basée sur le code afin que nous pouvons le modifier.</span><span class="sxs-lookup"><span data-stu-id="6daf1-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="6daf1-158">Le **Add-Migration** commande permet de nommer ces migrations, appelons nôtre **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="6daf1-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="6daf1-159">Exécutez le **Add-Migration AddBlogRating** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="6daf1-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="6daf1-160">Dans le **Migrations** dossier nous disposons désormais d’un nouveau **AddBlogRating** migration.</span><span class="sxs-lookup"><span data-stu-id="6daf1-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="6daf1-161">Le nom de fichier de migration est déjà résolu avec un horodatage pour faciliter le classement.</span><span class="sxs-lookup"><span data-stu-id="6daf1-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="6daf1-162">Nous allons modifier le code généré pour spécifier une valeur par défaut de 3 pour Blog.Rating (ligne 10 dans le code ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="6daf1-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="6daf1-163">*La migration a également un fichier code-behind qui capture des métadonnées. Ces métadonnées permettra de Migrations Code First répliquer les migrations automatiques, que nous avons effectué avant cette migration basée sur le code. Ceci est important si un autre développeur souhaite exécuter notre migrations ou lorsqu’il est temps de déployer notre application.*</span><span class="sxs-lookup"><span data-stu-id="6daf1-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="6daf1-164">Notre migration modifiée est prête, utilisons **Update-Database** pour mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="6daf1-165">Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="6daf1-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="6daf1-166">Vers les Migrations automatiques</span><span class="sxs-lookup"><span data-stu-id="6daf1-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="6daf1-167">Nous sommes maintenant libres de revenir à des migrations automatiques pour nos modifications plus simples.</span><span class="sxs-lookup"><span data-stu-id="6daf1-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="6daf1-168">Code First Migrations se chargera d’effectuer les migrations automatiques et basée sur le code dans l’ordre approprié en fonction des métadonnées stockées dans le fichier code-behind pour chaque migration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="6daf1-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="6daf1-169">Nous allons ajouter une propriété Post.Abstract à notre modèle</span><span class="sxs-lookup"><span data-stu-id="6daf1-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="6daf1-170">Maintenant, nous pouvons utiliser **Update-Database** pour obtenir les Migrations Code First pour envoyer cette modification à la base de données à l’aide d’une migration automatique.</span><span class="sxs-lookup"><span data-stu-id="6daf1-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="6daf1-171">Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="6daf1-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="6daf1-172">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="6daf1-172">Summary</span></span>

<span data-ttu-id="6daf1-173">Dans cette procédure pas à pas, que vous avez vu comment utiliser les migrations automatiques pour transmettre le modèle change à la base de données.</span><span class="sxs-lookup"><span data-stu-id="6daf1-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="6daf1-174">Vous avez également vu comment structurer et exécutez les migrations de base de code entre les migrations automatiques lorsque vous avez besoin de davantage de contrôle.</span><span class="sxs-lookup"><span data-stu-id="6daf1-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
