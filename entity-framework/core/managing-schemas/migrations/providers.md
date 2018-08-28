---
title: Migrations avec plusieurs fournisseurs - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997971"
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="9ccbf-102">Migrations avec plusieurs fournisseurs</span><span class="sxs-lookup"><span data-stu-id="9ccbf-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="9ccbf-103">Le [outils EF Core] [ 1] structurer uniquement les migrations pour le fournisseur active.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="9ccbf-104">Toutefois, vous pouvez être amené à utiliser plusieurs fournisseurs (par exemple Microsoft SQL Server et SQLite) avec votre DbContext.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="9ccbf-105">Il existe deux façons de gérer cela avec des Migrations.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="9ccbf-106">Vous pouvez conserver les deux ensembles de migrations--un pour chaque fournisseur--ou la fusion dans une seule valeur qui peut travailler sur les deux.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="9ccbf-107">Deux ensembles de migration</span><span class="sxs-lookup"><span data-stu-id="9ccbf-107">Two migration sets</span></span>
------------------
<span data-ttu-id="9ccbf-108">Dans la première approche, vous générez deux migrations pour chaque modification de modèle.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="9ccbf-109">Pour effectuer cela consiste à placer chaque ensemble de la migration [dans un assembly distinct] [ 2] et basculer manuellement le fournisseur actif (et l’assembly de migrations) entre l’ajout les deux migrations.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="9ccbf-110">Une autre approche qui facilite l’utilisation avec les outils consiste à créer un nouveau type qui dérive de votre DbContext et remplace le fournisseur actif.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="9ccbf-111">Ce type est utilisé lors de la conception du temps lors de l’ajout ou appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="9ccbf-112">Étant donné que chaque ensemble de migration utilise ses propres types DbContext, cette approche ne nécessite pas à l’aide d’un assembly de migrations distinctes.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="9ccbf-113">Lorsque vous ajoutez la nouvelle migration, spécifier les types de contexte.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="9ccbf-114">Vous n’avez pas besoin de spécifier le répertoire de sortie pour les migrations suivantes dans la mesure où ils sont créés en tant que frères au dernier signet.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="9ccbf-115">Migration d’un jeu</span><span class="sxs-lookup"><span data-stu-id="9ccbf-115">One migration set</span></span>
-----------------
<span data-ttu-id="9ccbf-116">Si vous n’aimez pas avoir deux ensembles de migrations, vous pouvez manuellement les combiner en un seul ensemble peut être appliqué pour les deux fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="9ccbf-117">Annotations peuvent coexister dans la mesure où un fournisseur ignore toutes les annotations qui ne comprend pas.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="9ccbf-118">Par exemple, une colonne de clé primaire qui fonctionne avec Microsoft SQL Server et SQLite peut ressembler à ceci.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="9ccbf-119">Si des opérations peuvent être appliquées uniquement sur un seul fournisseur (ou elles différemment entre les fournisseurs), utilisez le `ActiveProvider` propriété pour indiquer à quel fournisseur est actif.</span><span class="sxs-lookup"><span data-stu-id="9ccbf-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
