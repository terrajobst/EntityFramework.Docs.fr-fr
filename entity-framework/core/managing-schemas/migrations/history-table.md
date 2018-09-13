---
title: Table d’historique des Migrations personnalisé - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488814"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="390ca-102">Table d’historique des Migrations personnalisé</span><span class="sxs-lookup"><span data-stu-id="390ca-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="390ca-103">Par défaut, EF Core effectue le suivi des migrations qui ont été appliquées à la base de données en les enregistrant dans une table nommée `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="390ca-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="390ca-104">Pour diverses raisons, vous souhaiterez sans doute personnaliser ce tableau pour mieux répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="390ca-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="390ca-105">Si vous personnalisez la table d’historique de Migrations *après* appliquer des migrations, vous êtes responsable de la mise à jour de la table existante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="390ca-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="390ca-106">Nom de schéma et de table</span><span class="sxs-lookup"><span data-stu-id="390ca-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="390ca-107">Vous pouvez modifier le schéma et le nom de la table à l’aide du `MigrationsHistoryTable()` méthode dans `OnConfiguring()` (ou `ConfigureServices()` sur ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="390ca-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="390ca-108">Voici un exemple utilisant le fournisseur SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="390ca-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="390ca-109">Autres modifications</span><span class="sxs-lookup"><span data-stu-id="390ca-109">Other changes</span></span>
-------------
<span data-ttu-id="390ca-110">Pour configurer des aspects supplémentaires de la table, remplacer et remplacez spécifique au fournisseur `IHistoryRepository` service.</span><span class="sxs-lookup"><span data-stu-id="390ca-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="390ca-111">Voici un exemple de modification de nom de colonne MigrationId *Id* sur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="390ca-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="390ca-112">`SqlServerHistoryRepository` se trouve dans un espace de noms interne et peut changer dans les futures versions.</span><span class="sxs-lookup"><span data-stu-id="390ca-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
