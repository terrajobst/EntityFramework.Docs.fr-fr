---
title: Conventions de Model-Based - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283705"
---
# <a name="model-based-conventions"></a><span data-ttu-id="7985c-102">Conventions reposant sur un modèle</span><span class="sxs-lookup"><span data-stu-id="7985c-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="7985c-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7985c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="7985c-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="7985c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="7985c-105">Conventions de modèle basé sont une méthode avancée de la configuration de modèle basé sur une convention.</span><span class="sxs-lookup"><span data-stu-id="7985c-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="7985c-106">La plupart des scénarios le [Custom API Code First Convention sur DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) doit être utilisé.</span><span class="sxs-lookup"><span data-stu-id="7985c-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="7985c-107">Une présentation de l’API DbModelBuilder pour les conventions est recommandée avant d’utiliser les conventions de modèle en fonction.</span><span class="sxs-lookup"><span data-stu-id="7985c-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="7985c-108">Conventions de modèle en fonction d’autoriser la création de conventions qui affectent des propriétés et les tables qui ne sont pas configurables via les conventions standard.</span><span class="sxs-lookup"><span data-stu-id="7985c-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="7985c-109">Colonnes de discriminateur dans la table par les modèles de la hiérarchie et les colonnes de l’Association indépendante sont des exemples.</span><span class="sxs-lookup"><span data-stu-id="7985c-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="7985c-110">Création d’une Convention</span><span class="sxs-lookup"><span data-stu-id="7985c-110">Creating a Convention</span></span>   

<span data-ttu-id="7985c-111">La première étape de création d’une convention de modèle basé choisit quand dans le pipeline de la convention doit être appliquée au modèle.</span><span class="sxs-lookup"><span data-stu-id="7985c-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="7985c-112">Il existe deux types de conventions de modèles, conceptuel (espace C) et Store (espace S).</span><span class="sxs-lookup"><span data-stu-id="7985c-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="7985c-113">Une convention espace C est appliquée au modèle de l’application est générée, tandis qu’une convention espace S est appliquée à la version du modèle qui représente la base de données et des contrôles, notamment comment généré automatiquement les colonnes est nommés.</span><span class="sxs-lookup"><span data-stu-id="7985c-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="7985c-114">Une convention de modèle est une classe qui s’étend de IConceptualModelConvention ou IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="7985c-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="7985c-115">Ces interfaces, de que les deux acceptent un type générique qui peut être type MetadataItem qui est utilisé pour filtrer le type de données qui s’applique à la convention.</span><span class="sxs-lookup"><span data-stu-id="7985c-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="7985c-116">Ajout d’une Convention</span><span class="sxs-lookup"><span data-stu-id="7985c-116">Adding a Convention</span></span>   

