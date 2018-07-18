---
title: Conventions Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120815"
---
# <a name="code-first-conventions"></a><span data-ttu-id="676b4-102">Conventions Code First</span><span class="sxs-lookup"><span data-stu-id="676b4-102">Code First Conventions</span></span>
<span data-ttu-id="676b4-103">Code First vous permet de décrire un modèle à l’aide de classes c# ou Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="676b4-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="676b4-104">La forme de base du modèle est détectée à l’aide des conventions.</span><span class="sxs-lookup"><span data-stu-id="676b4-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="676b4-105">Conventions des ensembles de règles qui sont utilisés pour configurer automatiquement un modèle conceptuel basé sur les définitions de classe lorsque vous travaillez avec Code First.</span><span class="sxs-lookup"><span data-stu-id="676b4-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="676b4-106">Les conventions sont définies dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="676b4-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="676b4-107">Vous pouvez configurer davantage de votre modèle à l’aide des annotations de données ou l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="676b4-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="676b4-108">La priorité est donnée à la configuration via l’API fluent, suivi de conventions et des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="676b4-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="676b4-109">Pour plus d’informations, consultez [Annotations de données](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relations](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - Types de & propriétés](~/ef6/modeling/code-first/fluent/types-and-properties.md) et [API Fluent avec VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="676b4-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="676b4-110">Une liste détaillée des conventions Code First est disponible dans le [Documentation de l’API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="676b4-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="676b4-111">Cette rubrique fournit une vue d’ensemble des conventions utilisées par Code First.</span><span class="sxs-lookup"><span data-stu-id="676b4-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="676b4-112">Découverte de type</span><span class="sxs-lookup"><span data-stu-id="676b4-112">Type Discovery</span></span>  

<span data-ttu-id="676b4-113">Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine).</span><span class="sxs-lookup"><span data-stu-id="676b4-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="676b4-114">En plus de définir les classes, vous devez également communiquer à **DbContext** connaître les types que vous souhaitez inclure dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="676b4-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="676b4-115">Pour ce faire, vous définissez une classe de contexte qui dérive de **DbContext** et expose **DbSet** propriétés pour les types que vous souhaitez faire partie du modèle.</span><span class="sxs-lookup"><span data-stu-id="676b4-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="676b4-116">Code inclut tout d’abord ces types et également permet d’extraire tous les types référencés, même si les types référencés sont définis dans un autre assembly.</span><span class="sxs-lookup"><span data-stu-id="676b4-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="676b4-117">Si vos types de participent à une hiérarchie d’héritage, il suffit de définir un **DbSet** propriété pour la classe de base et les types dérivés qui sera incluse automatiquement, si elles se trouvent dans le même assembly que la classe de base.</span><span class="sxs-lookup"><span data-stu-id="676b4-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="676b4-118">Dans l’exemple suivant, il existe un seul **DbSet** propriété définie sur le **SchoolEntities** classe (**départements**).</span><span class="sxs-lookup"><span data-stu-id="676b4-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="676b4-119">Code utilise cette propriété pour découvrir et d’extraire tous les types référencés.</span><span class="sxs-lookup"><span data-stu-id="676b4-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="676b4-120">Si vous souhaitez exclure un type à partir du modèle, utilisez le **NotMapped** attribut ou le **DbModelBuilder.Ignore** API fluent.</span><span class="sxs-lookup"><span data-stu-id="676b4-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="676b4-121">Convention de clé primaire</span><span class="sxs-lookup"><span data-stu-id="676b4-121">Primary Key Convention</span></span>  

<span data-ttu-id="676b4-122">Code déduit tout d’abord qu’une propriété est une clé primaire si une propriété sur une classe est nommée « ID » (non sensible à la casse), ou le nom de classe suivie de « ID ».</span><span class="sxs-lookup"><span data-stu-id="676b4-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="676b4-123">Si le type de la propriété de clé primaire est numérique ou le GUID sera configuré comme une colonne d’identité.</span><span class="sxs-lookup"><span data-stu-id="676b4-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="676b4-124">Convention de relation</span><span class="sxs-lookup"><span data-stu-id="676b4-124">Relationship Convention</span></span>  

