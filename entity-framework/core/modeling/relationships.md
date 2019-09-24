---
title: Relations-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197652"
---
# <a name="relationships"></a><span data-ttu-id="77923-102">Relations</span><span class="sxs-lookup"><span data-stu-id="77923-102">Relationships</span></span>

<span data-ttu-id="77923-103">Une relation définit la relation entre deux entités.</span><span class="sxs-lookup"><span data-stu-id="77923-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="77923-104">Dans une base de données relationnelle, il est représenté par une contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="77923-105">La plupart des exemples de cet article utilisent une relation un-à-plusieurs pour illustrer les concepts.</span><span class="sxs-lookup"><span data-stu-id="77923-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="77923-106">Pour obtenir des exemples de relations un-à-un et plusieurs-à-plusieurs, consultez la section [autres modèles de relation](#other-relationship-patterns) à la fin de l’article.</span><span class="sxs-lookup"><span data-stu-id="77923-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="77923-107">Définition des termes</span><span class="sxs-lookup"><span data-stu-id="77923-107">Definition of Terms</span></span>

<span data-ttu-id="77923-108">Il existe un certain nombre de termes utilisés pour décrire les relations</span><span class="sxs-lookup"><span data-stu-id="77923-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="77923-109">**Entité dépendante :** Il s’agit de l’entité qui contient la ou les propriétés de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="77923-110">Parfois appelé « enfant » de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="77923-111">**Entité principale :** Il s’agit de l’entité qui contient la ou les propriétés de clé primaire/secondaire.</span><span class="sxs-lookup"><span data-stu-id="77923-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="77923-112">Parfois appelé « parent » de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="77923-113">**Clé étrangère :** Propriété (s) de l’entité dépendante utilisée pour stocker les valeurs de la propriété de clé principale à laquelle l’entité est associée.</span><span class="sxs-lookup"><span data-stu-id="77923-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="77923-114">**Clé principale :** La ou les propriétés qui identifient de façon unique l’entité principale.</span><span class="sxs-lookup"><span data-stu-id="77923-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="77923-115">Il peut s’agir de la clé primaire ou d’une clé secondaire.</span><span class="sxs-lookup"><span data-stu-id="77923-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="77923-116">**Propriété de navigation :** Propriété définie sur le principal et/ou l’entité dépendante qui contient une ou plusieurs références aux entités associées.</span><span class="sxs-lookup"><span data-stu-id="77923-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="77923-117">**Propriété de navigation de la collection :** Propriété de navigation qui contient des références à de nombreuses entités associées.</span><span class="sxs-lookup"><span data-stu-id="77923-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="77923-118">**Propriété de navigation de référence :** Propriété de navigation qui contient une référence à une entité associée unique.</span><span class="sxs-lookup"><span data-stu-id="77923-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="77923-119">**Propriété de navigation inverse :** Lorsque vous discutez d’une propriété de navigation particulière, ce terme fait référence à la propriété de navigation à l’autre extrémité de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="77923-120">La liste de code suivante montre une relation un-à-plusieurs `Blog` entre et`Post`</span><span class="sxs-lookup"><span data-stu-id="77923-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="77923-121">`Post`est l’entité dépendante</span><span class="sxs-lookup"><span data-stu-id="77923-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="77923-122">`Blog`est l’entité principale</span><span class="sxs-lookup"><span data-stu-id="77923-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="77923-123">`Post.BlogId`est la clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77923-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="77923-124">`Blog.BlogId`est la clé principale (dans le cas présent, il s’agit d’une clé primaire plutôt que d’une clé secondaire)</span><span class="sxs-lookup"><span data-stu-id="77923-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="77923-125">`Post.Blog`est une propriété de navigation de référence</span><span class="sxs-lookup"><span data-stu-id="77923-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="77923-126">`Blog.Posts`est une propriété de navigation de collection</span><span class="sxs-lookup"><span data-stu-id="77923-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="77923-127">`Post.Blog`est la propriété de navigation inverse `Blog.Posts` de (et vice versa)</span><span class="sxs-lookup"><span data-stu-id="77923-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="77923-128">Conventions</span><span class="sxs-lookup"><span data-stu-id="77923-128">Conventions</span></span>

<span data-ttu-id="77923-129">Par Convention, une relation est créée lorsqu’une propriété de navigation est détectée sur un type.</span><span class="sxs-lookup"><span data-stu-id="77923-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="77923-130">Une propriété est considérée comme une propriété de navigation si le type vers lequel elle pointe ne peut pas être mappé en tant que type scalaire par le fournisseur de base de données actuel.</span><span class="sxs-lookup"><span data-stu-id="77923-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="77923-131">Les relations découvertes par convention cibleront toujours la clé primaire de l’entité principale.</span><span class="sxs-lookup"><span data-stu-id="77923-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="77923-132">Pour cibler une clé secondaire, une configuration supplémentaire doit être effectuée à l’aide de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="77923-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="77923-133">Relations entièrement définies</span><span class="sxs-lookup"><span data-stu-id="77923-133">Fully Defined Relationships</span></span>

<span data-ttu-id="77923-134">Le modèle le plus courant pour les relations est d’avoir des propriétés de navigation définies aux deux extrémités de la relation et une propriété de clé étrangère définie dans la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="77923-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="77923-135">Si une paire de propriétés de navigation est trouvée entre deux types, ils sont configurés en tant que propriétés de navigation inverse de la même relation.</span><span class="sxs-lookup"><span data-stu-id="77923-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="77923-136">Si l’entité dépendante contient une `<primary key property name>`propriété `<navigation property name><primary key property name>`nommée, `<principal entity name><primary key property name>` , ou si elle est configurée en tant que clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="77923-137">S’il existe plusieurs propriétés de navigation définies entre deux types (autrement dit, plusieurs paires distinctes de navigation qui pointent les unes vers les autres), aucune relation ne sera créée par Convention et vous devrez les configurer manuellement pour identifier la façon dont le les propriétés de navigation sont couplées.</span><span class="sxs-lookup"><span data-stu-id="77923-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="77923-138">Aucune propriété de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77923-138">No Foreign Key Property</span></span>

<span data-ttu-id="77923-139">Bien qu’il soit recommandé d’avoir une propriété de clé étrangère définie dans la classe d’entité dépendante, elle n’est pas obligatoire.</span><span class="sxs-lookup"><span data-stu-id="77923-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="77923-140">Si aucune propriété de clé étrangère n’est trouvée, une propriété de clé étrangère Shadow est introduite `<navigation property name><principal key property name>` avec le nom (pour plus d’informations, consultez [Propriétés Shadow](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="77923-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="77923-141">Propriété de navigation unique</span><span class="sxs-lookup"><span data-stu-id="77923-141">Single Navigation Property</span></span>

<span data-ttu-id="77923-142">L’inclusion d’une seule propriété de navigation (aucune navigation inverse et aucune propriété de clé étrangère) suffit à avoir une relation définie par Convention.</span><span class="sxs-lookup"><span data-stu-id="77923-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="77923-143">Vous pouvez également avoir une propriété de navigation unique et une propriété de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="77923-144">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="77923-144">Cascade Delete</span></span>

<span data-ttu-id="77923-145">Par Convention, la suppression en cascade sera définie sur *cascade* pour les relations requises et *ClientSetNull* pour les relations facultatives.</span><span class="sxs-lookup"><span data-stu-id="77923-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="77923-146">*Cascade* signifie que les entités dépendantes sont également supprimées.</span><span class="sxs-lookup"><span data-stu-id="77923-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="77923-147">*ClientSetNull* signifie que les entités dépendantes qui ne sont pas chargées en mémoire restent inchangées et doivent être supprimées manuellement ou mises à jour pour pointer vers une entité principale valide.</span><span class="sxs-lookup"><span data-stu-id="77923-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="77923-148">Pour les entités chargées en mémoire, EF Core tente de définir les propriétés de clé étrangère sur la valeur null.</span><span class="sxs-lookup"><span data-stu-id="77923-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="77923-149">Consultez la section [relations obligatoires et facultatives](#required-and-optional-relationships) pour connaître la différence entre les relations obligatoires et facultatives.</span><span class="sxs-lookup"><span data-stu-id="77923-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="77923-150">Pour plus d’informations sur les différents comportements de suppression et les valeurs par défaut utilisées par Convention, consultez [suppression en cascade](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="77923-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="77923-151">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="77923-151">Data Annotations</span></span>

<span data-ttu-id="77923-152">Il existe deux annotations de données qui peuvent être utilisées pour configurer `[ForeignKey]` des `[InverseProperty]`relations, et.</span><span class="sxs-lookup"><span data-stu-id="77923-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="77923-153">Celles-ci sont disponibles `System.ComponentModel.DataAnnotations.Schema` dans l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="77923-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="77923-154">ForeignKey</span><span class="sxs-lookup"><span data-stu-id="77923-154">[ForeignKey]</span></span>

<span data-ttu-id="77923-155">Vous pouvez utiliser les annotations de données pour configurer la propriété à utiliser comme propriété de clé étrangère pour une relation donnée.</span><span class="sxs-lookup"><span data-stu-id="77923-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="77923-156">Cela s’effectue généralement lorsque la propriété de clé étrangère n’est pas détectée par Convention.</span><span class="sxs-lookup"><span data-stu-id="77923-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="77923-157">L' `[ForeignKey]` annotation peut être placée sur l’une des propriétés de navigation de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="77923-158">Elle n’a pas besoin de se trouver dans la propriété de navigation de la classe d’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="77923-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="77923-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="77923-159">[InverseProperty]</span></span>

<span data-ttu-id="77923-160">Vous pouvez utiliser les annotations de données pour configurer la combinaison des propriétés de navigation sur les entités dépendantes et principales.</span><span class="sxs-lookup"><span data-stu-id="77923-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="77923-161">Cela s’effectue généralement quand il existe plusieurs paires de propriétés de navigation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="77923-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="77923-162">API Fluent</span><span class="sxs-lookup"><span data-stu-id="77923-162">Fluent API</span></span>

<span data-ttu-id="77923-163">Pour configurer une relation dans l’API Fluent, commencez par identifier les propriétés de navigation qui composent la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="77923-164">`HasOne`ou `HasMany` identifie la propriété de navigation sur le type d’entité sur lequel vous commencez la configuration.</span><span class="sxs-lookup"><span data-stu-id="77923-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="77923-165">Vous enchaînez ensuite un `WithOne` appel `WithMany` à ou pour identifier la navigation inverse.</span><span class="sxs-lookup"><span data-stu-id="77923-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="77923-166">`HasOne`/`WithOne`sont utilisés pour les propriétés de navigation `HasMany` de référence et / `WithMany` sont utilisés pour les propriétés de navigation de collection.</span><span class="sxs-lookup"><span data-stu-id="77923-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="77923-167">Propriété de navigation unique</span><span class="sxs-lookup"><span data-stu-id="77923-167">Single Navigation Property</span></span>

<span data-ttu-id="77923-168">Si vous n’avez qu’une seule propriété de navigation, il y a des surcharges sans paramètre de `WithOne` et. `WithMany`</span><span class="sxs-lookup"><span data-stu-id="77923-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="77923-169">Cela indique qu’il existe conceptuellement une référence ou une collection à l’autre extrémité de la relation, mais qu’il n’y a aucune propriété de navigation incluse dans la classe d’entité.</span><span class="sxs-lookup"><span data-stu-id="77923-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="77923-170">Clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77923-170">Foreign Key</span></span>

<span data-ttu-id="77923-171">Vous pouvez utiliser l’API Fluent pour configurer la propriété à utiliser comme propriété de clé étrangère pour une relation donnée.</span><span class="sxs-lookup"><span data-stu-id="77923-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="77923-172">La liste de code suivante montre comment configurer une clé étrangère composite.</span><span class="sxs-lookup"><span data-stu-id="77923-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="77923-173">Vous pouvez utiliser la surcharge de chaîne `HasForeignKey(...)` de pour configurer une propriété Shadow comme clé étrangère (pour plus d’informations, consultez [Propriétés Shadow](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="77923-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="77923-174">Nous vous recommandons d’ajouter explicitement la propriété Shadow au modèle avant de l’utiliser comme clé étrangère (comme indiqué ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="77923-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="77923-175">Sans propriété de navigation</span><span class="sxs-lookup"><span data-stu-id="77923-175">Without Navigation Property</span></span>

<span data-ttu-id="77923-176">Vous n’avez pas nécessairement besoin de fournir une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="77923-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="77923-177">Vous pouvez simplement fournir une clé étrangère d’un côté de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="77923-178">Clé principale</span><span class="sxs-lookup"><span data-stu-id="77923-178">Principal Key</span></span>

<span data-ttu-id="77923-179">Si vous souhaitez que la clé étrangère fasse référence à une propriété autre que la clé primaire, vous pouvez utiliser l’API Fluent pour configurer la propriété de clé principale de la relation.</span><span class="sxs-lookup"><span data-stu-id="77923-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="77923-180">La propriété que vous configurez comme clé principale est automatiquement configurée en tant que clé secondaire (pour plus d’informations, consultez [clés alternatives](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="77923-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<span data-ttu-id="77923-181">L’exemple de code suivant montre comment configurer une clé principale composite.</span><span class="sxs-lookup"><span data-stu-id="77923-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> <span data-ttu-id="77923-182">L’ordre dans lequel vous spécifiez les propriétés de clé principale doit correspondre à l’ordre dans lequel elles sont spécifiées pour la clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="77923-183">Relations obligatoires et facultatives</span><span class="sxs-lookup"><span data-stu-id="77923-183">Required and Optional Relationships</span></span>

<span data-ttu-id="77923-184">Vous pouvez utiliser l’API Fluent pour déterminer si la relation est obligatoire ou facultative.</span><span class="sxs-lookup"><span data-stu-id="77923-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="77923-185">Au final, cela contrôle si la propriété de clé étrangère est obligatoire ou facultative.</span><span class="sxs-lookup"><span data-stu-id="77923-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="77923-186">Cela est particulièrement utile lorsque vous utilisez une clé étrangère d’État Shadow.</span><span class="sxs-lookup"><span data-stu-id="77923-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="77923-187">Si vous avez une propriété de clé étrangère dans votre classe d’entité, le caractère requis de la relation est déterminé selon que la propriété de clé étrangère est obligatoire ou facultative (pour plus d’informations, consultez [propriétés requises et facultatives](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="77923-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

### <a name="cascade-delete"></a><span data-ttu-id="77923-188">Suppression en cascade</span><span class="sxs-lookup"><span data-stu-id="77923-188">Cascade Delete</span></span>

<span data-ttu-id="77923-189">Vous pouvez utiliser l’API Fluent pour configurer explicitement le comportement de suppression en cascade pour une relation donnée.</span><span class="sxs-lookup"><span data-stu-id="77923-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="77923-190">Pour une présentation détaillée de chaque option, consultez [suppression en cascade](../saving/cascade-delete.md) dans la section enregistrement des données.</span><span class="sxs-lookup"><span data-stu-id="77923-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a><span data-ttu-id="77923-191">Autres modèles de relation</span><span class="sxs-lookup"><span data-stu-id="77923-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="77923-192">Un-à-un</span><span class="sxs-lookup"><span data-stu-id="77923-192">One-to-one</span></span>

<span data-ttu-id="77923-193">Les relations un-à-un ont une propriété de navigation de référence des deux côtés.</span><span class="sxs-lookup"><span data-stu-id="77923-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="77923-194">Ils suivent les mêmes conventions que les relations un-à-plusieurs, mais un index unique est introduit sur la propriété de clé étrangère pour s’assurer qu’un seul dépendant est lié à chaque principal.</span><span class="sxs-lookup"><span data-stu-id="77923-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> <span data-ttu-id="77923-195">EF choisit l’une des entités à dépendre en fonction de sa capacité à détecter une propriété de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="77923-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="77923-196">Si l’entité incorrecte est choisie comme dépendant, vous pouvez utiliser l’API Fluent pour corriger ce problème.</span><span class="sxs-lookup"><span data-stu-id="77923-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="77923-197">Quand vous configurez la relation avec l’API Fluent, vous `HasOne` utilisez `WithOne` les méthodes et.</span><span class="sxs-lookup"><span data-stu-id="77923-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="77923-198">Quand vous configurez la clé étrangère, vous devez spécifier le type d’entité dépendant. Notez le paramètre `HasForeignKey` générique fourni à dans la liste ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="77923-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="77923-199">Dans une relation un-à-plusieurs, il est clair que l’entité avec la navigation de référence est la dépendance et celle avec la collection est le principal.</span><span class="sxs-lookup"><span data-stu-id="77923-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="77923-200">Mais ce n’est pas le cas dans une relation un-à-un, c’est pourquoi il est nécessaire de la définir explicitement.</span><span class="sxs-lookup"><span data-stu-id="77923-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a><span data-ttu-id="77923-201">Plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="77923-201">Many-to-many</span></span>

<span data-ttu-id="77923-202">Les relations plusieurs-à-plusieurs sans classe d’entité pour représenter la table de jointure ne sont pas encore prises en charge.</span><span class="sxs-lookup"><span data-stu-id="77923-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="77923-203">Toutefois, vous pouvez représenter une relation plusieurs-à-plusieurs en incluant une classe d’entité pour la table de jointure et en mappant deux relations un-à-plusieurs distinctes.</span><span class="sxs-lookup"><span data-stu-id="77923-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
