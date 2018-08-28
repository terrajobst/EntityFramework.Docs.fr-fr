---
title: Mode de mappage prégénéré - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 397569ef374cb44d4938f9e201b588a26c408f6e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996470"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="51f59-102">Vues prégénérées mappage</span><span class="sxs-lookup"><span data-stu-id="51f59-102">Pre-generated mapping views</span></span>
<span data-ttu-id="51f59-103">Avant de l’Entity Framework peut exécuter une requête ou enregistrer les modifications apportées à la source de données, il doit générer un ensemble de vues de mappage pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="51f59-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="51f59-104">Ces vues de mappage sont un ensemble de l’instruction Entity SQL qui représentent la base de données de manière abstraite et font partie des métadonnées qui sont mis en cache par domaine d’application.</span><span class="sxs-lookup"><span data-stu-id="51f59-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="51f59-105">Si vous créez plusieurs instances du même contexte dans le même domaine d’application, elles réutiliseront les vues de mappage depuis les métadonnées mises en cache au lieu de les régénérer.</span><span class="sxs-lookup"><span data-stu-id="51f59-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="51f59-106">Étant donné que la génération de vues de mappage constitue une partie significative du coût total de l’exécution de la première requête, Entity Framework vous permet de prégénérer des vues de mappage et de les inclure dans le projet compilé.</span><span class="sxs-lookup"><span data-stu-id="51f59-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="51f59-107">Pour plus d’informations, consultez [considérations sur les performances (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="51f59-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="51f59-108">Génération de mappage des vues avec les outils EF Power Tools</span><span class="sxs-lookup"><span data-stu-id="51f59-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="51f59-109">Pour prégénérer des vues le plus simple consiste à utiliser le [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="51f59-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="51f59-110">Une fois que vous avez installé les outils Power avoir une option de menu pour générer des vues, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="51f59-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="51f59-111">Pour **Code First** modèles avec le bouton droit sur le fichier de code qui contient votre classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="51f59-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="51f59-112">Pour **Entity Framework Designer** modèles avec le bouton droit sur votre fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="51f59-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="51f59-114">Une fois le processus est terminé vous offrira une classe similaire à ce qui suit généré</span><span class="sxs-lookup"><span data-stu-id="51f59-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="51f59-116">Désormais, lorsque vous exécutez votre application EF utilise cette classe pour charger des vues en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="51f59-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="51f59-117">Si votre modèle change et vous ne ré-générez pas de cette classe EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="51f59-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="51f59-118">Génération de mappage des vues à partir du Code - EF6 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="51f59-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="51f59-119">Les autres pour générer des vues consiste à utiliser les API Entity Framework fournit.</span><span class="sxs-lookup"><span data-stu-id="51f59-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="51f59-120">Lorsque vous utilisez cette méthode que vous avez la possibilité de sérialiser les vues mais vous le souhaitez, mais vous devez également charger les vues vous-même.</span><span class="sxs-lookup"><span data-stu-id="51f59-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="51f59-121">**EF6 et versions ultérieures uniquement** -les API présentés dans cette section ont été introduits dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="51f59-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="51f59-122">Si vous utilisez une version antérieure, que ces informations ne s’applique pas.</span><span class="sxs-lookup"><span data-stu-id="51f59-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="51f59-123">Génération de vues</span><span class="sxs-lookup"><span data-stu-id="51f59-123">Generating Views</span></span>

<span data-ttu-id="51f59-124">Les API pour générer des vues se trouvent sur la classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="51f59-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="51f59-125">Vous pouvez récupérer une StorageMappingCollection pour un contexte à l’aide d’un ObjectContext MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="51f59-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="51f59-126">Si vous utilisez la nouvelle API DbContext, puis vous pouvez y accéder à l’aide de la IObjectContextAdapter comme ci-dessous, dans ce code, nous avons une instance de votre DbContext dérivée appelée dbContext :</span><span class="sxs-lookup"><span data-stu-id="51f59-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="51f59-127">Une fois que vous avez StorageMappingItemCollection puis vous pouvez accéder aux méthodes GenerateViews et ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="51f59-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="51f59-128">La première méthode crée un dictionnaire avec une entrée pour chaque vue dans le mappage de conteneur.</span><span class="sxs-lookup"><span data-stu-id="51f59-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="51f59-129">La deuxième méthode calcule une valeur de hachage pour le mappage de conteneur unique et est utilisée lors de l’exécution pour valider que le modèle n’a pas changé dans la mesure où les vues ont été préalablement générés.</span><span class="sxs-lookup"><span data-stu-id="51f59-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="51f59-130">Remplacements des deux méthodes sont fournies pour les scénarios complexes impliquant plusieurs mappages de conteneur.</span><span class="sxs-lookup"><span data-stu-id="51f59-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="51f59-131">Lors de la génération de vues vous appelez la méthode GenerateViews, puis écrire EntitySetBase et DbMappingView résultants.</span><span class="sxs-lookup"><span data-stu-id="51f59-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="51f59-132">Vous devez également stocker le hachage généré par la méthode ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="51f59-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="51f59-133">Chargement des vues</span><span class="sxs-lookup"><span data-stu-id="51f59-133">Loading Views</span></span>

<span data-ttu-id="51f59-134">Pour charger les vues générées par la méthode GenerateViews, vous pouvez fournir EF avec une classe qui hérite de la classe abstraite DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="51f59-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="51f59-135">DbMappingViewCache spécifie deux méthodes que vous devez implémenter :</span><span class="sxs-lookup"><span data-stu-id="51f59-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="51f59-136">La propriété MappingHashValue doit retourner le hachage généré par la méthode ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="51f59-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="51f59-137">Lorsque EF est accédant à poser pour les vues, il sera tout d’abord générer et comparer la valeur de hachage du modèle avec le hachage retourné par cette propriété.</span><span class="sxs-lookup"><span data-stu-id="51f59-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="51f59-138">Si elles ne correspondent pas EF lève une exception EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="51f59-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="51f59-139">La méthode GetView acceptera une EntitySetBase et vous devez renvoyer un DbMappingVIew contenant EntitySql qui a été généré pour qui a été associé à la EntitySetBase donné dans le dictionnaire généré par la méthode GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="51f59-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="51f59-140">Si la demande à EF pour une vue que vous n’avez pas puis GetView doit retourner null.</span><span class="sxs-lookup"><span data-stu-id="51f59-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="51f59-141">Voici un extrait de DbMappingViewCache qui est généré avec les outils Power Tools, comme décrit ci-dessus, que nous voyons comment stocker et récupérer le EntitySql requis.</span><span class="sxs-lookup"><span data-stu-id="51f59-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="51f59-142">Pour que l’utilisation d’EF votre DbMappingViewCache vous ajouter utilisent le DbMappingViewCacheTypeAttribute, en spécifiant le contexte dans lequel il a été créé pour.</span><span class="sxs-lookup"><span data-stu-id="51f59-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="51f59-143">Dans le code ci-dessous, nous associons la BlogContext avec la classe MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="51f59-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="51f59-144">Pour des scénarios plus complexes, les instances de cache de vue de mappage peuvent être fournis en spécifiant une fabrique de cache de vue de mappage.</span><span class="sxs-lookup"><span data-stu-id="51f59-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="51f59-145">Cela est possible en implémentant la classe abstraite System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="51f59-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="51f59-146">L’instance de la fabrique de cache de vue de mappage qui est utilisée peut être récupérée ou définie à l’aide de la StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="51f59-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
