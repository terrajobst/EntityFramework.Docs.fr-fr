---
title: Mode de mappage prégénéré - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459524"
---
# <a name="pre-generated-mapping-views"></a>Vues prégénérées mappage
Avant de l’Entity Framework peut exécuter une requête ou enregistrer les modifications apportées à la source de données, il doit générer un ensemble de vues de mappage pour accéder à la base de données. Ces vues de mappage sont un ensemble de l’instruction Entity SQL qui représentent la base de données de manière abstraite et font partie des métadonnées qui sont mis en cache par domaine d’application. Si vous créez plusieurs instances du même contexte dans le même domaine d’application, elles réutiliseront les vues de mappage depuis les métadonnées mises en cache au lieu de les régénérer. Étant donné que la génération de vues de mappage constitue une partie significative du coût total de l’exécution de la première requête, Entity Framework vous permet de prégénérer des vues de mappage et de les inclure dans le projet compilé. Pour plus d’informations, consultez [considérations sur les performances (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Génération de mappage des vues avec l’édition Community EF Power Tools

Pour prégénérer des vues le plus simple consiste à utiliser le [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Une fois que vous avez installé les outils Power avoir une option de menu pour générer des vues, comme indiqué ci-dessous.

-   Pour **Code First** modèles avec le bouton droit sur le fichier de code qui contient votre classe DbContext.
-   Pour **Entity Framework Designer** modèles avec le bouton droit sur votre fichier EDMX.

![générer des vues](~/ef6/media/generateviews.png)

Une fois le processus est terminé vous offrira une classe similaire à ce qui suit généré

![Vues générées](~/ef6/media/generatedviews.png)

Désormais, lorsque vous exécutez votre application EF utilise cette classe pour charger des vues en fonction des besoins. Si votre modèle change et vous ne ré-générez pas de cette classe EF lève une exception.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Génération de mappage des vues à partir du Code - EF6 et versions ultérieures

Les autres pour générer des vues consiste à utiliser les API Entity Framework fournit. Lorsque vous utilisez cette méthode que vous avez la possibilité de sérialiser les vues mais vous le souhaitez, mais vous devez également charger les vues vous-même.

> [!NOTE]
> **EF6 et versions ultérieures uniquement** -les API présentés dans cette section ont été introduits dans Entity Framework 6. Si vous utilisez une version antérieure, que ces informations ne s’applique pas.

### <a name="generating-views"></a>Génération de vues

Les API pour générer des vues se trouvent sur la classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection. Vous pouvez récupérer une StorageMappingCollection pour un contexte à l’aide d’un ObjectContext MetadataWorkspace. Si vous utilisez la nouvelle API DbContext, puis vous pouvez y accéder à l’aide de la IObjectContextAdapter comme ci-dessous, dans ce code, nous avons une instance de votre DbContext dérivée appelée dbContext :

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Une fois que vous avez StorageMappingItemCollection puis vous pouvez accéder aux méthodes GenerateViews et ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

La première méthode crée un dictionnaire avec une entrée pour chaque vue dans le mappage de conteneur. La deuxième méthode calcule une valeur de hachage pour le mappage de conteneur unique et est utilisée lors de l’exécution pour valider que le modèle n’a pas changé dans la mesure où les vues ont été préalablement générés. Remplacements des deux méthodes sont fournies pour les scénarios complexes impliquant plusieurs mappages de conteneur.

Lors de la génération de vues vous appelez la méthode GenerateViews, puis écrire EntitySetBase et DbMappingView résultants. Vous devez également stocker le hachage généré par la méthode ComputeMappingHashValue.

### <a name="loading-views"></a>Chargement des vues

Pour charger les vues générées par la méthode GenerateViews, vous pouvez fournir EF avec une classe qui hérite de la classe abstraite DbMappingViewCache. DbMappingViewCache spécifie deux méthodes que vous devez implémenter :

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

La propriété MappingHashValue doit retourner le hachage généré par la méthode ComputeMappingHashValue. Lorsque EF est accédant à poser pour les vues, il sera tout d’abord générer et comparer la valeur de hachage du modèle avec le hachage retourné par cette propriété. Si elles ne correspondent pas EF lève une exception EntityCommandCompilationException.

La méthode GetView acceptera une EntitySetBase et vous devez renvoyer un DbMappingVIew contenant EntitySql qui a été généré pour qui a été associé à la EntitySetBase donné dans le dictionnaire généré par la méthode GenerateViews. Si la demande à EF pour une vue que vous n’avez pas puis GetView doit retourner null.

Voici un extrait de DbMappingViewCache qui est généré avec les outils Power Tools, comme décrit ci-dessus, que nous voyons comment stocker et récupérer le EntitySql requis.

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

Pour que l’utilisation d’EF votre DbMappingViewCache vous ajouter utilisent le DbMappingViewCacheTypeAttribute, en spécifiant le contexte dans lequel il a été créé pour. Dans le code ci-dessous, nous associons la BlogContext avec la classe MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Pour des scénarios plus complexes, les instances de cache de vue de mappage peuvent être fournis en spécifiant une fabrique de cache de vue de mappage. Cela est possible en implémentant la classe abstraite System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. L’instance de la fabrique de cache de vue de mappage qui est utilisée peut être récupérée ou définie à l’aide de la StorageMappingItemCollection.MappingViewCacheFactoryproperty.
