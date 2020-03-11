---
title: Migrations Code First automatique-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418999"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="e9a5c-102">Migrations Code First automatique</span><span class="sxs-lookup"><span data-stu-id="e9a5c-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="e9a5c-103">La migration automatique vous permet d’utiliser Migrations Code First sans avoir de fichier de code dans votre projet pour chaque modification que vous apportez.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="e9a5c-104">Toutes les modifications ne peuvent pas être appliquées automatiquement. par exemple, les renommages de colonne nécessitent l’utilisation d’une migration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="e9a5c-105">Cet article suppose que vous savez utiliser Migrations Code First dans les scénarios de base.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="e9a5c-106">Si ce n’est pas le cas, vous devez lire [migrations code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="e9a5c-107">Recommandation pour les environnements d’équipe</span><span class="sxs-lookup"><span data-stu-id="e9a5c-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="e9a5c-108">Vous pouvez intercaler des migrations automatiques et basées sur du code, mais cela n’est pas recommandé dans les scénarios de développement d’équipe.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="e9a5c-109">Si vous faites partie d’une équipe de développeurs qui utilisent le contrôle de code source, vous devez utiliser des migrations purement automatiques ou des migrations purement basées sur le code.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="e9a5c-110">Étant donné les limitations des migrations automatiques, nous vous recommandons d’utiliser des migrations basées sur du code dans des environnements d’équipe.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="e9a5c-111">Génération d’un modèle et d’une base de données initiaux</span><span class="sxs-lookup"><span data-stu-id="e9a5c-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="e9a5c-112">Avant de commencer à utiliser les migrations, nous avons besoin d’un projet et d’un modèle Code First.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="e9a5c-113">Pour cette procédure pas à pas nous utilisons le modèle canonique **Blog** et **Post**.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="e9a5c-114">Créer une application console **MigrationsAutomaticDemo**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="e9a5c-115">Ajoutez la dernière version du package NuGet **EntityFramework** au projet</span><span class="sxs-lookup"><span data-stu-id="e9a5c-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="e9a5c-116">**Outils -&gt; Gestionnaire de package de bibliothèque -&gt; Console du Gestionnaire de package**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="e9a5c-117">Exécutez la commande **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="e9a5c-118">Ajoutez un fichier **Model.cs** avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="e9a5c-119">Ce code définit une seule classe **Blog** qui constitue notre modèle de domaine et une classe **BlogContext** qui est notre contexte EF Code First</span><span class="sxs-lookup"><span data-stu-id="e9a5c-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="e9a5c-120">Maintenant que nous avons un modèle, utilisons-le pour accéder aux données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="e9a5c-121">Mettez à jour le fichier **Program.cs** avec le code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="e9a5c-122">Exécutez votre application et vous verrez qu’une base de données **MigrationsAutomaticCodeDemo. BlogContext** est créée pour vous.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Base de données locale](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="e9a5c-124">Activation des migrations</span><span class="sxs-lookup"><span data-stu-id="e9a5c-124">Enabling Migrations</span></span>

<span data-ttu-id="e9a5c-125">Modifions un peu plus notre modèle.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="e9a5c-126">Nous introduisons une propriété Url à la classe Blog.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="e9a5c-127">Si vous deviez réexécuter l’application, vous obtiendrez une exception InvalidOperationException indiquant que le *modèle sauvegardant le contexte’BlogContext’a changé depuis la création de la base de données. Envisagez d’utiliser Migrations Code First pour mettre à jour la base de données (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="e9a5c-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="e9a5c-128">Comme le suggère l’exception, utilisons donc Migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="e9a5c-129">Étant donné que nous voulons utiliser des migrations automatiques, nous allons spécifier le commutateur **– EnableAutomaticMigrations** .</span><span class="sxs-lookup"><span data-stu-id="e9a5c-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="e9a5c-130">Exécutez la commande **Enable-migrations-EnableAutomaticMigrations** dans la console du gestionnaire de package. cette commande a ajouté un dossier **migrations** à notre projet.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="e9a5c-131">Ce nouveau dossier contient un fichier :</span><span class="sxs-lookup"><span data-stu-id="e9a5c-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="e9a5c-132">**La classe Configuration.**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-132">**The Configuration class.**</span></span> <span data-ttu-id="e9a5c-133">Cette classe vous permet de configurer le comportement des migrations pour votre contexte.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="e9a5c-134">Pour cette procédure pas à pas, nous utilisons simplement la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="e9a5c-135">*Comme il n’y a qu’un seul contexte Code First dans votre projet, Enable-Migrations a automatiquement renseigné le type de contexte auquel s’applique cette configuration.*</span><span class="sxs-lookup"><span data-stu-id="e9a5c-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="e9a5c-136">Votre première migration automatique</span><span class="sxs-lookup"><span data-stu-id="e9a5c-136">Your First Automatic Migration</span></span>

<span data-ttu-id="e9a5c-137">Migrations Code First a deux commandes principales que nous allons découvrir maintenant.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="e9a5c-138">**Add-Migration** génère automatiquement la prochaine migration en fonction des changements de votre modèle depuis la création de la dernière migration</span><span class="sxs-lookup"><span data-stu-id="e9a5c-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="e9a5c-139">**Update-Database** applique toutes les migrations en attente à la base de données</span><span class="sxs-lookup"><span data-stu-id="e9a5c-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="e9a5c-140">Nous allons éviter d’utiliser Add-migration (sauf si cela est vraiment nécessaire) et de se concentrer sur la possibilité Migrations Code First de calculer et d’appliquer automatiquement les modifications.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="e9a5c-141">Nous allons utiliser **Update-Database** pour récupérer migrations code First pour transmettre les modifications apportées à notre modèle (la nouvelle propriété **blog. ur**l) à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="e9a5c-142">Exécutez la commande **Update-Database** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="e9a5c-143">La base de données **MigrationsAutomaticDemo. BlogContext** est maintenant mise à jour pour inclure la colonne **URL** dans la table **blogs** .</span><span class="sxs-lookup"><span data-stu-id="e9a5c-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="e9a5c-144">Votre deuxième migration automatique</span><span class="sxs-lookup"><span data-stu-id="e9a5c-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="e9a5c-145">Nous allons effectuer une autre modification et laisser Migrations Code First envoyer automatiquement les modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="e9a5c-146">Ajoutons aussi une nouvelle classe **Post**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="e9a5c-147">Nous allons aussi ajouter une collection **Posts** à la classe **Blog** pour former l’autre extrémité de la relation entre **Blog** et **Post**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="e9a5c-148">Utilisez **à présent Update-Database** pour mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="e9a5c-149">Cette fois, nous spécifions l’indicateur **–Verbose** pour pouvoir voir le code SQL que Migrations Code First exécute.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="e9a5c-150">Exécutez la commande **Update-Database –Verbose** dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="e9a5c-151">Ajout d’une migration basée sur du code</span><span class="sxs-lookup"><span data-stu-id="e9a5c-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="e9a5c-152">Examinons maintenant ce que nous pourrions utiliser pour la migration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="e9a5c-153">Nous allons ajouter une propriété **Rating** à la classe **blog**</span><span class="sxs-lookup"><span data-stu-id="e9a5c-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="e9a5c-154">Nous pourrions simplement exécuter **Update-Database** pour envoyer ces modifications à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="e9a5c-155">Toutefois, nous ajoutons une colonne blogs qui n’accepte pas les valeurs null **.** si des données existent dans la table, elles reçoivent la valeur CLR par défaut du type de données de la nouvelle colonne (l’évaluation est un entier, donc **0**).</span><span class="sxs-lookup"><span data-stu-id="e9a5c-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="e9a5c-156">Toutefois, nous voulons spécifier une valeur par défaut égale à **3** pour que les lignes existantes de la table **Blogs** commencent avec un classement correct.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="e9a5c-157">Nous allons utiliser la commande Add-migration pour écrire cette modification dans une migration basée sur du code afin de pouvoir la modifier.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="e9a5c-158">La commande **Add-migration** nous permet d’attribuer un nom à ces migrations, nous appelons simplement le nôtre **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="e9a5c-159">Exécutez la commande **Add-migration AddBlogRating** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="e9a5c-160">Dans le dossier **migrations** , nous disposons désormais d’une nouvelle migration **AddBlogRating** .</span><span class="sxs-lookup"><span data-stu-id="e9a5c-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="e9a5c-161">Le nom du fichier de migration est précédé d’un horodateur pour faciliter le classement.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="e9a5c-162">Modifions le code généré pour spécifier une valeur par défaut de 3 pour blog. Rating (ligne 10 dans le code ci-dessous)</span><span class="sxs-lookup"><span data-stu-id="e9a5c-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="e9a5c-163">*La migration a également un fichier code-behind qui capture certaines métadonnées. Ces métadonnées permettront à Migrations Code First de répliquer les migrations automatiques que nous avons effectuées avant cette migration basée sur le code. Cela est important si un autre développeur souhaite exécuter nos migrations ou quand il est temps de déployer notre application.*</span><span class="sxs-lookup"><span data-stu-id="e9a5c-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="e9a5c-164">Notre migration modifiée est prête, utilisons **Update-Database** pour mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="e9a5c-165">Exécutez la commande **Update-Database** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="e9a5c-166">Retour aux migrations automatiques</span><span class="sxs-lookup"><span data-stu-id="e9a5c-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="e9a5c-167">Nous sommes maintenant libres de revenir aux migrations automatiques pour nos modifications plus simples.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="e9a5c-168">Migrations Code First s’occupera des migrations automatiques et basées sur du code dans l’ordre correct en fonction des métadonnées qu’il stocke dans le fichier code-behind pour chaque migration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="e9a5c-169">Nous allons ajouter une propriété poster. Abstract à notre modèle</span><span class="sxs-lookup"><span data-stu-id="e9a5c-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="e9a5c-170">À présent, nous pouvons utiliser **Update-Database** pour savoir migrations code First transmettre cette modification à la base de données à l’aide d’une migration automatique.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="e9a5c-171">Exécutez la commande **Update-Database** dans la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="e9a5c-172">Résumé</span><span class="sxs-lookup"><span data-stu-id="e9a5c-172">Summary</span></span>

<span data-ttu-id="e9a5c-173">Dans cette procédure pas à pas, vous avez vu comment utiliser des migrations automatiques pour transmettre des modifications de modèle à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="e9a5c-174">Vous avez également vu comment structurer et exécuter des migrations basées sur du code entre des migrations automatiques lorsque vous avez besoin de davantage de contrôle.</span><span class="sxs-lookup"><span data-stu-id="e9a5c-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