<span data-ttu-id="7985c-117">Conventions de modèle sont ajoutées dans la même façon que les classes de conventions régulière.</span><span class="sxs-lookup"><span data-stu-id="7985c-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="7985c-118">Dans le **OnModelCreating** (méthode), ajoutez la convention à la liste de conventions pour un modèle.</span><span class="sxs-lookup"><span data-stu-id="7985c-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="7985c-119">Une convention peut également être ajoutée par rapport à une autre convention à l’aide de la Conventions.AddBefore\< \> ou Conventions.AddAfter\< \> méthodes.</span><span class="sxs-lookup"><span data-stu-id="7985c-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="7985c-120">Pour plus d’informations sur les conventions Entity Framework s’applique, consultez la section notes.</span><span class="sxs-lookup"><span data-stu-id="7985c-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="7985c-121">Exemple : Convention de modèle de discriminateur</span><span class="sxs-lookup"><span data-stu-id="7985c-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="7985c-122">Renommer des colonnes générées par Entity Framework est un exemple de quelque chose que vous ne pouvez pas faire avec les autres conventions API.</span><span class="sxs-lookup"><span data-stu-id="7985c-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="7985c-123">Il s’agit d’une situation où à l’aide des conventions de modèle est votre seule option.</span><span class="sxs-lookup"><span data-stu-id="7985c-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="7985c-124">Un exemple montrant comment utiliser une convention en fonction de modèle pour configurer les colonnes générées personnalise la manière dont les colonnes de discriminateur sont nommés.</span><span class="sxs-lookup"><span data-stu-id="7985c-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="7985c-125">Voici un exemple d’une convention en fonction de modèle simple qui renomme toutes les colonnes dans le modèle nommé « Discriminator » à « EntityType ».</span><span class="sxs-lookup"><span data-stu-id="7985c-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="7985c-126">Cela inclut les colonnes que le développeur doit simplement nommée « Discriminator ».</span><span class="sxs-lookup"><span data-stu-id="7985c-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="7985c-127">Étant donné que la colonne « Discriminator » est une colonne générée, qu'il doit s’exécuter dans l’espace de S.</span><span class="sxs-lookup"><span data-stu-id="7985c-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="7985c-128">Exemple : Des IA général Convention de changement de nom</span><span class="sxs-lookup"><span data-stu-id="7985c-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="7985c-129">Un autre exemple plus complexe des conventions de modèle basé en action consiste à configurer la façon que les Associations indépendantes (IAs) sont nommées.</span><span class="sxs-lookup"><span data-stu-id="7985c-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="7985c-130">Il s’agit d’une situation où les conventions de modèles sont applicables, car IAs sont générés par Entity Framework et ne sont pas présents dans le modèle qui peut accéder à l’API DbModelBuilder.</span><span class="sxs-lookup"><span data-stu-id="7985c-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="7985c-131">Lors de l’Entity Framework génère un IA, il crée une colonne nommée EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="7985c-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="7985c-132">Par exemple, pour une association nommée Customer avec une colonne de clé nommé CustomerId il générerait une colonne nommée Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="7985c-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="7985c-133">Les bandes de convention suivant le «\_' caractère hors le nom de colonne qui est généré pour l’IA.</span><span class="sxs-lookup"><span data-stu-id="7985c-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="7985c-134">Extension des Conventions existantes</span><span class="sxs-lookup"><span data-stu-id="7985c-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="7985c-135">Si vous avez besoin écrire une convention qui est similaire à un des conventions qui applique déjà les Entity Framework à votre modèle, que vous pouvez toujours étendre cette convention pour éviter d’avoir à réécrire de zéro.</span><span class="sxs-lookup"><span data-stu-id="7985c-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="7985c-136">Un exemple consiste à remplacer l’Id existant correspondant à la convention avec un personnalisé.</span><span class="sxs-lookup"><span data-stu-id="7985c-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="7985c-137">Un avantage supplémentaire à la substitution de la convention de clés est que la méthode substituée est appelée uniquement si aucune clé déjà détecté ou configurée de manière explicite.</span><span class="sxs-lookup"><span data-stu-id="7985c-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="7985c-138">Une liste des conventions utilisées par Entity Framework est disponible ici : [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="7985c-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="7985c-139">Nous devons ensuite ajouter notre nouvelle convention avant la convention de clés existante.</span><span class="sxs-lookup"><span data-stu-id="7985c-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="7985c-140">Une fois que nous ajoutons la CustomKeyDiscoveryConvention, nous pouvons supprimer la IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="7985c-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="7985c-141">Si nous n’avez pas supprimé la IdKeyDiscoveryConvention existante cette convention est toujours prioritaire sur la convention de détection d’Id dans la mesure où il est exécuté tout d’abord, mais dans le cas où aucune propriété de « clé » est trouvée, la convention « id » s’exécute.</span><span class="sxs-lookup"><span data-stu-id="7985c-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="7985c-142">Nous voyons ce comportement, car chaque convention voit le modèle comme mise à jour par la convention précédente (plutôt que de travailler indépendamment sur celle-ci et tous être combinés) afin que si, par exemple, une convention précédente mise à jour un nom de colonne pour faire correspondre un élément de présentant un intérêt pour votre convention personnalisée (lorsque avant que le nom n’a pas d’intérêt), il s’applique à cette colonne.</span><span class="sxs-lookup"><span data-stu-id="7985c-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="7985c-143">Notes</span><span class="sxs-lookup"><span data-stu-id="7985c-143">Notes</span></span>  

<span data-ttu-id="7985c-144">Une liste de conventions qui sont actuellement appliquées par Entity Framework est disponible dans la documentation MSDN ici : [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="7985c-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="7985c-145">Cette liste est extraite directement à partir de notre code source.</span><span class="sxs-lookup"><span data-stu-id="7985c-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="7985c-146">Le code source pour Entity Framework 6 est disponible sur [GitHub](https://github.com/aspnet/entityframework6/) et la plupart des conventions utilisées par Entity Framework sont bon point de départ pour les modèles personnalisés en fonction des conventions.</span><span class="sxs-lookup"><span data-stu-id="7985c-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
