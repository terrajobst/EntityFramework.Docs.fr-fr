---
title: Nouveautés d’EF Core 2.0 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417495"
---
# <a name="new-features-in-ef-core-20"></a>Nouvelles fonctionnalités d’EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0

EF Core cible désormais .NET Standard 2.0, ce qui signifie qu’il peut fonctionner avec .NET Core 2.0, .NET Framework 4.6.1 et d’autres bibliothèques qui implémentent .NET Standard 2.0.
Pour plus d’informations sur ce qui est pris en charge, consultez [Implémentations .NET prises en charge](../platforms/index.md).

## <a name="modeling"></a>Modélisation

### <a name="table-splitting"></a>Fractionnement de table

Vous pouvez désormais mapper deux ou plusieurs types d’entité à la même table, où la ou les colonnes de clé primaire sont partagées et chaque ligne correspond à deux ou plusieurs entités.

Pour utiliser le fractionnement de table, vous devez configurer une relation d’identification (où les propriétés de clé étrangère forment la clé primaire) entre tous les types d’entité partageant la table :

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Pour plus d’informations sur cette fonctionnalité, lisez la [section sur le fractionnement de table](xref:core/modeling/table-splitting).

### <a name="owned-types"></a>Types détenus

Un type d’entité possédé peut partager le même type .NET avec un autre type d’entité appartenant, mais puisqu’il ne peut pas être identifié juste par le type .NET il doit y avoir une navigation à elle à partir d’un autre type d’entité. L’entité qui contient la navigation définie est le propriétaire. Quand le propriétaire fait l’objet d’une interrogation, les types détenus sont inclus par défaut.

Par convention, une clé primaire cachée est créée pour le type détenu et est mappée à la même table que le propriétaire à l’aide du fractionnement de table. Cela permet d’utiliser des types détenus à l’image des types complexes dans EF6 :

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

Pour plus d’informations sur cette fonctionnalité, lisez la [section sur les types d’entités détenus](xref:core/modeling/owned-entities).

### <a name="model-level-query-filters"></a>Filtres de requête au niveau du modèle

EF Core 2.0 inclut une nouvelle fonctionnalité que nous appelons « filtres de requête au niveau du modèle ». Cette fonctionnalité permet aux prédicats de requête LINQ (expression booléenne généralement passée à l’opérateur de requête LINQ Where) d’être définis directement sur des types d’entité dans le modèle de métadonnées (généralement dans OnModelCreating). Ces filtres sont automatiquement appliqués à toutes les requêtes LINQ impliquant ces types d’entités, y compris ceux référencés indirectement, par exemple à l’aide d’Include ou de références de propriété de navigation directe. Voici deux applications courantes de cette fonctionnalité :

- Suppression réversible : un type d’entité définit une propriété IsDeleted.
- Architecture multilocataire : un type d’entité définit une propriété TenantId.

