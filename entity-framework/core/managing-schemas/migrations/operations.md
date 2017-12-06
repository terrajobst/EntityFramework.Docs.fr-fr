---
title: "Opérations de Migrations personnalisé - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d41409dee034e84d22092a5f9111dd79c87dcec3
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-operations"></a>Opérations de migration personnalisée
============================
Permet à l’API MigrationBuilder vous permet d’effectuer de nombreux types d’opérations lors d’une migration, mais il est loin d’être exhaustive. Toutefois, l’API est également extensible, ce qui vous permet de définir vos propres opérations. Il existe deux façons d’étendre l’API : à l’aide de la `Sql()` (méthode), ou en définissant personnalisé `MigrationOperation` objets.

Pour illustrer cela, regardons implémente une opération qui crée un utilisateur de base de données à l’aide de chaque approche. Dans notre migrations, nous voulons pouvoir écrire le code suivant :

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>À l’aide de MigrationBuilder.Sql()
----------------------------
Le moyen le plus simple pour implémenter une opération personnalisée consiste à définir une méthode d’extension qui appelle `MigrationBuilder.Sql()`.
Voici un exemple qui génère le code Transact-SQL approprié.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Si vos migrations avez besoin prendre en charge plusieurs fournisseurs de base de données, vous pouvez utiliser le `MigrationBuilder.ActiveProvider` propriété. Voici un exemple de prise en charge de Microsoft SQL Server et PostreSQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }
}
```

Cette approche fonctionne uniquement si vous savez que tous les fournisseurs où votre opération personnalisé est appliquée.

<a name="using-a-migrationoperation"></a>À l’aide d’une MigrationOperation
---------------------------
Pour séparer l’opération personnalisée à partir de l’instruction SQL, vous pouvez définir vos propres `MigrationOperation` pour le représenter. L’opération est ensuite transmise au fournisseur pour permettre de déterminer la requête SQL appropriée pour générer.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Avec cette approche, la méthode d’extension doit simplement ajouter une de ces opérations à `MigrationBuilder.Operations`.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

Cette approche, chaque fournisseur pour savoir comment générer SQL pour cette opération dans leurs `IMigrationsSqlGenerator` service. Voici un exemple de substitution de générateur de SQL Server pour gérer la nouvelle opération.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Remplacer le service de génération par défaut migrations sql par la mise à jour.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
