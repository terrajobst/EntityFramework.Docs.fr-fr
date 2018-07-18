---
title: Annotations de données First - EF6 de code
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121487"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="08c93-102">Annotations de données Code First</span><span class="sxs-lookup"><span data-stu-id="08c93-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="08c93-103">**EF4.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="08c93-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="08c93-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="08c93-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="08c93-105">Le contenu de cette page est une adaptation d’et que l’article ont été écrit par Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="08c93-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="08c93-106">Entity Framework Code First vous permet d’utiliser vos propres classes de domaine pour représenter le modèle EF reposant sur pour exécuter des requêtes sur, modifier le suivi et la mise à jour des fonctions.</span><span class="sxs-lookup"><span data-stu-id="08c93-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="08c93-107">Code exploite tout d’abord un modèle de programmation appelé convention sur configuration.</span><span class="sxs-lookup"><span data-stu-id="08c93-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="08c93-108">Cela signifie que le code tout d’abord supposera que vos classes suivent les conventions utilisées par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="08c93-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="08c93-109">Dans ce cas, EF pourrez travailler sur les détails qu’il doit faire son travail.</span><span class="sxs-lookup"><span data-stu-id="08c93-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="08c93-110">Toutefois, si vos classes ne suivent pas ces conventions, vous avez la possibilité d’ajouter des configurations à vos classes afin de fournir d’EF avec les informations que nécessaires.</span><span class="sxs-lookup"><span data-stu-id="08c93-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="08c93-111">Tout d’abord les code vous offre deux façons d’ajouter ces configurations à vos classes.</span><span class="sxs-lookup"><span data-stu-id="08c93-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="08c93-112">Une utilise des attributs simples appelés DataAnnotations et l’autre est tout d’abord à l’aide de code est l’API Fluent, qui vous offre un moyen de décrire des configurations de manière impérative dans le code.</span><span class="sxs-lookup"><span data-stu-id="08c93-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="08c93-113">Cet article se concentrera sur l’utilisation de DataAnnotations (dans l’espace de noms System.ComponentModel.DataAnnotations) pour configurer vos classes – les configurations plus fréquemment requises de mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="08c93-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="08c93-114">DataAnnotations sont également comprises par un nombre d’applications .NET, telles qu’ASP.NET MVC qui autorise ces applications d’exploiter les mêmes annotations de validations côté client.</span><span class="sxs-lookup"><span data-stu-id="08c93-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="08c93-115">Le modèle</span><span class="sxs-lookup"><span data-stu-id="08c93-115">The model</span></span>

<span data-ttu-id="08c93-116">Je vais vous montrer DataAnnotations premier avec une simple paire de classes de code : Blog et Post.</span><span class="sxs-lookup"><span data-stu-id="08c93-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="08c93-117">Lorsqu’ils sont, les classes de Blog et Post facilement suivent la convention de premier code et requis sans ajustements pour aider à EF de travailler avec eux.</span><span class="sxs-lookup"><span data-stu-id="08c93-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="08c93-118">Mais vous pouvez également utiliser les annotations pour fournir plus d’informations à EF sur les classes et de la base de données qu’ils associent aux.</span><span class="sxs-lookup"><span data-stu-id="08c93-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="08c93-119">Touche</span><span class="sxs-lookup"><span data-stu-id="08c93-119">Key</span></span>