<span data-ttu-id="676b4-125">Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="676b4-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="676b4-126">Chaque objet peut avoir une propriété de navigation pour chaque relation à laquelle il participe.</span><span class="sxs-lookup"><span data-stu-id="676b4-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="676b4-127">Propriétés de navigation permettent de parcourir et de gérer les relations dans les deux directions, en retournant un objet de la référence (si la multiplicité est soit un ou zéro-ou-un) ou une collection (si la multiplicité est nombreuses).</span><span class="sxs-lookup"><span data-stu-id="676b4-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="676b4-128">Code déduit tout d’abord les relations basées sur les propriétés de navigation définies sur vos types.</span><span class="sxs-lookup"><span data-stu-id="676b4-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="676b4-129">En plus des propriétés de navigation, nous vous recommandons d’inclure des propriétés de clé étrangère sur les types qui représentent des objets dépendants.</span><span class="sxs-lookup"><span data-stu-id="676b4-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="676b4-130">N’importe quelle propriété avec le même type de données en tant que propriété de clé primaire principal et avec un nom qui suit l’un des formats suivants représente une clé étrangère pour la relation : «\<nom de propriété de navigation\>\<principal nom de propriété de clé primaire\>','\<nom de la classe principal\>\<nom de propriété de clé primaire\>», ou «\<nom de propriété de clé primaire principal\>».</span><span class="sxs-lookup"><span data-stu-id="676b4-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="676b4-131">Si plusieurs correspondances sont trouvées priorité est donnée dans l’ordre indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="676b4-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="676b4-132">La détection de clé étrangère n’est pas sensible à la casse.</span><span class="sxs-lookup"><span data-stu-id="676b4-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="676b4-133">Lorsqu’une propriété de clé étrangère est détectée, Code First déduit la multiplicité de la relation en fonction de la possibilité de valeur de la clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="676b4-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="676b4-134">Si la propriété est nullable, la relation est enregistrée comme étant facultatifs ; Sinon, la relation est inscrit en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="676b4-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="676b4-135">Si une clé étrangère sur l’entité dépendante n’est pas nullable, Code First définit ensuite suppression en cascade sur la relation.</span><span class="sxs-lookup"><span data-stu-id="676b4-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="676b4-136">Si une clé étrangère sur l’entité dépendante est nullable, Code First ne définit pas suppression en cascade sur la relation, et lorsque le principal est supprimé la clé étrangère sera définie sur null.</span><span class="sxs-lookup"><span data-stu-id="676b4-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="676b4-137">La multiplicité et cascade delete comportement détecté par convention peut être substituée à l’aide de l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="676b4-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="676b4-138">Dans l’exemple suivant, les propriétés de navigation et une clé étrangère sont utilisés pour définir la relation entre les classes de service et un cours.</span><span class="sxs-lookup"><span data-stu-id="676b4-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="676b4-139">Si vous avez plusieurs relations entre les mêmes types (par exemple, supposons que vous définissiez le **personne** et **livre** classes, où le **personne** classe contient le  **ReviewedBooks** et **AuthoredBooks** propriétés de navigation et la **livre** classe contient le **auteur** et  **Réviseur** propriétés de navigation) vous devez configurer manuellement les relations à l’aide des Annotations de données ou l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="676b4-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="676b4-140">Pour plus d’informations, consultez [Annotations de données - relations](~/ef6/modeling/code-first/data-annotations.md) et [API Fluent - relations](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="676b4-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="676b4-141">Convention de Types complexes</span><span class="sxs-lookup"><span data-stu-id="676b4-141">Complex Types Convention</span></span>  

<span data-ttu-id="676b4-142">Lorsque Code First détecte une définition de classe où une clé primaire ne peut pas être déduite, et aucune clé primaire n’est inscrit par le biais des annotations de données ou l’API fluent, puis le type est automatiquement enregistré comme un type complexe.</span><span class="sxs-lookup"><span data-stu-id="676b4-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="676b4-143">Détection de type complexe requiert également que le type n’a pas de propriétés qui référencent des types d’entités et ne sont pas référencées à partir d’une propriété de collection sur un autre type.</span><span class="sxs-lookup"><span data-stu-id="676b4-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="676b4-144">Compte tenu des définitions de classe suivant Code First serait déduire que **détails** est un type complexe, car il n’a aucune clé primaire.</span><span class="sxs-lookup"><span data-stu-id="676b4-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="676b4-145">Convention de chaîne de connexion</span><span class="sxs-lookup"><span data-stu-id="676b4-145">Connection String Convention</span></span>  

<span data-ttu-id="676b4-146">Pour en savoir plus sur les conventions que DbContext utilise pour détecter la connexion à utiliser voir [connexions et des modèles](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="676b4-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="676b4-147">Suppression des Conventions</span><span class="sxs-lookup"><span data-stu-id="676b4-147">Removing Conventions</span></span>  

<span data-ttu-id="676b4-148">Vous pouvez supprimer une des conventions définies dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="676b4-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="676b4-149">L’exemple suivant supprime **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="676b4-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="676b4-150">Conventions personnalisées</span><span class="sxs-lookup"><span data-stu-id="676b4-150">Custom Conventions</span></span>  

<span data-ttu-id="676b4-151">Conventions personnalisées sont prises en charge dans EF6 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="676b4-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="676b4-152">Pour plus d’informations, consultez [Conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="676b4-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
