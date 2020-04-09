---
title: Nouveautés dans EF Core 5.0
description: Aperçu des nouvelles fonctionnalités dans EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634279"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="5cca0-103">Nouveautés dans EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="5cca0-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="5cca0-104">EF Core 5.0 est actuellement en développement.</span><span class="sxs-lookup"><span data-stu-id="5cca0-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="5cca0-105">Cette page contiendra un aperçu des modifications intéressantes introduites dans chaque aperçu.</span><span class="sxs-lookup"><span data-stu-id="5cca0-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="5cca0-106">Cette page ne duplique pas le [plan pour EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="5cca0-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="5cca0-107">Le plan décrit les thèmes généraux de EF Core 5.0, y compris tout ce que nous prévoyons d’inclure avant d’expédier la version finale.</span><span class="sxs-lookup"><span data-stu-id="5cca0-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="5cca0-108">Nous ajouterons des liens d’ici à la documentation officielle telle qu’elle sera publiée.</span><span class="sxs-lookup"><span data-stu-id="5cca0-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="5cca0-109">Préversion 2</span><span class="sxs-lookup"><span data-stu-id="5cca0-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="5cca0-110">Utilisez un attribut CMD pour spécifier un champ de sauvegarde de propriété</span><span class="sxs-lookup"><span data-stu-id="5cca0-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="5cca0-111">Un attribut Cmd peut maintenant être utilisé pour spécifier le champ d’appui d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="5cca0-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="5cca0-112">Cet attribut permet à EF Core d’écrire et de lire à partir du champ d’accompagnement comme cela se produirait normalement, même lorsque le champ de soutien ne peut pas être trouvé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="5cca0-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="5cca0-113">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="5cca0-114">La documentation est suivie par question [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="5cca0-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="5cca0-115">Cartographie complète des discriminateteurs</span><span class="sxs-lookup"><span data-stu-id="5cca0-115">Complete discriminator mapping</span></span>

<span data-ttu-id="5cca0-116">EF Core utilise une colonne discriminatrice pour [la cartographie TPH d’une hiérarchie successorale](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="5cca0-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="5cca0-117">Certaines améliorations de performance sont possibles tant que EF Core connaît toutes les valeurs possibles pour le discriminateur.</span><span class="sxs-lookup"><span data-stu-id="5cca0-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="5cca0-118">EF Core 5.0 met maintenant en œuvre ces améliorations.</span><span class="sxs-lookup"><span data-stu-id="5cca0-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="5cca0-119">Par exemple, les versions précédentes d’EF Core généreraient toujours cette SQL pour une requête restituant tous les types dans une hiérarchie :</span><span class="sxs-lookup"><span data-stu-id="5cca0-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="5cca0-120">EF Core 5.0 générera désormais ce qui suit lorsqu’une cartographie complète des discriminateurs sera configurée :</span><span class="sxs-lookup"><span data-stu-id="5cca0-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="5cca0-121">Ce sera le comportement par défaut à partir de l’aperçu 3.</span><span class="sxs-lookup"><span data-stu-id="5cca0-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="5cca0-122">Amélioration des performances dans Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="5cca0-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="5cca0-123">Nous avons apporté deux améliorations de performance pour SQLIte :</span><span class="sxs-lookup"><span data-stu-id="5cca0-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="5cca0-124">Récupérer des données binaires et de chaînes avec GetBytes, GetChars et GetTextReader est maintenant plus efficace en faisant usage de SqliteBlob et des flux.</span><span class="sxs-lookup"><span data-stu-id="5cca0-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="5cca0-125">L’initialisation de SqliteConnection est maintenant paresseuse.</span><span class="sxs-lookup"><span data-stu-id="5cca0-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="5cca0-126">Ces améliorations sont dans le ADO.NET fournisseur Microsoft.Data.Sqlite et donc également améliorer les performances en dehors de EF Core.</span><span class="sxs-lookup"><span data-stu-id="5cca0-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="5cca0-127">Preview 1</span><span class="sxs-lookup"><span data-stu-id="5cca0-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="5cca0-128">Enregistrement simple</span><span class="sxs-lookup"><span data-stu-id="5cca0-128">Simple logging</span></span>

<span data-ttu-id="5cca0-129">Cette fonctionnalité ajoute des `Database.Log` fonctionnalités similaires à celles d’EF6.</span><span class="sxs-lookup"><span data-stu-id="5cca0-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="5cca0-130">C’est-à-dire qu’il fournit un moyen simple d’obtenir des journaux à partir de EF Core sans avoir besoin de configurer n’importe quel type de cadre d’enregistrement externe.</span><span class="sxs-lookup"><span data-stu-id="5cca0-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="5cca0-131">La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="5cca0-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="5cca0-132">La documentation supplémentaire est suivie par question [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="5cca0-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="5cca0-133">Moyen simple d’obtenir généré SQL</span><span class="sxs-lookup"><span data-stu-id="5cca0-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="5cca0-134">EF Core 5.0 `ToQueryString` introduit la méthode d’extension, qui retournera la SQL que EF Core générera lors de l’exécution d’une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="5cca0-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="5cca0-135">La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 9 janvier 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="5cca0-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="5cca0-136">La documentation supplémentaire est suivie par question [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="5cca0-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="5cca0-137">Utilisez un attribut CMD pour indiquer qu’une entité n’a pas de</span><span class="sxs-lookup"><span data-stu-id="5cca0-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="5cca0-138">Un type d’entité peut maintenant être configuré comme n’ayant aucune clé en utilisant la nouvelle `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="5cca0-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="5cca0-139">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="5cca0-140">La documentation est suivie par question [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="5cca0-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="5cca0-141">La chaîne de connexion ou de connexion peut être modifiée sur DbContext initialisé</span><span class="sxs-lookup"><span data-stu-id="5cca0-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="5cca0-142">Il est maintenant plus facile de créer une instance DbContext sans aucune connexion ou chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="5cca0-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="5cca0-143">En outre, la chaîne de connexion ou de connexion peut maintenant être mutée sur l’instance contextuelle.</span><span class="sxs-lookup"><span data-stu-id="5cca0-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="5cca0-144">Cette fonctionnalité permet au même exemple de contexte de se connecter dynamiquement à différentes bases de données.</span><span class="sxs-lookup"><span data-stu-id="5cca0-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="5cca0-145">La documentation est suivie par question [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="5cca0-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="5cca0-146">Proxies de suivi des changements</span><span class="sxs-lookup"><span data-stu-id="5cca0-146">Change-tracking proxies</span></span>

<span data-ttu-id="5cca0-147">EF Core peut maintenant générer des procurations de temps d’exécution qui implémentent automatiquement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) et [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="5cca0-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="5cca0-148">Ceux-ci signalent ensuite les changements de valeur sur les propriétés de l’entité directement à EF Core, évitant la nécessité de numériser pour les changements.</span><span class="sxs-lookup"><span data-stu-id="5cca0-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="5cca0-149">Cependant, les procurations viennent avec leur propre ensemble de limitations, de sorte qu’ils ne sont pas pour tout le monde.</span><span class="sxs-lookup"><span data-stu-id="5cca0-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="5cca0-150">La documentation est suivie par question [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="5cca0-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="5cca0-151">Vues améliorées de débaillement</span><span class="sxs-lookup"><span data-stu-id="5cca0-151">Enhanced debug views</span></span>

<span data-ttu-id="5cca0-152">Les vues de débogé sont un moyen facile d’examiner les internes de EF Core lors de la débogage des questions.</span><span class="sxs-lookup"><span data-stu-id="5cca0-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="5cca0-153">Une vue de débogé pour le modèle a été mise en œuvre il ya quelque temps.</span><span class="sxs-lookup"><span data-stu-id="5cca0-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="5cca0-154">Pour EF Core 5.0, nous avons rendu la vue modèle plus facile à lire et ajouté une nouvelle vue de débbug pour les entités suivies dans le gestionnaire de l’État.</span><span class="sxs-lookup"><span data-stu-id="5cca0-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="5cca0-155">La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 12 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="5cca0-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="5cca0-156">La documentation supplémentaire est suivie par question [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="5cca0-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="5cca0-157">Amélioration de la manipulation de la sémantique nulle de base de données</span><span class="sxs-lookup"><span data-stu-id="5cca0-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="5cca0-158">Les bases de données relationnelles traitent généralement NULL comme une valeur inconnue et ne sont donc pas égales à tout autre NULL.</span><span class="sxs-lookup"><span data-stu-id="5cca0-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="5cca0-159">Bien que le CMD traite l’in null comme une valeur définie, qui se compare à l’égalité à tout autre null.</span><span class="sxs-lookup"><span data-stu-id="5cca0-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="5cca0-160">EF Core par défaut traduit les requêtes de sorte qu’ils utilisent la sémantique nulle C.</span><span class="sxs-lookup"><span data-stu-id="5cca0-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="5cca0-161">EF Core 5.0 améliore considérablement l’efficacité de ces traductions.</span><span class="sxs-lookup"><span data-stu-id="5cca0-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="5cca0-162">La documentation est suivie par question [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="5cca0-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="5cca0-163">Propriétés d’indexeur</span><span class="sxs-lookup"><span data-stu-id="5cca0-163">Indexer properties</span></span>

<span data-ttu-id="5cca0-164">EF Core 5.0 prend en charge la cartographie des propriétés de l’indexeur C.</span><span class="sxs-lookup"><span data-stu-id="5cca0-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="5cca0-165">Ces propriétés permettent aux entités d’agir comme sacs de propriété où les colonnes sont cartographiées aux propriétés nommées dans le sac.</span><span class="sxs-lookup"><span data-stu-id="5cca0-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="5cca0-166">La documentation est suivie par question [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="5cca0-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="5cca0-167">Génération de contraintes de contrôle pour les cartographies enum</span><span class="sxs-lookup"><span data-stu-id="5cca0-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="5cca0-168">EF Core 5.0 Les migrations peuvent désormais générer des contraintes CHECK pour les cartographies des propriétés enum.</span><span class="sxs-lookup"><span data-stu-id="5cca0-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="5cca0-169">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="5cca0-170">La documentation est suivie par question [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="5cca0-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="5cca0-171">IsRelational (en)</span><span class="sxs-lookup"><span data-stu-id="5cca0-171">IsRelational</span></span>

<span data-ttu-id="5cca0-172">Une `IsRelational` nouvelle méthode a été ajoutée `IsSqlServer` `IsSqlite`en `IsInMemory`plus de l’existant , , et .</span><span class="sxs-lookup"><span data-stu-id="5cca0-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="5cca0-173">Cette méthode peut être utilisée pour tester si le DbContext utilise n’importe quel fournisseur de base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="5cca0-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="5cca0-174">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="5cca0-175">La documentation est suivie par question [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="5cca0-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="5cca0-176">Cosmos optimiste concordance avec ETags</span><span class="sxs-lookup"><span data-stu-id="5cca0-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="5cca0-177">Le fournisseur de bases de données Azure Cosmos DB prend désormais en charge la concurrence optimiste à l’aide d’ETags.</span><span class="sxs-lookup"><span data-stu-id="5cca0-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="5cca0-178">Utilisez le constructeur de modèles dans OnModelCreating pour configurer un ETag :</span><span class="sxs-lookup"><span data-stu-id="5cca0-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="5cca0-179">SaveChanges lancera `DbUpdateConcurrencyException` alors un conflit de concurrence, qui [peut être manipulé](https://docs.microsoft.com/ef/core/saving/concurrency) pour mettre en œuvre des retries, etc.</span><span class="sxs-lookup"><span data-stu-id="5cca0-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="5cca0-180">La documentation est suivie par question [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="5cca0-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="5cca0-181">Traductions de requête pour plus de constructions DateTime</span><span class="sxs-lookup"><span data-stu-id="5cca0-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="5cca0-182">Les requêtes contenant la nouvelle construction dateTime sont maintenant traduites.</span><span class="sxs-lookup"><span data-stu-id="5cca0-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="5cca0-183">De plus, les fonctions suivantes de SQL Server sont maintenant cartographiées :</span><span class="sxs-lookup"><span data-stu-id="5cca0-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="5cca0-184">DateDiffWeek (en anglais seulement)</span><span class="sxs-lookup"><span data-stu-id="5cca0-184">DateDiffWeek</span></span>
* <span data-ttu-id="5cca0-185">DateDeparts</span><span class="sxs-lookup"><span data-stu-id="5cca0-185">DateFromParts</span></span>

<span data-ttu-id="5cca0-186">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="5cca0-187">La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="5cca0-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="5cca0-188">Traductions de requête pour plus de constructions de tableaux d’byte</span><span class="sxs-lookup"><span data-stu-id="5cca0-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="5cca0-189">Les requêtes utilisant contient, Longueur, SéquenceEqual, etc. sur les propriétés byte[] sont maintenant traduites en SQL.</span><span class="sxs-lookup"><span data-stu-id="5cca0-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="5cca0-190">La documentation préliminaire est incluse dans le [statut hebdomadaire ef pour le 5 décembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="5cca0-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="5cca0-191">La documentation supplémentaire est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="5cca0-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="5cca0-192">Traduction de requête pour Reverse</span><span class="sxs-lookup"><span data-stu-id="5cca0-192">Query translation for Reverse</span></span>

<span data-ttu-id="5cca0-193">Les requêtes utilisant `Reverse` sont maintenant traduites.</span><span class="sxs-lookup"><span data-stu-id="5cca0-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="5cca0-194">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="5cca0-195">La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="5cca0-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="5cca0-196">Traduction de requête pour les opérateurs bitwise</span><span class="sxs-lookup"><span data-stu-id="5cca0-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="5cca0-197">Les requêtes utilisant des opérateurs bitwise sont maintenant traduites dans plus de cas Par exemple :</span><span class="sxs-lookup"><span data-stu-id="5cca0-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="5cca0-198">La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="5cca0-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="5cca0-199">Traduction de requête pour cordes sur Cosmos</span><span class="sxs-lookup"><span data-stu-id="5cca0-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="5cca0-200">Les requêtes qui utilisent les méthodes de chaîne Contient, Démarrez et FinsAveavent sont maintenant traduites lors de l’utilisation du fournisseur Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5cca0-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="5cca0-201">La documentation est suivie par question [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="5cca0-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