<span data-ttu-id="08c93-120">Entity Framework s’appuie sur chaque entité ayant une valeur de clé qu’il utilise pour les entités de suivi.</span><span class="sxs-lookup"><span data-stu-id="08c93-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="08c93-121">Les conventions de code en premier dépend est comment il implique la propriété qui est la clé dans chacune des classes de premier code.</span><span class="sxs-lookup"><span data-stu-id="08c93-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="08c93-122">Cette convention consiste à rechercher pour une propriété nommée « Id » ou qui combine le nom de classe et un « Id », tels que « BlogId ».</span><span class="sxs-lookup"><span data-stu-id="08c93-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="08c93-123">La propriété doit être mappée à une colonne de clé primaire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="08c93-124">Les classes Blog et Post suivent cette convention.</span><span class="sxs-lookup"><span data-stu-id="08c93-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="08c93-125">Mais que se passe-t-il si ce n’était pas ?</span><span class="sxs-lookup"><span data-stu-id="08c93-125">But what if they didn’t?</span></span> <span data-ttu-id="08c93-126">Que se passe-t-il si Blog utilisé le nom *PrimaryTrackingKey* à la place ou même *foo*?</span><span class="sxs-lookup"><span data-stu-id="08c93-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="08c93-127">Si le code tout d’abord ne trouve pas d’une propriété qui correspond à cette convention lève une exception en raison de l’exigence d’Entity Framework que vous devez disposer d’une propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="08c93-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="08c93-128">Vous pouvez utiliser l’annotation de clé pour spécifier quelle propriété doit être utilisé comme valeur EntityKey.</span><span class="sxs-lookup"><span data-stu-id="08c93-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="08c93-129">Si vous êtes tout d’abord à l’aide de code est fonction de génération de base de données, la table de Blog aura une colonne clé primaire nommée PrimaryTrackingKey qui est également défini en tant qu’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="08c93-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="08c93-131">Clés composites</span><span class="sxs-lookup"><span data-stu-id="08c93-131">Composite keys</span></span>

<span data-ttu-id="08c93-132">Entity Framework prend en charge les clés composites - clés primaires qui se composent de plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="08c93-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="08c93-133">Par exemple, votre peut avoir une classe de Passport dont la clé primaire est une combinaison de PassportNumber et IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="08c93-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="08c93-134">Si vous deviez essayer et utiliser la classe ci-dessus dans votre modèle EF vous obtiendriez un indiquant d’exceptions InvalidOperationException ;</span><span class="sxs-lookup"><span data-stu-id="08c93-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="08c93-135">*Impossible de déterminer le composite primaire tri par clé pour le type « Passport ». Utilisez la ColumnAttribute ou la méthode HasKey pour spécifier l’ordre des clés primaires composites.*</span><span class="sxs-lookup"><span data-stu-id="08c93-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="08c93-136">Lorsque vous avez des clés composites, Entity Framework vous oblige à définir un ordre des propriétés de clé.</span><span class="sxs-lookup"><span data-stu-id="08c93-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="08c93-137">Pour cela, à l’aide de l’annotation de colonne pour spécifier un ordre.</span><span class="sxs-lookup"><span data-stu-id="08c93-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="08c93-138">La valeur d’ordre est relative (plutôt qu’un index est basé) pour toutes les valeurs puissent être utilisées.</span><span class="sxs-lookup"><span data-stu-id="08c93-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="08c93-139">Par exemple, 100 et 200 serait acceptable à la place de 1 et 2.</span><span class="sxs-lookup"><span data-stu-id="08c93-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="08c93-140">Si vous avez des entités avec des clés étrangères composites vous devez spécifier la même colonne de classement que vous avez utilisé pour les propriétés de clé primaire correspondantes.</span><span class="sxs-lookup"><span data-stu-id="08c93-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="08c93-141">Uniquement l’ordre relatif dans les propriétés de clé étrangères doit être le même, les valeurs exactes affectés à **ordre** n’avez pas besoin de correspondre.</span><span class="sxs-lookup"><span data-stu-id="08c93-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="08c93-142">Par exemple, dans la classe suivante, 3 et 4 peut servir à la place de 1 et 2.</span><span class="sxs-lookup"><span data-stu-id="08c93-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="08c93-143">Obligatoire</span><span class="sxs-lookup"><span data-stu-id="08c93-143">Required</span></span>

