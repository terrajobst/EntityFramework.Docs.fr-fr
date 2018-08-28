---
title: Conventions personnalisées Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995826"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="f0c72-102">Conventions personnalisées Code First</span><span class="sxs-lookup"><span data-stu-id="f0c72-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="f0c72-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f0c72-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f0c72-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="f0c72-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="f0c72-105">Lorsque vous utilisez Code First, votre modèle est calculé à partir de vos classes à l’aide d’un ensemble de conventions.</span><span class="sxs-lookup"><span data-stu-id="f0c72-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="f0c72-106">La valeur par défaut [Conventions Code First](~/ef6/modeling/code-first/conventions/built-in.md) déterminer des choses comme qui propriété devient la clé primaire d’une entité, le nom de la table d’une entité est mappé à, et que la précision et l’échelle une colonne decimal a par défaut.</span><span class="sxs-lookup"><span data-stu-id="f0c72-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="f0c72-107">Parfois, ces conventions par défaut ne sont pas adaptées à votre modèle, et vous devez y remédier en configurant des nombreuses entités individuelles à l’aide des Annotations de données ou l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f0c72-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="f0c72-108">Conventions Code First personnalisées vous permettent de définir vos propres conventions qui fournissent des valeurs par défaut de configuration pour votre modèle.</span><span class="sxs-lookup"><span data-stu-id="f0c72-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="f0c72-109">Dans cette procédure pas à pas, nous allons examiner les différents types de conventions personnalisées et comment créer chacun d’eux.</span><span class="sxs-lookup"><span data-stu-id="f0c72-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="f0c72-110">Conventions reposant sur un modèle</span><span class="sxs-lookup"><span data-stu-id="f0c72-110">Model-Based Conventions</span></span>

<span data-ttu-id="f0c72-111">Cette page traite de l’API DbModelBuilder pour conventions personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f0c72-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="f0c72-112">Cette API doit être suffisante pour la création de la plupart des conventions personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f0c72-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="f0c72-113">Toutefois, il est également la capacité à créer des conventions basée sur modèle - conventions qui manipulent le modèle final qui a été créé - pour gérer des scénarios avancés.</span><span class="sxs-lookup"><span data-stu-id="f0c72-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="f0c72-114">Pour plus d’informations, consultez [Conventions reposant sur le modèle](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="f0c72-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="f0c72-115">Notre modèle</span><span class="sxs-lookup"><span data-stu-id="f0c72-115">Our Model</span></span>

