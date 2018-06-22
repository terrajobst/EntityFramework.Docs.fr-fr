---
title: Relations - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053029"
---
# <a name="relationships"></a><span data-ttu-id="502e6-102">Relationships</span><span class="sxs-lookup"><span data-stu-id="502e6-102">Relationships</span></span>

<span data-ttu-id="502e6-103">Une relation définit comment deux entités sont liés entre eux.</span><span class="sxs-lookup"><span data-stu-id="502e6-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="502e6-104">Dans une base de données relationnelle, il est représenté par une contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="502e6-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="502e6-105">La plupart des exemples dans cet article utilisent une relation un-à-plusieurs pour illustrer les concepts.</span><span class="sxs-lookup"><span data-stu-id="502e6-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="502e6-106">Pour obtenir des exemples de relations un à un et plusieurs-à-plusieurs, consultez le [autres modèles de relation](#other-relationship-patterns) section à la fin de l’article.</span><span class="sxs-lookup"><span data-stu-id="502e6-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="502e6-107">Définition des termes</span><span class="sxs-lookup"><span data-stu-id="502e6-107">Definition of Terms</span></span>

<span data-ttu-id="502e6-108">Il existe un nombre de termes utilisés pour décrire des relations</span><span class="sxs-lookup"><span data-stu-id="502e6-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="502e6-109">**Entité dépendante :** cela est l’entité qui contient les propriétés de clés étrangères.</span><span class="sxs-lookup"><span data-stu-id="502e6-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="502e6-110">Parfois comme enfant de la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="502e6-111">**Entité principale :** cela est l’entité qui contient les propriétés de clés primaire/remplacement.</span><span class="sxs-lookup"><span data-stu-id="502e6-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="502e6-112">Parfois appelée « parent » de la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="502e6-113">**Clé étrangère :** les propriétés de l’entité dépendante qui est utilisée pour stocker les valeurs de la propriété de clé principale associée à l’entité.</span><span class="sxs-lookup"><span data-stu-id="502e6-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="502e6-114">**Clé principale :** la propriété qui identifie de façon unique l’entité principale.</span><span class="sxs-lookup"><span data-stu-id="502e6-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="502e6-115">Cela peut être la clé primaire ou une autre clé.</span><span class="sxs-lookup"><span data-stu-id="502e6-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="502e6-116">**Propriété de navigation :** une propriété définie sur l’entité principale et/ou dépendante qui contient une ou plusieurs références aux ou les entités connexes.</span><span class="sxs-lookup"><span data-stu-id="502e6-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="502e6-117">**Propriété de navigation de collection :** une propriété de navigation qui contient des références à nombreuses entités connexes.</span><span class="sxs-lookup"><span data-stu-id="502e6-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="502e6-118">**Référence de propriété de navigation :** une propriété de navigation qui conserve une référence à une entité connexe unique.</span><span class="sxs-lookup"><span data-stu-id="502e6-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="502e6-119">**Propriété de navigation inversée :** lorsque vous présentez une propriété de navigation particulier, ce terme fait référence à la propriété de navigation à l’autre extrémité de la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="502e6-120">Le code suivant montre une relation un-à-plusieurs entre `Blog` et`Post`</span><span class="sxs-lookup"><span data-stu-id="502e6-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="502e6-121">`Post`est de l’entité dépendante</span><span class="sxs-lookup"><span data-stu-id="502e6-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="502e6-122">`Blog`est de l’entité principale</span><span class="sxs-lookup"><span data-stu-id="502e6-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="502e6-123">`Post.BlogId`est la clé étrangère</span><span class="sxs-lookup"><span data-stu-id="502e6-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="502e6-124">`Blog.BlogId`est la clé principale (dans ce cas il est une clé primaire plutôt que d’une autre clé)</span><span class="sxs-lookup"><span data-stu-id="502e6-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="502e6-125">`Post.Blog`est une propriété de navigation de référence</span><span class="sxs-lookup"><span data-stu-id="502e6-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="502e6-126">`Blog.Posts`est une propriété de navigation de collection</span><span class="sxs-lookup"><span data-stu-id="502e6-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="502e6-127">`Post.Blog`la propriété de navigation inversée de `Blog.Posts` (et vice versa)</span><span class="sxs-lookup"><span data-stu-id="502e6-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="502e6-128">Conventions</span><span class="sxs-lookup"><span data-stu-id="502e6-128">Conventions</span></span>