<span data-ttu-id="08c93-144">L’annotation requise indique à EF qu’une propriété particulière est requise.</span><span class="sxs-lookup"><span data-stu-id="08c93-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="08c93-145">Ajout nécessaire pour la propriété Title forcera EF (et MVC) pour vous assurer que la propriété comporte des données.</span><span class="sxs-lookup"><span data-stu-id="08c93-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="08c93-146">Sans aucun supplémentaire aucune modification de code ou de balisage dans l’application, une application MVC effectue la validation côté client, création même dynamique d’un message en utilisant les noms de propriété et d’annotation.</span><span class="sxs-lookup"><span data-stu-id="08c93-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="08c93-148">L’attribut Required affecte également la base de données généré en effectuant la propriété mappée non nullable.</span><span class="sxs-lookup"><span data-stu-id="08c93-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="08c93-149">Vérifiez que le champ titre est passé à « not null ».</span><span class="sxs-lookup"><span data-stu-id="08c93-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="08c93-150">Dans certains cas il ne peut pas être possible pour la colonne dans la base de données soit non nullable même si la propriété est requise.</span><span class="sxs-lookup"><span data-stu-id="08c93-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="08c93-151">Par exemple, quand à l’aide de données de stratégie de l’héritage TPH pour plusieurs types est stockée dans une table unique.</span><span class="sxs-lookup"><span data-stu-id="08c93-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="08c93-152">Si un type dérivé inclut une propriété obligatoire, que la colonne ne peut pas être rendue non nullable comme pas tous les types dans la hiérarchie ont cette propriété.</span><span class="sxs-lookup"><span data-stu-id="08c93-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="08c93-154">MaxLength et MinLength</span><span class="sxs-lookup"><span data-stu-id="08c93-154">MaxLength and MinLength</span></span>

<span data-ttu-id="08c93-155">Les attributs MaxLength et MinLength autoriser vous permettent de spécifier des validations de propriété supplémentaires, comme vous le faisiez avec requis.</span><span class="sxs-lookup"><span data-stu-id="08c93-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="08c93-156">Voici le BloggerName avec les exigences de longueur.</span><span class="sxs-lookup"><span data-stu-id="08c93-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="08c93-157">L’exemple montre également comment combiner des attributs.</span><span class="sxs-lookup"><span data-stu-id="08c93-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="08c93-158">L’annotation MaxLength aura un impact sur la base de données en définissant la longueur de la propriété à 10.</span><span class="sxs-lookup"><span data-stu-id="08c93-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="08c93-160">Annotation de côté client MVC et annotation de côté serveur EF 4.1 les deux respecteront cette validation, création à nouveau dynamique d’un message d’erreur : « le champ BloggerName doit être un type de chaîne ou tableau avec une longueur maximale de « 10 ». » Ce message est un peu long.</span><span class="sxs-lookup"><span data-stu-id="08c93-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="08c93-161">Nombre illimité d’annotations vous permettre de spécifier un message d’erreur avec l’attribut de message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="08c93-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="08c93-162">Vous pouvez également spécifier le message d’erreur dans l’annotation requise.</span><span class="sxs-lookup"><span data-stu-id="08c93-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="08c93-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="08c93-164">NotMapped</span></span>

<span data-ttu-id="08c93-165">Convention de premier code impose que chaque propriété d’un type de données pris en charge est représentée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="08c93-166">Mais ce n’est pas toujours le cas dans vos applications.</span><span class="sxs-lookup"><span data-stu-id="08c93-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="08c93-167">Par exemple, vous pouvez avoir une propriété dans la classe de Blog qui crée un code basé sur les champs titre et BloggerName.</span><span class="sxs-lookup"><span data-stu-id="08c93-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="08c93-168">Cette propriété peut être créée dynamiquement et ne doit pas être stocké.</span><span class="sxs-lookup"><span data-stu-id="08c93-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="08c93-169">Vous pouvez marquer toutes les propriétés qui ne correspondent pas à la base de données avec l’annotation NotMapped telle que cette propriété BlogCode.</span><span class="sxs-lookup"><span data-stu-id="08c93-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="08c93-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="08c93-170">ComplexType</span></span>

