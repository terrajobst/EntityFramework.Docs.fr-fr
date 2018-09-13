---
title: Migrations avec plusieurs fournisseurs - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
ms.openlocfilehash: 75c055221609679db3f00016b9cb44c6c8c6e473
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488775"
---
<a name="migrations-with-multiple-providers"></a>Migrations avec plusieurs fournisseurs
==================================
Le [outils EF Core] [ 1] structurer uniquement les migrations pour le fournisseur active. Toutefois, vous pouvez être amené à utiliser plusieurs fournisseurs (par exemple Microsoft SQL Server et SQLite) avec votre DbContext. Il existe deux façons de gérer cela avec des Migrations. Vous pouvez conserver les deux ensembles de migrations--un pour chaque fournisseur--ou la fusion dans une seule valeur qui peut travailler sur les deux.

<a name="two-migration-sets"></a>Deux ensembles de migration
------------------
Dans la première approche, vous générez deux migrations pour chaque modification de modèle.

Pour effectuer cela consiste à placer chaque ensemble de la migration [dans un assembly distinct] [ 2] et basculer manuellement le fournisseur actif (et l’assembly de migrations) entre l’ajout les deux migrations.

Une autre approche qui facilite l’utilisation avec les outils consiste à créer un nouveau type qui dérive de votre DbContext et remplace le fournisseur actif. Ce type est utilisé lors de la conception du temps lors de l’ajout ou appliquer des migrations.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Étant donné que chaque ensemble de migration utilise ses propres types DbContext, cette approche ne nécessite pas à l’aide d’un assembly de migrations distinctes.

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
Si vous n’aimez pas avoir deux ensembles de migrations, vous pouvez manuellement les combiner en un seul ensemble peut être appliqué pour les deux fournisseurs.

Annotations peuvent coexister dans la mesure où un fournisseur ignore toutes les annotations qui ne comprend pas. Par exemple, une colonne de clé primaire qui fonctionne avec Microsoft SQL Server et SQLite peut ressembler à ceci.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Si des opérations peuvent être appliquées uniquement sur un seul fournisseur (ou elles différemment entre les fournisseurs), utilisez le `ActiveProvider` propriété pour indiquer à quel fournisseur est actif.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
