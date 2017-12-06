---
title: "Table d’historique des Migrations personnalisé - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a>Table d’historique des Migrations personnalisé
===============================
Par défaut, EF Core effectue le suivi des migrations ont été appliquées à la base de données en les enregistrant dans une table nommée `__EFMigrationsHistory`. Pour diverses raisons, vous souhaiterez le personnaliser en fonction de vos besoins.

> [!IMPORTANT]
> Si vous personnalisez la table d’historique de Migrations *après* application migrations, vous êtes responsable de la mise à jour de la table existante dans la base de données.

<a name="schema-and-table-name"></a>Nom de schéma et une table
----------------------
Vous pouvez modifier le schéma et le nom de la table à l’aide du `MigrationsHistoryTable()` méthode dans `OnConfiguring()` (ou `ConfigureServices()` sur ASP.NET Core). Voici un exemple utilisant le fournisseur SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Autres modifications
-------------
Pour configurer des aspects supplémentaires de la table, remplacer et remplacez spécifique au fournisseur `IHistoryRepository` service. Voici un exemple de modification de nom de colonne MigrationId *Id* sur SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`à l’intérieur d’un espace de noms interne et peut changer dans les futures versions.

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