<span data-ttu-id="08c93-171">Il n’est pas rare de décrire vos entités de domaine sur un ensemble de classes et de couche ensuite ces classes pour décrire une entité complète.</span><span class="sxs-lookup"><span data-stu-id="08c93-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="08c93-172">Par exemple, vous pouvez ajouter une classe appelée BlogDetails à votre modèle.</span><span class="sxs-lookup"><span data-stu-id="08c93-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="08c93-173">Notez que BlogDetails n’a pas de n’importe quel type de propriété de clé.</span><span class="sxs-lookup"><span data-stu-id="08c93-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="08c93-174">Dans la conception conduite par domaine, BlogDetails est appelé pour un objet de valeur.</span><span class="sxs-lookup"><span data-stu-id="08c93-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="08c93-175">Entity Framework fait référence aux objets de valeur comme des types complexes.</span><span class="sxs-lookup"><span data-stu-id="08c93-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="08c93-176">Les types complexes ne peuvent pas être suivies sur leurs propres.</span><span class="sxs-lookup"><span data-stu-id="08c93-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="08c93-177">Toutefois en tant que propriété dans la classe de Blog, BlogDetails il sera suivie dans le cadre d’un objet de Blog.</span><span class="sxs-lookup"><span data-stu-id="08c93-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="08c93-178">Dans l’ordre pour code first pour reconnaître de cela, vous devez marquer la classe BlogDetails comme un ComplexType.</span><span class="sxs-lookup"><span data-stu-id="08c93-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="08c93-179">Vous pouvez maintenant ajouter une propriété dans la classe de Blog pour représenter le BlogDetails pour ce blog.</span><span class="sxs-lookup"><span data-stu-id="08c93-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="08c93-180">Dans la base de données, la table de Blog contiendra toutes les propriétés du blog, y compris les propriétés contenues dans sa propriété BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="08c93-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="08c93-181">Par défaut, chacun d’eux est précédé par le nom du type complexe, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="08c93-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="08c93-183">Autre Remarque intéressant est que bien que la propriété DateCreated a été définie comme une valeur non nullable DateTime dans la classe, le champ de base de données correspondante est nullable.</span><span class="sxs-lookup"><span data-stu-id="08c93-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="08c93-184">Vous devez utiliser l’annotation requise si vous souhaitez affecter le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="08c93-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="08c93-185">ConcurrencyCheck</span></span>

<span data-ttu-id="08c93-186">L’annotation ConcurrencyCheck vous permet de marquer une ou plusieurs propriétés à utiliser pour l’accès concurrentiel dans la base de données lorsqu’un utilisateur modifie ou supprime une entité.</span><span class="sxs-lookup"><span data-stu-id="08c93-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="08c93-187">Si vous avez travaillé avec le Concepteur EF, celui-ci s’aligne définition ConcurrencyMode d’une propriété sur Fixed.</span><span class="sxs-lookup"><span data-stu-id="08c93-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="08c93-188">Nous allons voir comment ConcurrencyCheck fonctionne en l’ajoutant à la propriété BloggerName.</span><span class="sxs-lookup"><span data-stu-id="08c93-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="08c93-189">Lorsque SaveChanges est appelée, en raison de l’annotation ConcurrencyCheck sur le champ BloggerName, la valeur d’origine de cette propriété sera utilisée dans la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="08c93-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="08c93-190">La commande tente de localiser la ligne correcte par filtrage non seulement sur la valeur de clé, mais également sur la valeur d’origine de BloggerName.</span><span class="sxs-lookup"><span data-stu-id="08c93-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="08c93-191">Voici les parties critiques de la commande de mise à jour envoyées à la base de données, où vous pouvez voir la commande met à jour la ligne qui a un PrimaryTrackingKey est 1 et un BloggerName de « Julie » qui était la valeur d’origine quand ce blog a été récupéré à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="08c93-192">Si quelqu'un a modifié le nom de blogueur pour ce blog en attendant, cette mise à jour échoue et vous obtiendrez une DbUpdateConcurrencyException dont vous aurez besoin pour gérer.</span><span class="sxs-lookup"><span data-stu-id="08c93-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="08c93-193">Horodatage</span><span class="sxs-lookup"><span data-stu-id="08c93-193">TimeStamp</span></span>