<span data-ttu-id="f0c72-116">Commençons par définir un modèle simple que nous pouvons utiliser avec nos conventions.</span><span class="sxs-lookup"><span data-stu-id="f0c72-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="f0c72-117">Ajoutez les classes suivantes à votre projet.</span><span class="sxs-lookup"><span data-stu-id="f0c72-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="f0c72-118">Présentation de Conventions personnalisées</span><span class="sxs-lookup"><span data-stu-id="f0c72-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="f0c72-119">Nous allons écrire une convention qui configure de n’importe quelle propriété de clé qui sera la clé primaire pour son type d’entité nommée.</span><span class="sxs-lookup"><span data-stu-id="f0c72-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="f0c72-120">Conventions sont activées sur le Générateur de modèles, qui est accessible en substituant OnModelCreating dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="f0c72-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="f0c72-121">Mettre à jour la classe ProductContext comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="f0c72-122">Maintenant, n’importe quelle propriété dans notre modèle nommé clé sera configuré en tant que la clé primaire de toute entité sa partie de.</span><span class="sxs-lookup"><span data-stu-id="f0c72-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="f0c72-123">Nous pourrions également souhaiter que nos conventions plus spécifique en filtrant sur le type de propriété que nous allons configurer :</span><span class="sxs-lookup"><span data-stu-id="f0c72-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="f0c72-124">Cette opération configure toutes les propriétés appelées clé primaire de clé de l’entité, mais uniquement si elles sont un entier.</span><span class="sxs-lookup"><span data-stu-id="f0c72-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="f0c72-125">Une fonctionnalité intéressante de la méthode IsKey est qu’il est additif.</span><span class="sxs-lookup"><span data-stu-id="f0c72-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="f0c72-126">Ce qui signifie que si vous appelez IsKey sur plusieurs propriétés et ils deviennent tous partie d’une clé composite.</span><span class="sxs-lookup"><span data-stu-id="f0c72-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="f0c72-127">Pour cela l’inconvénient est que lorsque vous spécifiez plusieurs propriétés pour une clé vous devez également spécifier un ordre de ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="f0c72-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="f0c72-128">Pour cela, en appelant le HasColumnOrder méthode comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f0c72-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="f0c72-129">Ce code configurera les types dans notre modèle pour avoir une clé composite constituée de la colonne de clé de type int et la colonne de nom de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f0c72-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="f0c72-130">Si nous permet d’afficher le modèle dans le concepteur, elle ressemblerait à ceci :</span><span class="sxs-lookup"><span data-stu-id="f0c72-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="f0c72-132">Un autre exemple de conventions de propriété consiste à configurer toutes les propriétés de date/heure dans mon modèle pour mapper au type datetime2 dans SQL Server au lieu de date/heure.</span><span class="sxs-lookup"><span data-stu-id="f0c72-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="f0c72-133">Vous pouvez y parvenir avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f0c72-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="f0c72-134">Classes de convention</span><span class="sxs-lookup"><span data-stu-id="f0c72-134">Convention Classes</span></span>

<span data-ttu-id="f0c72-135">Une autre façon de définir les conventions consiste à utiliser une Convention de classe pour encapsuler votre convention.</span><span class="sxs-lookup"><span data-stu-id="f0c72-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="f0c72-136">Lorsque vous utilisez une classe de Convention puis vous créez un type qui hérite de la classe Convention dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="f0c72-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="f0c72-137">Nous pouvons créer une classe de Convention avec la convention datetime2 que nous vous avons montré précédemment en procédant comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="f0c72-138">Pour demander à EF d’utiliser cette convention ajoutez-le à la collection de Conventions dans OnModelCreating, qui si vous avez suivi la procédure pas à pas se présentera comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="f0c72-139">Comme vous pouvez le voir nous ajoutons une instance de notre convention à la collection de conventions.</span><span class="sxs-lookup"><span data-stu-id="f0c72-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="f0c72-140">Héritage à partir de la Convention d’offre un moyen pratique de regroupement et de partager des conventions entre équipes ou quelques projets.</span><span class="sxs-lookup"><span data-stu-id="f0c72-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="f0c72-141">Vous pouvez, par exemple, avoir une bibliothèque de classes avec un ensemble commun de conventions que toutes vos organisations projets utilisent.</span><span class="sxs-lookup"><span data-stu-id="f0c72-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="f0c72-142">Attributs personnalisés</span><span class="sxs-lookup"><span data-stu-id="f0c72-142">Custom Attributes</span></span>

