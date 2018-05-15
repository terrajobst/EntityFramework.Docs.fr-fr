---
title: Opérations de Migrations personnalisé - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a><span data-ttu-id="7aeeb-102">Opérations de migration personnalisée</span><span class="sxs-lookup"><span data-stu-id="7aeeb-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="7aeeb-103">Permet à l’API MigrationBuilder vous permet d’effectuer de nombreux types d’opérations lors d’une migration, mais il est loin d’être exhaustive.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="7aeeb-104">Toutefois, l’API est également extensible, ce qui vous permet de définir vos propres opérations.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="7aeeb-105">Il existe deux façons d’étendre l’API : à l’aide de la `Sql()` (méthode), ou en définissant personnalisé `MigrationOperation` objets.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="7aeeb-106">Pour illustrer cela, regardons implémente une opération qui crée un utilisateur de base de données à l’aide de chaque approche.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="7aeeb-107">Dans notre migrations, nous voulons pouvoir écrire le code suivant :</span><span class="sxs-lookup"><span data-stu-id="7aeeb-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="7aeeb-108">À l’aide de MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="7aeeb-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="7aeeb-109">Le moyen le plus simple pour implémenter une opération personnalisée consiste à définir une méthode d’extension qui appelle `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="7aeeb-110">Voici un exemple qui génère le code Transact-SQL approprié.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="7aeeb-111">Si vos migrations avez besoin prendre en charge plusieurs fournisseurs de base de données, vous pouvez utiliser le `MigrationBuilder.ActiveProvider` propriété.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="7aeeb-112">Voici un exemple de prise en charge de Microsoft SQL Server et PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="7aeeb-113">Cette approche fonctionne uniquement si vous savez que tous les fournisseurs où votre opération personnalisé est appliquée.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="7aeeb-114">À l’aide d’une MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="7aeeb-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="7aeeb-115">Pour séparer l’opération personnalisée à partir de l’instruction SQL, vous pouvez définir vos propres `MigrationOperation` pour le représenter.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="7aeeb-116">L’opération est ensuite transmise au fournisseur pour permettre de déterminer la requête SQL appropriée pour générer.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="7aeeb-117">Avec cette approche, la méthode d’extension doit simplement ajouter une de ces opérations à `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="7aeeb-118">Cette approche, chaque fournisseur pour savoir comment générer SQL pour cette opération dans leurs `IMigrationsSqlGenerator` service.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="7aeeb-119">Voici un exemple de substitution de générateur de SQL Server pour gérer la nouvelle opération.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="7aeeb-120">Remplacer le service de génération par défaut migrations sql par la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="7aeeb-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