<span data-ttu-id="08c93-194">Il est plus courant d’utiliser des champs rowversion ou timestamp pour le contrôle d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="08c93-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="08c93-195">Mais au lieu d’utiliser l’annotation ConcurrencyCheck, vous pouvez utiliser l’annotation d’horodateur plus spécifique, que le type de la propriété est le tableau d’octets.</span><span class="sxs-lookup"><span data-stu-id="08c93-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="08c93-196">Code tout d’abord traite les propriétés horodatage exactement en tant que propriétés de ConcurrencyCheck, mais il garantit également que le champ de base de données qui génère d’abord code est non nullable.</span><span class="sxs-lookup"><span data-stu-id="08c93-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="08c93-197">Vous ne pouvez avoir qu’une seule propriété d’horodatage dans une classe donnée.</span><span class="sxs-lookup"><span data-stu-id="08c93-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="08c93-198">Ajout de la propriété suivante à la classe de Blog :</span><span class="sxs-lookup"><span data-stu-id="08c93-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="08c93-199">résultats dans le code créant d’abord une colonne timestamp non null dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="08c93-201">Table et colonne</span><span class="sxs-lookup"><span data-stu-id="08c93-201">Table and Column</span></span>

<span data-ttu-id="08c93-202">Si vous permettez à Code First créer la base de données, vous souhaiterez modifier le nom des tables et des colonnes en cours de création.</span><span class="sxs-lookup"><span data-stu-id="08c93-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="08c93-203">Vous pouvez également utiliser Code First avec une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="08c93-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="08c93-204">Mais il n’est pas toujours le cas que les noms des classes et des propriétés dans votre domaine correspondent aux noms des tables et des colonnes dans votre base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="08c93-205">Ma classe est nommée Blog et par convention, code tout d’abord part du principe que cela permet de mapper vers une table nommée Blogs.</span><span class="sxs-lookup"><span data-stu-id="08c93-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="08c93-206">Si tel n’est pas le cas, vous pouvez spécifier le nom de la table avec l’attribut de la Table.</span><span class="sxs-lookup"><span data-stu-id="08c93-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="08c93-207">Ici, par exemple, l’annotation Spécifie que le nom de table est InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="08c93-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="08c93-208">L’annotation de la colonne est un adept plus en spécifiant les attributs d’une colonne mappée.</span><span class="sxs-lookup"><span data-stu-id="08c93-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="08c93-209">Vous pouvez stipuler un nom, type de données ou même l’ordre dans lequel une colonne apparaît dans la table.</span><span class="sxs-lookup"><span data-stu-id="08c93-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="08c93-210">Voici un exemple de l’attribut de colonne.</span><span class="sxs-lookup"><span data-stu-id="08c93-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="08c93-211">Ne confondez pas attribut TypeName de la colonne avec le DataType DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="08c93-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="08c93-212">Type de données est une annotation utilisée pour l’interface utilisateur et est ignorée par Code First.</span><span class="sxs-lookup"><span data-stu-id="08c93-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="08c93-213">Voici la table une fois qu’il est régénéré.</span><span class="sxs-lookup"><span data-stu-id="08c93-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="08c93-214">Le nom de la table a changé à InternalBlogs et colonne de Description à partir du type complex est désormais BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="08c93-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="08c93-215">Étant donné que le nom a été spécifié dans l’annotation, code tout d’abord n’utilise pas la convention de commencer le nom de colonne avec le nom du type complexe.</span><span class="sxs-lookup"><span data-stu-id="08c93-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="08c93-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="08c93-217">DatabaseGenerated</span></span>

