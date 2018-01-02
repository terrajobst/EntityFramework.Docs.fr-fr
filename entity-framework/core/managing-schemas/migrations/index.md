---
title: Migrations - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 24fbe344eba9b99929d905ac2b9e49c68a1a4323
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
<a name="migrations"></a><span data-ttu-id="c6347-102">Migrations</span><span class="sxs-lookup"><span data-stu-id="c6347-102">Migrations</span></span>
==========
<span data-ttu-id="c6347-103">Les migrations permettent d’appliquer de façon incrémentielle des modifications de schéma à la base de données pour qu’elle demeure synchronisée avec votre modèle EF Core tout en conservant les données existantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6347-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="c6347-104">Création de la base de données</span><span class="sxs-lookup"><span data-stu-id="c6347-104">Creating the database</span></span>
---------------------
<span data-ttu-id="c6347-105">Une fois que vous avez [défini votre modèle initial][1], il est temps de créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6347-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="c6347-106">Pour ce faire, ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="c6347-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="c6347-107">Installez les [outils EF Core][2] et exécutez la commande appropriée.</span><span class="sxs-lookup"><span data-stu-id="c6347-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c6347-108">Trois fichiers sont ajoutés à votre projet sous le répertoire **Migrations** :</span><span class="sxs-lookup"><span data-stu-id="c6347-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="c6347-109">**00000000000000_InitialCreate.cs** : fichier principal des migrations.</span><span class="sxs-lookup"><span data-stu-id="c6347-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="c6347-110">Contient les opérations nécessaires à l’application de la migration (dans `Up()`) et à sa restauration (dans `Down()`).</span><span class="sxs-lookup"><span data-stu-id="c6347-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="c6347-111">**00000000000000_InitialCreate.Designer.cs** : fichier de métadonnées des migrations.</span><span class="sxs-lookup"><span data-stu-id="c6347-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="c6347-112">Contient des informations utilisées par EF.</span><span class="sxs-lookup"><span data-stu-id="c6347-112">Contains information used by EF.</span></span>
* <span data-ttu-id="c6347-113">**MyContextModelSnapshot.cs** : instantané de votre modèle actuel.</span><span class="sxs-lookup"><span data-stu-id="c6347-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="c6347-114">Permet de déterminer ce qui a changé pendant l’ajout de la migration suivante.</span><span class="sxs-lookup"><span data-stu-id="c6347-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="c6347-115">L’horodatage dans le nom des fichiers permet de conserver ces derniers dans l’ordre chronologique et de voir ainsi la progression des modifications.</span><span class="sxs-lookup"><span data-stu-id="c6347-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="c6347-116">Vous êtes libre de déplacer les fichiers Migrations et de changer leur espace de noms.</span><span class="sxs-lookup"><span data-stu-id="c6347-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="c6347-117">Les migrations sont créées en tant que sœurs de la dernière migration.</span><span class="sxs-lookup"><span data-stu-id="c6347-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="c6347-118">Ensuite, appliquez la migration à la base de données pour créer le schéma.</span><span class="sxs-lookup"><span data-stu-id="c6347-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="c6347-119">Ajout d’une autre migration</span><span class="sxs-lookup"><span data-stu-id="c6347-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="c6347-120">Une fois votre modèle EF Core modifié, le schéma de base de données n’est plus synchronisé. Pour le mettre à jour, ajoutez une autre migration.</span><span class="sxs-lookup"><span data-stu-id="c6347-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="c6347-121">Le nom de la migration peut être utilisé comme un message de validation dans un système de gestion de versions.</span><span class="sxs-lookup"><span data-stu-id="c6347-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="c6347-122">Par exemple, si j’ai apporté des modifications pour enregistrer des avis de clients sur des produits, je peux choisir quelque chose comme *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="c6347-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="c6347-123">Une fois la migration structurée, vous devez vérifier sa précision et ajouter toutes les opérations supplémentaires nécessaires à son application.</span><span class="sxs-lookup"><span data-stu-id="c6347-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="c6347-124">Par exemple, la migration peut contenir les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="c6347-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="c6347-125">Bien que ces opérations rendent le schéma de base de données compatible, elles ne conservent pas les noms de client existants.</span><span class="sxs-lookup"><span data-stu-id="c6347-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="c6347-126">Pour l’améliorer, réécrivez la migration comme suit.</span><span class="sxs-lookup"><span data-stu-id="c6347-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="c6347-127">L’ajout d’une nouvelle migration émet un avertissement si la structuration d’une opération risque d’entraîner une perte de données (telle que la suppression d’une colonne).</span><span class="sxs-lookup"><span data-stu-id="c6347-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="c6347-128">Veillez à vérifier tout particulièrement la précision de ces migrations.</span><span class="sxs-lookup"><span data-stu-id="c6347-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="c6347-129">Appliquez la migration à la base de données à l’aide de la commande appropriée.</span><span class="sxs-lookup"><span data-stu-id="c6347-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="c6347-130">Suppression d’une migration</span><span class="sxs-lookup"><span data-stu-id="c6347-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="c6347-131">Parfois, vous ajoutez une migration et réalisez que vous devez apporter des modifications supplémentaires à votre modèle EF Core avant de l’appliquer.</span><span class="sxs-lookup"><span data-stu-id="c6347-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="c6347-132">Pour supprimer la dernière migration, utilisez cette commande.</span><span class="sxs-lookup"><span data-stu-id="c6347-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="c6347-133">Après l’avoir supprimée, vous pouvez apporter les modifications supplémentaires au modèle et la rajouter.</span><span class="sxs-lookup"><span data-stu-id="c6347-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="c6347-134">Restauration d’une migration</span><span class="sxs-lookup"><span data-stu-id="c6347-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="c6347-135">Si vous avez déjà appliqué une migration (ou plusieurs migrations) à la base de données, mais que vous devez la restaurer, vous pouvez utiliser la même commande que celle servant à appliquer des migrations, mais en spécifiant le nom de la migration à restaurer.</span><span class="sxs-lookup"><span data-stu-id="c6347-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="c6347-136">Migrations vides</span><span class="sxs-lookup"><span data-stu-id="c6347-136">Empty migrations</span></span>
----------------
<span data-ttu-id="c6347-137">Il est parfois utile d’ajouter une migration sans apporter de modification au modèle.</span><span class="sxs-lookup"><span data-stu-id="c6347-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="c6347-138">Dans ce cas, l’ajout d’une nouvelle migration crée une migration vide.</span><span class="sxs-lookup"><span data-stu-id="c6347-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="c6347-139">Vous pouvez personnaliser cette migration pour effectuer des opérations qui ne sont pas directement liées au modèle EF Core.</span><span class="sxs-lookup"><span data-stu-id="c6347-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="c6347-140">Voici quelques exemples de ce que vous pouvez gérer de cette façon :</span><span class="sxs-lookup"><span data-stu-id="c6347-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="c6347-141">Recherche en texte intégral</span><span class="sxs-lookup"><span data-stu-id="c6347-141">Full-Text Search</span></span>
* <span data-ttu-id="c6347-142">Fonctions</span><span class="sxs-lookup"><span data-stu-id="c6347-142">Functions</span></span>
* <span data-ttu-id="c6347-143">Procédures stockées</span><span class="sxs-lookup"><span data-stu-id="c6347-143">Stored procedures</span></span>
* <span data-ttu-id="c6347-144">Déclencheurs</span><span class="sxs-lookup"><span data-stu-id="c6347-144">Triggers</span></span>
* <span data-ttu-id="c6347-145">Affichages</span><span class="sxs-lookup"><span data-stu-id="c6347-145">Views</span></span>
* <span data-ttu-id="c6347-146">Etc.</span><span class="sxs-lookup"><span data-stu-id="c6347-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="c6347-147">Génération d’un script SQL</span><span class="sxs-lookup"><span data-stu-id="c6347-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="c6347-148">Quand vous déboguez vos migrations ou que vous les déployez sur une base de données de production, il est utile de générer un script SQL.</span><span class="sxs-lookup"><span data-stu-id="c6347-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="c6347-149">Vous pouvez ensuite revoir le script et l’affiner en fonction des besoins d’une base de données de production.</span><span class="sxs-lookup"><span data-stu-id="c6347-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="c6347-150">Vous pouvez également utiliser le script conjointement avec une technologie de déploiement.</span><span class="sxs-lookup"><span data-stu-id="c6347-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="c6347-151">La commande de base est la suivante.</span><span class="sxs-lookup"><span data-stu-id="c6347-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="c6347-152">Il existe plusieurs options pour cette commande.</span><span class="sxs-lookup"><span data-stu-id="c6347-152">There are several options to this command.</span></span>

