---
title: Migrations avec plusieurs fournisseurs-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: 384ad27e405adc2bccb5e96aae30e5bd7ac556be
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824712"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="dcf6b-102">Migrations avec plusieurs fournisseurs</span><span class="sxs-lookup"><span data-stu-id="dcf6b-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="dcf6b-103">Les [outils de EF corent][1] uniquement les migrations de l’échafaudage pour le fournisseur actif.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="dcf6b-104">Toutefois, il peut arriver que vous souhaitiez utiliser plusieurs fournisseurs (par exemple Microsoft SQL Server et SQLite) avec votre DbContext.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="dcf6b-105">Il existe deux façons de gérer cela avec les migrations.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="dcf6b-106">Vous pouvez gérer deux ensembles de migrations, un pour chaque fournisseur, ou les fusionner dans un ensemble unique qui peut fonctionner sur les deux.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="dcf6b-107">Deux ensembles de migration</span><span class="sxs-lookup"><span data-stu-id="dcf6b-107">Two migration sets</span></span>

<span data-ttu-id="dcf6b-108">Dans la première approche, vous générez deux migrations pour chaque modification de modèle.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="dcf6b-109">Pour ce faire, vous pouvez placer chaque ensemble de migration [dans un assembly distinct][2] et basculer manuellement le fournisseur actif (et l’assembly de migration) entre l’ajout des deux migrations.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="dcf6b-110">Une autre approche qui facilite l’utilisation des outils consiste à créer un nouveau type qui dérive de votre DbContext et qui remplace le fournisseur actif.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="dcf6b-111">Ce type est utilisé au moment de la conception lors de l’ajout ou de l’application de migrations.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="dcf6b-112">Étant donné que chaque jeu de migration utilise ses propres types DbContext, cette approche ne nécessite pas l’utilisation d’un assembly de migrations distinct.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="dcf6b-113">Lorsque vous ajoutez une nouvelle migration, spécifiez les types de contexte.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-113">When adding new migration, specify the context types.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="dcf6b-114">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="dcf6b-114">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="dcf6b-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dcf6b-115">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="dcf6b-116">Vous n’avez pas besoin de spécifier le répertoire de sortie pour les migrations suivantes, car elles sont créées en tant que frères au dernier.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-116">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="dcf6b-117">Un jeu de migration</span><span class="sxs-lookup"><span data-stu-id="dcf6b-117">One migration set</span></span>

<span data-ttu-id="dcf6b-118">Si vous n’aimez pas avoir deux ensembles de migrations, vous pouvez les combiner manuellement en un seul ensemble qui peut être appliqué aux deux fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-118">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="dcf6b-119">Les annotations peuvent coexister car un fournisseur ignore les annotations qu’il ne comprend pas.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-119">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="dcf6b-120">Par exemple, une colonne de clé primaire qui fonctionne avec Microsoft SQL Server et SQLite peut se présenter comme suit.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-120">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="dcf6b-121">Si les opérations ne peuvent être appliquées que sur un fournisseur (ou si elles sont différentes entre les fournisseurs), utilisez la propriété `ActiveProvider` pour indiquer quel fournisseur est actif.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-121">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