<span data-ttu-id="08c93-218">Une fonctionnalités de base de données important est la possibilité d’avoir les propriétés calculées.</span><span class="sxs-lookup"><span data-stu-id="08c93-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="08c93-219">Si vous mappez vos classes de Code First pour les tables qui contiennent des colonnes calculées, vous ne voulez Entity Framework pour tenter de mettre à jour ces colonnes.</span><span class="sxs-lookup"><span data-stu-id="08c93-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="08c93-220">Mais vous ne souhaitez pas que EF pour renvoyer ces valeurs à partir de la base de données une fois que vous avez inséré ou mis à jour des données.</span><span class="sxs-lookup"><span data-stu-id="08c93-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="08c93-221">Vous pouvez utiliser l’annotation DatabaseGenerated pour signaler les propriétés dans votre classe, ainsi que l’énumération calculé.</span><span class="sxs-lookup"><span data-stu-id="08c93-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="08c93-222">Autres énumérations sont None et d’identité.</span><span class="sxs-lookup"><span data-stu-id="08c93-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="08c93-223">Vous pouvez utiliser la base de données générée sur les colonnes byte ou timestamp lorsque code génère tout d’abord la base de données, sinon vous devez uniquement l’utiliser en pointant sur les bases de données existantes, car le code ne sont pas tout d’abord être en mesure de déterminer la formule pour la colonne calculée.</span><span class="sxs-lookup"><span data-stu-id="08c93-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="08c93-224">Vous lisez supérieur à celui par défaut, une propriété de clé est un entier deviendra une clé d’identité dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="08c93-225">Qui serait le même que l’affectation DatabaseGenerated DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="08c93-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="08c93-226">Si vous ne souhaitez pas qu’il soit une clé d’identité, vous pouvez définir la valeur à DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="08c93-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="08c93-227">Index</span><span class="sxs-lookup"><span data-stu-id="08c93-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="08c93-228">**EF6.1 et versions ultérieures uniquement** -attribut de l’Index a été introduite dans Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="08c93-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="08c93-229">Si vous utilisez une version antérieure les informations contenues dans cette section ne s’applique pas.</span><span class="sxs-lookup"><span data-stu-id="08c93-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="08c93-230">Vous pouvez créer un index sur une ou plusieurs colonnes à l’aide de la **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="08c93-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="08c93-231">Ajout de l’attribut à une ou plusieurs propriétés sera provoquer d’EF créer l’index correspondant dans la base de données lorsqu’il crée la base de données ou structurer le correspondantes **CreateIndex** appelle si vous utilisez Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="08c93-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="08c93-232">Par exemple, le code suivant entraîne un index créé sur le **évaluation** colonne de la **billets** table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="08c93-233">Par défaut, l’index sera nommé **IX\_&lt;nom de la propriété&gt;**  (IX\_Rating dans l’exemple ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="08c93-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="08c93-234">Vous pouvez également spécifier un nom pour l’index cependant.</span><span class="sxs-lookup"><span data-stu-id="08c93-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="08c93-235">L’exemple suivant spécifie que l’index doit être nommé **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="08c93-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="08c93-236">Par défaut, les index sont non uniques, mais vous pouvez utiliser la **IsUnique** nommé de paramètre pour spécifier qu’un index doit être unique.</span><span class="sxs-lookup"><span data-stu-id="08c93-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="08c93-237">L’exemple suivant présente un index unique sur une **utilisateur**du nom de connexion.</span><span class="sxs-lookup"><span data-stu-id="08c93-237">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="08c93-238">Index de plusieurs colonnes</span><span class="sxs-lookup"><span data-stu-id="08c93-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="08c93-239">Les index qui s’étendent sur plusieurs colonnes sont spécifiés à l’aide du même nom dans plusieurs annotations d’Index pour une table donnée.</span><span class="sxs-lookup"><span data-stu-id="08c93-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="08c93-240">Lorsque vous créez des index multi-colonnes, vous devez spécifier un ordre pour les colonnes dans l’index.</span><span class="sxs-lookup"><span data-stu-id="08c93-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="08c93-241">Par exemple, le code suivant crée un index multicolonnes sur **évaluation** et **BlogId** appelé **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="08c93-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="08c93-242">**BlogId** est la première colonne dans l’index et **évaluation** est le deuxième.</span><span class="sxs-lookup"><span data-stu-id="08c93-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="08c93-243">Relations d’attributs : InverseProperty et ForeignKey</span><span class="sxs-lookup"><span data-stu-id="08c93-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="08c93-244">Cette page fournit des informations sur la configuration des relations dans votre modèle Code First à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="08c93-245">Pour obtenir des informations générales sur les relations dans Entity Framework et comment accéder à et manipuler des données à l’aide de relations, consultez [relations & Propriétés de Navigation](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="08c93-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="08c93-246">Convention de premier code s’occupe des relations plus courantes dans votre modèle, mais il existe quelques cas où il a besoin d’aide.</span><span class="sxs-lookup"><span data-stu-id="08c93-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="08c93-247">Modification du nom de la propriété de clé dans la classe Blog créée un problème avec sa relation à Post.</span><span class="sxs-lookup"><span data-stu-id="08c93-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="08c93-248">Lors de la génération de la base de données, code tout d’abord voit la propriété BlogId dans la classe de publication et la reconnaît, par la convention qu’elle correspond à un nom de classe et un « Id », comme une clé étrangère à la classe de Blog.</span><span class="sxs-lookup"><span data-stu-id="08c93-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="08c93-249">Mais il n’existe aucune propriété BlogId dans la classe de blog.</span><span class="sxs-lookup"><span data-stu-id="08c93-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="08c93-250">La solution consiste à créer une propriété de navigation dans le billet d’utiliser le DataAnnotation étrangère pour aider à code tout d’abord comprendre comment créer la relation entre les deux classes, à l’aide de la propriété Post.BlogId, ainsi que la façon de spécifier des contraintes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="08c93-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="08c93-251">La contrainte dans la base de données montre une relation entre InternalBlogs.PrimaryTrackingKey et Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="08c93-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="08c93-253">Le InverseProperty est utilisé lorsque vous avez plusieurs relations entre les classes.</span><span class="sxs-lookup"><span data-stu-id="08c93-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="08c93-254">Dans la classe Post, voulez-vous effectuer le suivi de qui l’a écrit un billet de blog, ainsi que qui a été modifié.</span><span class="sxs-lookup"><span data-stu-id="08c93-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="08c93-255">Voici deux nouvelles propriétés de navigation pour la classe de publication.</span><span class="sxs-lookup"><span data-stu-id="08c93-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="08c93-256">Vous devrez également ajouter dans la classe Person référencée par ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="08c93-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="08c93-257">La classe Person a les propriétés de navigation à la publication, un pour tous les messages du écrites par la personne et un pour tous les billets de mise à jour par cette personne.</span><span class="sxs-lookup"><span data-stu-id="08c93-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="08c93-258">Tout d’abord code n’est pas capable de faire correspondre les propriétés dans les deux classes sur son propre.</span><span class="sxs-lookup"><span data-stu-id="08c93-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="08c93-259">La table de base de données pour des publications doit avoir une clé étrangère de la personne CreatedBy et une pour la personne de UpdatedBy, le code, mais tout d’abord créera quatre propriétés de clé étrangère est : personne\_Id, personne\_Id1, CreatedBy\_Id et UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="08c93-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="08c93-261">Pour résoudre ces problèmes, vous pouvez utiliser l’annotation InverseProperty pour spécifier l’alignement des propriétés.</span><span class="sxs-lookup"><span data-stu-id="08c93-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="08c93-262">Étant donné que la propriété PostsWritten en personne sait que cela fait référence au type de publication, il générera la relation à Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="08c93-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="08c93-263">De même, PostsUpdated sera connecté à Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="08c93-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="08c93-264">Et tout d’abord code ne crée pas les clés étrangères supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="08c93-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="08c93-266">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="08c93-266">Summary</span></span>

<span data-ttu-id="08c93-267">DataAnnotations vous permettent non seulement de décrire la validation côté client et serveur dans vos classes de premier code, mais ils vous permettent également d’améliorer et même corriger les hypothèses code va d’abord faire sur vos classes selon ses conventions.</span><span class="sxs-lookup"><span data-stu-id="08c93-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="08c93-268">Avec DataAnnotations, vous pouvez optimiser pas uniquement les génération de schéma de base de données, mais vous pouvez également mapper vos classes de premier code pour une base de données existant.</span><span class="sxs-lookup"><span data-stu-id="08c93-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="08c93-269">Pendant qu’elles sont très flexibles, n’oubliez pas que DataAnnotations fournir uniquement le meilleur parti généralement les modifications de configuration que vous pouvez effectuer sur vos classes de premier code nécessaires.</span><span class="sxs-lookup"><span data-stu-id="08c93-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="08c93-270">Pour configurer vos classes pour certains cas edge, vous devriez rechercher au mécanisme de configuration de remplacement API du Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="08c93-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
