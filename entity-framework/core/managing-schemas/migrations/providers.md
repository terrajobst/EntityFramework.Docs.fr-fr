---
title: Migrations avec plusieurs fournisseurs-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: c9b1a2563ef548e592374f90a6242b0bd851bc98
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811962"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="fc6ea-102">Migrations avec plusieurs fournisseurs</span><span class="sxs-lookup"><span data-stu-id="fc6ea-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="fc6ea-103">Les [outils de EF corent][1] uniquement les migrations de l’échafaudage pour le fournisseur actif.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="fc6ea-104">Toutefois, il peut arriver que vous souhaitiez utiliser plusieurs fournisseurs (par exemple Microsoft SQL Server et SQLite) avec votre DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="fc6ea-105">Il existe deux façons de gérer cela avec les migrations.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="fc6ea-106">Vous pouvez gérer deux ensembles de migrations, un pour chaque fournisseur, ou les fusionner dans un ensemble unique qui peut fonctionner sur les deux.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="fc6ea-107">Deux ensembles de migration</span><span class="sxs-lookup"><span data-stu-id="fc6ea-107">Two migration sets</span></span>

<span data-ttu-id="fc6ea-108">Dans la première approche, vous générez deux migrations pour chaque modification de modèle.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="fc6ea-109">Pour ce faire, vous pouvez placer chaque ensemble de migration [dans un assembly distinct][2] et basculer manuellement le fournisseur actif (et l’assembly de migration) entre l’ajout des deux migrations.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="fc6ea-110">Une autre approche qui facilite l’utilisation des outils consiste à créer un nouveau type qui dérive de votre DbContext et qui remplace le fournisseur actif.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="fc6ea-111">Ce type est utilisé au moment de la conception lors de l’ajout ou de l’application de migrations.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="fc6ea-112">Étant donné que chaque jeu de migration utilise ses propres types DbContext, cette approche ne nécessite pas l’utilisation d’un assembly de migrations distinct.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="fc6ea-113">Lorsque vous ajoutez une nouvelle migration, spécifiez les types de contexte.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="fc6ea-114">Vous n’avez pas besoin de spécifier le répertoire de sortie pour les migrations suivantes, car elles sont créées en tant que frères au dernier.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="fc6ea-115">Un jeu de migration</span><span class="sxs-lookup"><span data-stu-id="fc6ea-115">One migration set</span></span>

<span data-ttu-id="fc6ea-116">Si vous n’aimez pas avoir deux ensembles de migrations, vous pouvez les combiner manuellement en un seul ensemble qui peut être appliqué aux deux fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="fc6ea-117">Les annotations peuvent coexister car un fournisseur ignore les annotations qu’il ne comprend pas.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="fc6ea-118">Par exemple, une colonne de clé primaire qui fonctionne avec Microsoft SQL Server et SQLite peut se présenter comme suit.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="fc6ea-119">Si les opérations ne peuvent être appliquées que sur un fournisseur (ou si elles sont différentes entre les fournisseurs), utilisez la propriété `ActiveProvider` pour indiquer quel fournisseur est actif.</span><span class="sxs-lookup"><span data-stu-id="fc6ea-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