<span data-ttu-id="f0c72-143">Une autre utilisation des conventions consiste à activer les nouveaux attributs à utiliser lors de la configuration d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="f0c72-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="f0c72-144">Pour illustrer cela, nous allons créer un attribut que nous pouvons utiliser pour marquer les propriétés de chaîne en tant que non-Unicode.</span><span class="sxs-lookup"><span data-stu-id="f0c72-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="f0c72-145">Maintenant, nous allons créer une convention pour appliquer cet attribut à notre modèle :</span><span class="sxs-lookup"><span data-stu-id="f0c72-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="f0c72-146">Avec cette convention, nous pouvons ajouter l’attribut de type non-Unicode à un de nos propriétés de chaîne, ce qui signifie que la colonne dans la base de données est stockée en tant que varchar au lieu de nvarchar.</span><span class="sxs-lookup"><span data-stu-id="f0c72-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="f0c72-147">Une chose à noter concernant cette convention est que si vous placez l’attribut de type non-Unicode sur autre chose qu’une propriété de chaîne, puis elle lève une exception.</span><span class="sxs-lookup"><span data-stu-id="f0c72-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="f0c72-148">Pour cela, car vous ne pouvez pas configurer IsUnicode sur n’importe quel type autre qu’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f0c72-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="f0c72-149">Si cela se produit, alors vous pouvez rendre votre convention plus spécifiques, afin qu’il exclut tout ce qui n’est pas une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f0c72-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="f0c72-150">Bien que la convention ci-dessus fonctionne pour la définition des attributs personnalisés, il existe une autre API peut être beaucoup plus facile à utiliser, en particulier lorsque vous souhaitez utiliser les propriétés de la classe d’attributs.</span><span class="sxs-lookup"><span data-stu-id="f0c72-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="f0c72-151">Pour cet exemple, nous allons mettre à jour notre attribut et remplacez-le par un attribut IsUnicode, afin qu’il ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="f0c72-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="f0c72-152">Une fois que nous avons ceci, nous pouvons définir une valeur booléenne sur notre attribut pour indiquer à la convention ou non une propriété doit être Unicode.</span><span class="sxs-lookup"><span data-stu-id="f0c72-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="f0c72-153">Nous pourrions effectuer cette opération dans la convention que nous avons déjà en accédant à la ClrProperty de la classe de configuration comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="f0c72-154">Cela est assez simple, mais il existe un moyen plus succinct de parvenir à l’aide de la méthode de l’API de conventions.</span><span class="sxs-lookup"><span data-stu-id="f0c72-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="f0c72-155">L’avoir de méthode a un paramètre de type Func&lt;PropertyInfo, T&gt; qui accepte le PropertyInfo identique à l’emplacement où la méthode, mais est censée renvoyer un objet.</span><span class="sxs-lookup"><span data-stu-id="f0c72-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="f0c72-156">Si l’objet retourné est null, alors la propriété ne sera pas configurée, ce qui signifie que vous pouvez filtrer les propriétés avec lui comme Where, mais il est différent, il sera également capturer l’objet retourné et passez-le à la méthode Configure.</span><span class="sxs-lookup"><span data-stu-id="f0c72-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="f0c72-157">Cela fonctionne comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="f0c72-158">Attributs personnalisés ne sont pas la seule raison d’utiliser la méthode, il est utile en tout lieu dont vous avez besoin de raisonner sur quelque chose que vous filtrez sur lors de la configuration de vos types ou les propriétés.</span><span class="sxs-lookup"><span data-stu-id="f0c72-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="f0c72-159">Configuration des Types</span><span class="sxs-lookup"><span data-stu-id="f0c72-159">Configuring Types</span></span>