<span data-ttu-id="502e6-129">Par convention, une relation sera créée lors d’une propriété de navigation détectée sur un type.</span><span class="sxs-lookup"><span data-stu-id="502e6-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="502e6-130">Une propriété est considérée comme une propriété de navigation si le type vers lequel elle pointe ne peut pas être mappé en tant que type scalaire par le fournisseur actuel de la base de données.</span><span class="sxs-lookup"><span data-stu-id="502e6-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="502e6-131">Les relations qui sont découverts par convention ciblera toujours la clé primaire de l’entité principale.</span><span class="sxs-lookup"><span data-stu-id="502e6-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="502e6-132">Pour cibler une autre clé, une configuration supplémentaire doit être effectuée à l’aide de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="502e6-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="502e6-133">Relations entièrement définies</span><span class="sxs-lookup"><span data-stu-id="502e6-133">Fully Defined Relationships</span></span>

<span data-ttu-id="502e6-134">Le modèle le plus courant pour les relations est d’avoir des propriétés de navigation définies sur les deux extrémités de la relation et une propriété de clé étrangère définie dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="502e6-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="502e6-135">Si une paire de propriétés de navigation se trouve entre deux types, ils seront configurés en tant que propriétés de navigation inverse de la même relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="502e6-136">Si l’entité dépendante contient une propriété nommée `<primary key property name>`, `<navigation property name><primary key property name>`, ou `<principal entity name><primary key property name>` , puis il sera configuré en tant que la clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="502e6-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="502e6-137">S’il existe plusieurs propriétés de navigation définies entre deux types (autrement dit, plusieurs paires distinctes de navigations qui pointent vers l’autre), alors aucune relation ne sera créée par convention, et vous devez les configurer manuellement pour identifier la propriétés de navigation par deux.</span><span class="sxs-lookup"><span data-stu-id="502e6-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="502e6-138">Aucune propriété de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="502e6-138">No Foreign Key Property</span></span>

<span data-ttu-id="502e6-139">S’il est recommandé d’avoir une propriété de clé étrangère définie dans la classe d’entité dépendante, il n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="502e6-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="502e6-140">Si aucune propriété de clé étrangère n’est trouvée, une propriété de clé étrangère de clichés instantanés est introduite avec le nom `<navigation property name><principal key property name>` (consultez [occulter les propriétés](shadow-properties.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="502e6-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="502e6-141">Propriété de Navigation</span><span class="sxs-lookup"><span data-stu-id="502e6-141">Single Navigation Property</span></span>

<span data-ttu-id="502e6-142">Notamment qu’une propriété de navigation (aucune navigation inverse et aucune propriété de clé étrangère) est suffisante pour permettre une relation définie par convention.</span><span class="sxs-lookup"><span data-stu-id="502e6-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="502e6-143">Vous pouvez également avoir une propriété de navigation et une propriété de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="502e6-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="502e6-144">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="502e6-144">Cascade Delete</span></span>