Voici un exemple simple illustrant la fonctionnalité pour les deux scénarios répertoriés ci-dessus :

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId);
    }
}
```

Nous définissons un filtre au niveau du modèle qui implémente une architecture multilocataire et une suppression réversible pour des instances du type d’entité `Post`. Notez l’utilisation `DbContext` d’une `TenantId`propriété au niveau de l’instance : . Les filtres au niveau du modèle utilisent la valeur de l’instance de contexte correcte (c’est-à-dire celle qui exécute la requête).

Les filtres peuvent être désactivés pour des requêtes LINQ individuelles à l’aide de l’opérateur IgnoreQueryFilters().

#### <a name="limitations"></a>Limites

- Les références de navigation ne sont pas autorisées. Cette fonctionnalité peut être ajoutée en fonction de vos commentaires.
- Les filtres ne peuvent être définis que sur le type d’entité racine d’une hiérarchie.

### <a name="database-scalar-function-mapping"></a>Mappage de fonctions scalaires de base de données

EF Core 2.0 inclut une contribution importante de [Paul Middleton](https://github.com/pmiddleton) qui permet de mapper les fonctions scalaires de base de données à des stubs de méthode en vue de les utiliser dans des requêtes LINQ et de les convertir en SQL.

Voici une brève description de l’utilisation de la fonctionnalité :

Déclarez une méthode statique sur votre `DbContext` et annotez-la avec `DbFunctionAttribute` :

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

Les méthodes telles que celle-ci sont inscrites automatiquement. Une fois inscrits, les appels à la méthode dans une requête LINQ peuvent être traduits en appels de fonction dans SQL :

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Points à noter :

- Par convention, le nom de la méthode est utilisé comme nom d’une fonction (dans ce cas une fonction définie par l’utilisateur) lors de la génération de la SQL, mais vous pouvez remplacer le nom et le schéma lors de l’enregistrement de la méthode.
- Actuellement, seules les fonctions scalaires sont prises en charge.
- Vous devez créer la fonction mappée dans la base de données. Les migrations EF Core ne s’occuperont pas de sa création.

### <a name="self-contained-type-configuration-for-code-first"></a>Configuration du type autonome pour Code First

Dans EF6, il était possible d’encapsuler la configuration Code First d’un type d’entité spécifique en effectuant une dérivation à partir *d’EntityTypeConfiguration*. Dans EF Core 2.0, nous reprenons ce modèle :

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
    public void Configure(EntityTypeBuilder<Customer> builder)
    {
        builder.HasKey(c => c.AlternateKey);
        builder.Property(c => c.Name).HasMaxLength(200);
    }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a>Hautes performances

### <a name="dbcontext-pooling"></a>Regroupement DbContext

En règle générale, le modèle de base pour l’utilisation d’EF Core dans une application ASP.NET Core implique l’inscription d’un type DbContext personnalisé dans le système d’injection de dépendances, puis l’obtention d’instances de ce type par le biais de paramètres de constructeur dans les contrôleurs. Cela signifie qu’une instance de DbContext est créée pour chaque demande.

La version 2.0 offre une nouvelle façon d’inscrire des types DbContext personnalisés dans l’injection de dépendances, qui introduit en toute transparence un groupe d’instances de DbContext réutilisables. Pour utiliser le regroupement DbContext, utilisez `AddDbContextPool` au lieu d’`AddDbContext` durant l’inscription des services :

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Si cette méthode est utilisée, au moment où une instance de DbContext est demandée par un contrôleur, nous vérifions d’abord s’il existe une instance disponible dans le pool. Une fois finalisé le traitement de la demande, tout état sur l’instance est réinitialisé et l’instance elle-même est retournée au pool.

Ce regroupement est conceptuellement semblable au regroupement de connexions dans les fournisseurs ADO.NET et présente l’avantage de réduire les coûts d’initialisation de l’instance de DbContext.

### <a name="limitations"></a>Limites

La nouvelle méthode présente quelques limitations quant à ce qui peut être effectué dans la méthode `OnConfiguring()` de DbContext.

> [!WARNING]  
> Évitez d’utiliser le regroupement DbContext si vous gérez votre propre état (par exemple, champs privés) dans votre classe DbContext dérivée qui ne doit pas être partagée entre les requêtes. EF Core réinitialise uniquement l’état dont il a connaissance avant d’ajouter une instance de DbContext au pool.

### <a name="explicitly-compiled-queries"></a>Requêtes compilées explicitement

Il s’agit de la deuxième fonctionnalité de gain de performance à utiliser au choix conçue pour offrir des avantages dans les scénarios à grande échelle.

Les API de requête manuelles ou compilées explicitement étaient disponibles dans les versions précédentes d’EF, ainsi que dans LINQ to SQL, pour permettre aux applications de mettre en cache la conversion des requêtes afin qu’elles puissent être calculées une seule fois et exécutées plusieurs fois.

Bien qu’en général EF Core puisse automatiquement compiler et mettre en cache les requêtes en fonction d’une représentation hachée des expressions de requête, ce mécanisme peut être utilisé pour obtenir un petit gain de performances en contournant le calcul du hachage et la recherche dans le cache, ce qui permet à l’application d’utiliser une requête déjà compilée par le biais de l’appel d’un délégué.

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a>Suivi des modifications

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Attach peut suivre un graphique des entités nouvelles et existantes

EF Core prend en charge la génération automatique des valeurs de clés par le biais d’une variété de mécanismes. Quand vous utilisez cette fonctionnalité, une valeur est générée si la propriété de clé est la valeur par défaut du CLR, généralement zéro ou null. Cela signifie qu’un graphique d’entités peut être transmis à `DbContext.Attach` ou `DbSet.Attach` et qu’EF Core marque les entités qui ont déjà une clé définie comme `Unchanged` tandis que les entités qui n’ont pas de clé définie sont marquées comme `Added`. Il est ainsi plus facile de joindre un graphique regroupant des entités nouvelles et existantes quand des clés générées sont utilisées. `DbContext.Update` et `DbSet.Update` fonctionnent de la même manière, à la différence que les entités ayant une clé définie sont marquées comme `Modified` au lieu de `Unchanged`.

## <a name="query"></a>Requête

### <a name="improved-linq-translation"></a>Amélioration de la traduction LINQ

Permet l’exécution de davantage de requêtes, avec davantage de logique évaluée dans la base de données (plutôt qu’en mémoire) et moins de données inutilement récupérées de la base de données.

### <a name="groupjoin-improvements"></a>Améliorations apportées aux jonctions de groupes

Le SQL généré pour les jonctions de groupes est amélioré. Les jonctions de groupes sont souvent le résultat de sous-requêtes sur des propriétés de navigation facultatives.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolation de chaîne dans FromSql et ExecuteSqlCommand

C# 6 a introduit l’interpolation de chaîne, fonctionnalité qui permet d’incorporer les expressions C# directement dans des littéraux de chaîne, offrant ainsi un moyen agréable de générer des chaînes au moment de l’exécution. Dans EF Core 2.0, nous avons ajouté une prise en charge spéciale pour les chaînes interpolées à nos deux API principales qui acceptent des chaînes SQL brutes : `FromSql` et `ExecuteSqlCommand`. Ce nouveau support permet d’utiliser l’interpolation des chaînes CMD de manière « sûre ». c’est-à-dire d’une façon qui protège contre les erreurs d’injection SQL courantes pouvant se produire pendant la construction dynamique du code SQL au moment de l’exécution.

Voici un exemple :

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

Dans cet exemple, il existe deux variables incorporées dans la chaîne de format SQL. EF Core génère le code SQL suivant :

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Nous avons ajouté la propriété EF.Functions qui peut être utilisée par EF Core ou des fournisseurs pour définir des méthodes mappées à des fonctions ou des opérateurs de base de données afin qu’elles puissent être appelées dans les requêtes LINQ. Le premier exemple de ce type de méthode est Like() :

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Notez que Like() est fourni avec une implémentation en mémoire, ce qui peut être pratique quand vous utilisez une base de données en mémoire ou que l’évaluation du prédicat doit se produire côté client.

## <a name="database-management"></a>Gestion de bases de données

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Crochet de pluralisation pour l’échafaudage DbContext

EF Core 2.0 introduit un nouveau service *IPluralizer* qui est utilisé pour singulariser les noms des types d’entité et pluraliser les noms DbSet. L’implémentation par défaut étant l’absence d’opération, il s’agit simplement d’un dispositif permettant à l’utilisateur d’incorporer facilement son propre pluraliseur.

Voici à quoi cela ressemble dans le cas d’un développeur souhaitant mettre en place son propre pluraliseur :

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a>Autres

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Déplacement du fournisseur ADO.NET SQLite vers SQLitePCL.raw

Nous disposons ainsi d’une solution plus fiable dans Microsoft.Data.Sqlite pour distribuer des fichiers binaires SQLite natifs sur différentes plateformes.

### <a name="only-one-provider-per-model"></a>Un seul fournisseur par modèle

Renforce considérablement les possibilités d’interaction des fournisseurs avec le modèle et simplifie le fonctionnement des conventions, annotations et API Fluent avec les différents fournisseurs.

EF Core 2.0 génère désormais un [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) différent par fournisseur utilisé. Cela est généralement transparent pour l’application. Cela a facilité une simplification des API métadonnées de niveau inférieur de telle sorte que tout `.SqlServer`accès `.Sqlite`à des *concepts communs de métadonnées relationnelles* est toujours fait par un appel à `.Relational` au lieu de , etc.

### <a name="consolidated-logging-and-diagnostics"></a>Exploitation forestière et diagnostic consolidés

Les mécanismes de journalisation (basés sur ILogger) et de diagnostics (basés sur DiagnosticSource) partagent désormais plus de code.

Les ID d’événement pour les messages envoyés à un ILogger ont été changés dans la version 2.0. Les ID d’événement sont maintenant uniques dans le code EF Core. En outre, ces messages suivent désormais le modèle standard de la journalisation structurée utilisé, par exemple, par le modèle MVC.

Les catégories d’enregistreurs d’événements ont également changé. Il existe désormais un jeu connu de catégories accessibles par le biais de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).

Les événements DiagnosticSource utilisent désormais les mêmes noms d’ID d’événement que les messages `ILogger` correspondants.