<span data-ttu-id="f0c72-160">Jusqu'à présent, toutes nos conventions ont été pour les propriétés, mais il existe une autre zone de l’API de conventions pour la configuration des types dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="f0c72-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="f0c72-161">L’expérience est semblable aux conventions que nous avons vus jusqu'à présent, mais les options de configuration à l’intérieur sera à l’entité au lieu de la propriété niveau.</span><span class="sxs-lookup"><span data-stu-id="f0c72-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="f0c72-162">Une des choses que les conventions de niveau de Type peuvent être très utiles pour change la convention de nommage table pour mapper à un schéma existant qui diffère de la valeur par défaut d’EF ou pour créer une nouvelle base de données avec une autre convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="f0c72-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="f0c72-163">Pour ce faire, nous devons tout d’abord une méthode qui peut accepter la TypeInfo pour un type dans notre modèle et retourner ce que le nom de table pour ce type doit être :</span><span class="sxs-lookup"><span data-stu-id="f0c72-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="f0c72-164">Cette méthode prend un type et retourne une chaîne qui utilise des minuscules avec des traits de soulignement au lieu de la casse mixte.</span><span class="sxs-lookup"><span data-stu-id="f0c72-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="f0c72-165">Dans notre modèle, cela signifie que la classe de ProductCategory sera être mappée à une table appelée produit\_catégorie au lieu de ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="f0c72-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="f0c72-166">Une fois que nous avons cette méthode, nous pouvons l’appeler dans une convention comme suit :</span><span class="sxs-lookup"><span data-stu-id="f0c72-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="f0c72-167">Cette convention configure chaque type dans notre modèle pour mapper au nom de table qui est retourné à partir de notre méthode GetTableName.</span><span class="sxs-lookup"><span data-stu-id="f0c72-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="f0c72-168">Cette convention est l’équivalent à l’appel de la méthode ToTable pour chaque entité dans le modèle à l’aide de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f0c72-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="f0c72-169">Une chose à noter à ce sujet est que lorsque vous appelez EF ToTable prendra la chaîne que vous fournissez le nom de table exact, sans les pluralisation qui serait normalement le cas lors de la détermination des noms de table.</span><span class="sxs-lookup"><span data-stu-id="f0c72-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="f0c72-170">C’est pourquoi le nom de table à partir de notre convention est produit\_catégorie au lieu de produit\_catégories.</span><span class="sxs-lookup"><span data-stu-id="f0c72-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="f0c72-171">Nous pouvons résoudre que dans notre convention en effectuant un appel au service de pluralisation nous-mêmes.</span><span class="sxs-lookup"><span data-stu-id="f0c72-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="f0c72-172">Dans le code suivant, nous utiliserons le [résolution des dépendances](~/ef6/fundamentals/configuring/dependency-resolution.md) fonctionnalité ajoutée dans EF6 pour récupérer le service de pluralisation EF aurait utilisé et pluraliser notre nom de la table.</span><span class="sxs-lookup"><span data-stu-id="f0c72-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="f0c72-173">La version générique de GetService est une méthode d’extension dans l’espace de noms System.Data.Entity.Infrastructure.DependencyResolution, vous devez ajouter une à l’aide de l’instruction à votre contexte pour pouvoir l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="f0c72-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="f0c72-174">ToTable et héritage</span><span class="sxs-lookup"><span data-stu-id="f0c72-174">ToTable and Inheritance</span></span>

<span data-ttu-id="f0c72-175">Un autre aspect important de ToTable est que si vous mappez explicitement un type à une table donnée, vous pouvez modifier la stratégie de mappage qui seront utilisés par EF.</span><span class="sxs-lookup"><span data-stu-id="f0c72-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="f0c72-176">Si vous appelez ToTable pour chaque type dans une hiérarchie d’héritage, en passant le nom de type en tant que le nom de la table, comme nous l’avons fait ci-dessus, puis vous allez modifier la stratégie de mappage de Table par hiérarchie (TPH) par défaut pour la Table par Type (TPT).</span><span class="sxs-lookup"><span data-stu-id="f0c72-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="f0c72-177">La meilleure façon de décrire Ceci est un exemple concret de whith :</span><span class="sxs-lookup"><span data-stu-id="f0c72-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="f0c72-178">Par défaut employé et manager sont mappés à la même table (employés) dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="f0c72-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="f0c72-179">La table contiendra les employés et aux responsables d’une colonne de discriminateur qui vous indiquera à quel type d’instance est stocké dans chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="f0c72-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="f0c72-180">Il s’agit de mappage TPH qu’il existe une seule table pour la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="f0c72-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="f0c72-181">Toutefois, si vous appelez ToTable sur les deux classe puis chaque type sera plutôt être mappée à sa propre table, également appelé TPT, car chaque type possède sa propre table.</span><span class="sxs-lookup"><span data-stu-id="f0c72-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="f0c72-182">Le code ci-dessus mappera vers une structure de table qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="f0c72-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="f0c72-184">Vous pouvez éviter ce problème et mettre à jour le mappage TPH par défaut, de différentes manières :</span><span class="sxs-lookup"><span data-stu-id="f0c72-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="f0c72-185">Appelez ToTable portant le même nom de table pour chaque type dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="f0c72-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="f0c72-186">Appelez ToTable uniquement sur la classe de base de la hiérarchie, dans notre exemple serait employé.</span><span class="sxs-lookup"><span data-stu-id="f0c72-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="f0c72-187">Ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="f0c72-187">Execution Order</span></span>

