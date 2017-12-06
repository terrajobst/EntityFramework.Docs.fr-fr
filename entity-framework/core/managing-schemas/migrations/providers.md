---
title: Migrations avec plusieurs fournisseurs - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a>Migrations avec plusieurs fournisseurs
==================================
Le [EF principaux outils] [ 1] structurez uniquement des migrations pour le fournisseur active. Toutefois, vous pouvez être amené à utiliser plusieurs fournisseurs (par exemple Microsoft SQL Server et SQLite) avec votre DbContext. Il existe deux façons de gérer cette situation avec des Migrations. Vous pouvez conserver deux jeux de migrations--un pour chaque fournisseur--ou la fusion en une seule valeur qui peut travailler sur les deux.

<a name="two-migration-sets"></a>Deux ensembles de migration
------------------
Dans la première approche, vous générez deux migrations pour chaque modification de modèle.

Pour faire cela consiste à placer chaque ensemble de la migration [dans un assembly distinct] [ 2] et basculez manuellement le fournisseur active (et l’assembly de migrations) entre l’ajout de ces deux migrations.

Une autre approche que facilite l’utilisation des outils plus facile pour créer un nouveau type dérive de votre DbContext et remplace le fournisseur actif. Ce type est utilisé lors de la conception du temps lors de l’ajout ou de l’application de migrations.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Étant donné que chaque ensemble de migration utilise ses propres types DbContext, cette approche ne requiert pas à l’aide d’un assembly distinct des migrations.

Lorsque vous ajoutez la nouvelle migration, spécifier les types de contexte.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Vous n’avez pas besoin de spécifier le répertoire de sortie pour les migrations suivantes dans la mesure où ils sont créés en tant que frères au dernier signet.

<a name="one-migration-set"></a>Migration d’un jeu
-----------------
Si vous n’aimez pas avoir deux jeux de migration, vous pouvez manuellement les combiner en un seul ensemble peut être appliqué pour les deux fournisseurs.

Les annotations peuvent coexister dans la mesure où un fournisseur ignore toutes les annotations ne comprend pas. Par exemple, une colonne de clé primaire qui fonctionne avec Microsoft SQL Server et SQLite peut ressembler à ceci.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Si les opérations peuvent être appliquées uniquement sur un fournisseur (ou elles sont différemment entre les fournisseurs), utilisez le `ActiveProvider` propriété pour indiquer à quel fournisseur est actif.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