<span data-ttu-id="502e6-145">Par convention, la suppression en cascade est définie sur *Cascade* pour les relations requises et *ClientSetNull* pour les relations facultatif.</span><span class="sxs-lookup"><span data-stu-id="502e6-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="502e6-146">*Cascade* signifie entités dépendantes sont également supprimées.</span><span class="sxs-lookup"><span data-stu-id="502e6-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="502e6-147">*ClientSetNull* signifie que les entités dépendantes ne sont pas chargées en mémoire reste inchangée et doit être supprimés manuellement ou mis à jour pour pointer vers une entité principale valide.</span><span class="sxs-lookup"><span data-stu-id="502e6-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="502e6-148">Pour les entités qui sont chargées en mémoire, EF Core tente de définir les propriétés de clé étrangères sur null.</span><span class="sxs-lookup"><span data-stu-id="502e6-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="502e6-149">Consultez le [relations obligatoires et facultatifs](#required-and-optional-relationships) section de la différence entre les relations requises et facultatives.</span><span class="sxs-lookup"><span data-stu-id="502e6-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="502e6-150">Consultez [suppression en Cascade](../saving/cascade-delete.md) pour plus d’informations sur les différents comportements et les valeurs par défaut utilisés par la convention de suppression.</span><span class="sxs-lookup"><span data-stu-id="502e6-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="502e6-151">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="502e6-151">Data Annotations</span></span>

<span data-ttu-id="502e6-152">Il existe deux des annotations de données qui peuvent être utilisées pour configurer des relations, `[ForeignKey]` et `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="502e6-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="502e6-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="502e6-153">[ForeignKey]</span></span>

<span data-ttu-id="502e6-154">Vous pouvez utiliser les Annotations de données pour configurer la propriété doit être utilisée comme propriété de clé étrangère pour une relation donnée.</span><span class="sxs-lookup"><span data-stu-id="502e6-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="502e6-155">Cela est généralement le cas lorsque la propriété de clé étrangère n’est pas découvert par convention.</span><span class="sxs-lookup"><span data-stu-id="502e6-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="502e6-156">Le `[ForeignKey]` annotation peut être placée sur une propriété de navigation dans la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="502e6-157">Il n’a pas besoin de passer la propriété de navigation dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="502e6-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="502e6-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="502e6-158">[InverseProperty]</span></span>

<span data-ttu-id="502e6-159">Vous pouvez utiliser les Annotations de données pour configurer la façon dont les propriétés de navigation sur les entités principales et dépendantes par deux.</span><span class="sxs-lookup"><span data-stu-id="502e6-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="502e6-160">Cela est généralement le cas lorsqu’il y a plus d’une paire de propriétés de navigation entre les deux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="502e6-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="502e6-161">API Fluent</span><span class="sxs-lookup"><span data-stu-id="502e6-161">Fluent API</span></span>

<span data-ttu-id="502e6-162">Pour configurer une relation dans l’API Fluent, commencez par identifier les propriétés de navigation qui composent la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="502e6-163">`HasOne`ou `HasMany` identifie la propriété de navigation sur le type d’entité vous sont à partir de la configuration.</span><span class="sxs-lookup"><span data-stu-id="502e6-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="502e6-164">Vous liez ensuite un appel à `WithOne` ou `WithMany` pour identifier le volet de navigation inverse.</span><span class="sxs-lookup"><span data-stu-id="502e6-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="502e6-165">`HasOne`/`WithOne`sont utilisés pour les propriétés de navigation de référence et `HasMany` / `WithMany` sont utilisés pour les propriétés de navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="502e6-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="502e6-166">Propriété de Navigation</span><span class="sxs-lookup"><span data-stu-id="502e6-166">Single Navigation Property</span></span>

<span data-ttu-id="502e6-167">Si vous avez uniquement une propriété de navigation, les surcharges sans paramètre de `WithOne` et `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="502e6-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="502e6-168">Cela indique qu’il le concept est une référence ou une collection à l’autre extrémité de la relation, mais il n’existe aucune propriété de navigation incluse dans la classe d’entité.</span><span class="sxs-lookup"><span data-stu-id="502e6-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="502e6-169">Clé étrangère</span><span class="sxs-lookup"><span data-stu-id="502e6-169">Foreign Key</span></span>

<span data-ttu-id="502e6-170">Vous pouvez utiliser l’API Fluent pour configurer la propriété doit être utilisée comme propriété de clé étrangère pour une relation donnée.</span><span class="sxs-lookup"><span data-stu-id="502e6-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="502e6-171">Le code suivant montre comment configurer une clé étrangère composite.</span><span class="sxs-lookup"><span data-stu-id="502e6-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="502e6-172">Vous pouvez utiliser la surcharge de chaîne de `HasForeignKey(...)` pour configurer une propriété de clichés instantanés comme une clé étrangère (voir [occulter les propriétés](shadow-properties.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="502e6-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="502e6-173">Nous vous recommandons d’ajouter explicitement la propriété de clichés instantanés pour le modèle avant de l’utiliser comme une clé étrangère (comme indiqué ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="502e6-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="502e6-174">Clé d’entité de sécurité</span><span class="sxs-lookup"><span data-stu-id="502e6-174">Principal Key</span></span>

<span data-ttu-id="502e6-175">Si vous souhaitez que la clé étrangère à une propriété autre que la clé primaire, vous pouvez utiliser l’API Fluent pour configurer la propriété de clé principale de la relation.</span><span class="sxs-lookup"><span data-stu-id="502e6-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="502e6-176">La propriété que vous configurez en tant que le principal est clé automatiquement être configuré en tant qu’une autre clé (voir [clés secondaires](alternate-keys.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="502e6-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="502e6-177">Le code suivant montre comment configurer une clé principale.</span><span class="sxs-lookup"><span data-stu-id="502e6-177">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="502e6-178">L’ordre dans lequel vous spécifiez les propriétés de clé principales doit correspondre à l’ordre dans lequel elles sont spécifiées pour la clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="502e6-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="502e6-179">Relations obligatoires et facultatifs</span><span class="sxs-lookup"><span data-stu-id="502e6-179">Required and Optional Relationships</span></span>

<span data-ttu-id="502e6-180">Vous pouvez utiliser l’API Fluent pour déterminer si la relation est requise ou facultative.</span><span class="sxs-lookup"><span data-stu-id="502e6-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="502e6-181">Il décide si la propriété de clé étrangère est obligatoire ou facultatif.</span><span class="sxs-lookup"><span data-stu-id="502e6-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="502e6-182">Cela est particulièrement utile lorsque vous utilisez une clé étrangère état de clichés instantanés.</span><span class="sxs-lookup"><span data-stu-id="502e6-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="502e6-183">Si vous disposez d’une propriété de clé étrangère dans votre classe d’entité, puis le requiredness de la relation est déterminée selon que la propriété de clé étrangère est obligatoire ou facultatif (voir [propriétés obligatoires et facultatifs](required-optional.md) pour plus d’informations plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="502e6-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="502e6-184">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="502e6-184">Cascade Delete</span></span>

<span data-ttu-id="502e6-185">Vous pouvez utiliser l’API Fluent pour configurer le comportement de suppression en cascade pour une relation donnée explicitement.</span><span class="sxs-lookup"><span data-stu-id="502e6-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="502e6-186">Consultez [suppression en Cascade](../saving/cascade-delete.md) dans la section de l’enregistrement des données pour une présentation détaillée de chaque option.</span><span class="sxs-lookup"><span data-stu-id="502e6-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="502e6-187">Autres modèles de relation</span><span class="sxs-lookup"><span data-stu-id="502e6-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="502e6-188">Un à un</span><span class="sxs-lookup"><span data-stu-id="502e6-188">One-to-one</span></span>

<span data-ttu-id="502e6-189">Les relations un à un ont une propriété de navigation de référence sur les deux côtés.</span><span class="sxs-lookup"><span data-stu-id="502e6-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="502e6-190">Elles suivent les mêmes conventions que les relations un-à-plusieurs, mais un index unique est présentée sur la propriété de clé étrangère pour vous assurer qu’un seul dépendant est lié à chaque principal.</span><span class="sxs-lookup"><span data-stu-id="502e6-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="502e6-191">EF choisira l’une des entités pour être dépendantes en fonction de sa capacité à détecter une propriété de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="502e6-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="502e6-192">Si l’entité incorrecte est choisie comme dépendantes, vous pouvez utiliser l’API Fluent pour corriger ce problème.</span><span class="sxs-lookup"><span data-stu-id="502e6-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="502e6-193">Lors de la configuration de la relation avec l’API Fluent, vous utilisez la `HasOne` et `WithOne` méthodes.</span><span class="sxs-lookup"><span data-stu-id="502e6-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="502e6-194">Lors de la configuration de la clé étrangère, vous devez spécifier le type d’entité dépendante - Notez le paramètre générique fourni pour `HasForeignKey` dans la liste ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="502e6-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="502e6-195">Dans une relation un-à-plusieurs, il est clair que l’entité avec le volet de navigation référence dépend et celle de la collection est le principal.</span><span class="sxs-lookup"><span data-stu-id="502e6-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="502e6-196">Mais ce n’est pas dans une relation un à un - par conséquent, la nécessité pour le définir explicitement.</span><span class="sxs-lookup"><span data-stu-id="502e6-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="502e6-197">Plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="502e6-197">Many-to-many</span></span>

<span data-ttu-id="502e6-198">Relations plusieurs-à-plusieurs sans une classe d’entité pour représenter la table de jointure ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="502e6-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="502e6-199">Toutefois, vous pouvez représenter une relation plusieurs-à-plusieurs en incluant une classe d’entité pour la table de jointure et de deux relations un-à-plusieurs distinct mappage.</span><span class="sxs-lookup"><span data-stu-id="502e6-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