<span data-ttu-id="f0c72-188">Conventions de fonctionnent de manière dernière wins, le même que l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f0c72-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="f0c72-189">Cela signifie que si vous écrivez deux conventions qui configurent la même option de la même propriété, puis il pour exécuter wins.</span><span class="sxs-lookup"><span data-stu-id="f0c72-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="f0c72-190">Par exemple, dans le code ci-dessous, la longueur maximale de toutes les chaînes est définie sur 500, mais nous ensuite configurer toutes les propriétés appelées nom dans le modèle pour avoir une longueur maximale de 250.</span><span class="sxs-lookup"><span data-stu-id="f0c72-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="f0c72-191">Étant donné que la convention pour définir la longueur maximale de 250 est après celle qui définit toutes les chaînes à 500, toutes les propriétés appelées nom dans notre modèle aura un MaxLength de 250 lors de toutes les autres chaînes, telles que des descriptions, serait de 500.</span><span class="sxs-lookup"><span data-stu-id="f0c72-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="f0c72-192">À l’aide des conventions de cette façon signifie que vous pouvez fournir une convention générale pour les types ou les propriétés dans votre modèle, puis overide pour les sous-ensembles sont différents.</span><span class="sxs-lookup"><span data-stu-id="f0c72-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="f0c72-193">L’API Fluent et les Annotations de données peuvent également être utilisées pour remplacer une convention dans des cas spécifiques.</span><span class="sxs-lookup"><span data-stu-id="f0c72-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="f0c72-194">Dans l’exemple ci-dessus si nous avions utilisé l’API Fluent pour définir la longueur maximale d’une propriété puis nous pouvons également avoir ajouter il avant ou après la convention, étant donné que l’API Fluent plus spécifiques emporte sur la Convention de Configuration plus générales.</span><span class="sxs-lookup"><span data-stu-id="f0c72-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="f0c72-195">Conventions intégrées</span><span class="sxs-lookup"><span data-stu-id="f0c72-195">Built-in Conventions</span></span>

<span data-ttu-id="f0c72-196">Étant donné que les conventions personnalisées pourrait être affectées par les conventions Code First par défaut, il peut être utile d’ajouter des conventions à exécuter avant ou après une autre convention.</span><span class="sxs-lookup"><span data-stu-id="f0c72-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="f0c72-197">Pour ce faire, vous pouvez utiliser les méthodes AddBefore et AddAfter de la collection de Conventions sur votre DbContext dérivée.</span><span class="sxs-lookup"><span data-stu-id="f0c72-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="f0c72-198">Le code suivant ajouteriez la classe convention que nous avons créé précédemment afin qu’elle sera exécutée avant la génération de la convention de découverte des clés.</span><span class="sxs-lookup"><span data-stu-id="f0c72-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="f0c72-199">Cela va être les plus utiles lors de l’ajout des conventions qui doivent s’exécuter avant ou après les conventions intégrées, une liste des conventions intégrées est disponible ici : [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="f0c72-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="f0c72-200">Vous pouvez également supprimer les conventions que vous ne souhaitez pas appliquées à votre modèle.</span><span class="sxs-lookup"><span data-stu-id="f0c72-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="f0c72-201">Pour supprimer une convention, utilisez la méthode Remove.</span><span class="sxs-lookup"><span data-stu-id="f0c72-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="f0c72-202">Voici un exemple de la suppression de la PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="f0c72-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