<span data-ttu-id="c6347-153">La migration **from** doit être la dernière migration appliquée à la base de données avant l’exécution du script.</span><span class="sxs-lookup"><span data-stu-id="c6347-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="c6347-154">Si aucune migration n’a été appliquée, spécifiez `0` (il s’agit de la valeur par défaut).</span><span class="sxs-lookup"><span data-stu-id="c6347-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="c6347-155">La migration **to** est la dernière migration à appliquer à la base de données après l’exécution du script.</span><span class="sxs-lookup"><span data-stu-id="c6347-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="c6347-156">Par défaut, il s’agit de la dernière migration dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="c6347-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="c6347-157">Un script **idempotent** peut également être généré.</span><span class="sxs-lookup"><span data-stu-id="c6347-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="c6347-158">Ce script n’applique les migrations que si elles n’ont pas déjà été appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6347-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="c6347-159">Cela est utile si vous ne savez pas exactement ce que la dernière migration a appliqué à la base de données ou si vous effectuez un déploiement sur plusieurs bases de données pouvant chacune être liée à une migration différente.</span><span class="sxs-lookup"><span data-stu-id="c6347-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="c6347-160">Application de migrations au moment de l’exécution</span><span class="sxs-lookup"><span data-stu-id="c6347-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="c6347-161">Certaines applications sont susceptibles d’appliquer des migrations au moment de l’exécution (au démarrage ou à la première exécution).</span><span class="sxs-lookup"><span data-stu-id="c6347-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="c6347-162">Ces opérations nécessitent l’utilisation de la méthode `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="c6347-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="c6347-163">Attention, cette approche ne s’adresse pas à tout un chacun.</span><span class="sxs-lookup"><span data-stu-id="c6347-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="c6347-164">Bien qu’elle soit idéale pour les applications avec une base de données locale, la plupart des applications nécessitent une stratégie de déploiement plus robuste, telle que la génération de scripts SQL.</span><span class="sxs-lookup"><span data-stu-id="c6347-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="c6347-165">N’appelez pas `EnsureCreated()` avant `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="c6347-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="c6347-166">`EnsureCreated()` ignore Migrations pour créer le schéma et entraîne l’échec de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="c6347-166">`EnsureCreated()` bypasses Migrations to create the schema and cause `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="c6347-167">Cette méthode s’appuie sur le service `IMigrator`, qui peut être utilisé pour des scénarios plus avancés.</span><span class="sxs-lookup"><span data-stu-id="c6347-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="c6347-168">Utilisez `DbContext.GetService<IMigrator>()` pour y accéder.</span><span class="sxs-lookup"><span data-stu-id="c6347-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
