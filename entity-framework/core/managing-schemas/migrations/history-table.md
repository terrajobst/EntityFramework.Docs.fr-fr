---
title: Table d’historique des migrations personnalisées-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812074"
---
# <a name="custom-migrations-history-table"></a>Table d’historique des migrations personnalisées

Par défaut, EF Core effectue le suivi des migrations qui ont été appliquées à la base de données en les enregistrant dans une table nommée `__EFMigrationsHistory`. Pour différentes raisons, vous souhaiterez peut-être personnaliser ce tableau pour mieux répondre à vos besoins.

> [!IMPORTANT]
> Si vous personnalisez la table d’historique des migrations *après avoir* appliqué des migrations, vous êtes responsable de la mise à jour de la table existante dans la base de données.

## <a name="schema-and-table-name"></a>Nom du schéma et de la table

Vous pouvez modifier le schéma et le nom de la table à l’aide de la méthode `MigrationsHistoryTable()` dans `OnConfiguring()` (ou `ConfigureServices()` sur ASP.NET Core). Voici un exemple d’utilisation du fournisseur SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Autres modifications

Pour configurer des aspects supplémentaires de la table, substituez et remplacez le service `IHistoryRepository` spécifique au fournisseur. Voici un exemple de modification du nom de la colonne MigrationId en *ID* sur SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` est à l’intérieur d’un espace de noms interne et peut changer dans les versions ultérieures.

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
