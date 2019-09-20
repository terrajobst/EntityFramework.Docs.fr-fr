---
title: Fournisseur de Azure Cosmos DB-utilisation de données non structurées-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150815"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="ed7e3-102">Utilisation de données non structurées dans EF Core fournisseur Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ed7e3-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="ed7e3-103">EF Core a été conçu pour faciliter l’utilisation des données qui suivent un schéma défini dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="ed7e3-104">Toutefois, l’une des forces de Azure Cosmos DB est la flexibilité dans la forme des données stockées.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="ed7e3-105">Accès au JSON brut</span><span class="sxs-lookup"><span data-stu-id="ed7e3-105">Accessing the raw JSON</span></span>

<span data-ttu-id="ed7e3-106">Il est possible d’accéder aux propriétés qui ne sont pas suivies par EF Core via une propriété spéciale dans l' [État Shadow,](../../modeling/shadow-properties.md) nommée `"__jObject"` , `JObject` qui contient un représentant les données reçues du magasin et les données qui seront stockées :</span><span class="sxs-lookup"><span data-stu-id="ed7e3-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="ed7e3-107">La `"__jObject"` propriété fait partie de l’infrastructure EF Core et ne doit être utilisée qu’en dernier recours, car elle est susceptible d’avoir un comportement différent dans les futures versions.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="ed7e3-108">Les modifications apportées à l’entité remplacent les `"__jObject"` valeurs `SaveChanges`stockées dans pendant.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="ed7e3-109">Utilisation de CosmosClient</span><span class="sxs-lookup"><span data-stu-id="ed7e3-109">Using CosmosClient</span></span>

<span data-ttu-id="ed7e3-110">Pour découpler complètement de EF Core récupérez l' `CosmosClient` objet qui fait [partie du kit de développement logiciel (SDK) Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) à partir de : `DbContext`</span><span class="sxs-lookup"><span data-stu-id="ed7e3-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="ed7e3-111">Valeurs de propriété manquantes</span><span class="sxs-lookup"><span data-stu-id="ed7e3-111">Missing property values</span></span>

<span data-ttu-id="ed7e3-112">Dans l’exemple précédent, nous avons `"TrackingNumber"` supprimé la propriété de la commande.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="ed7e3-113">En raison du fonctionnement de l’indexation dans Cosmos DB, les requêtes qui font référence à la propriété manquante ailleurs que dans la projection peuvent retourner des résultats inattendus.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="ed7e3-114">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ed7e3-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="ed7e3-115">En fait, la requête triée ne retourne aucun résultat.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="ed7e3-116">Cela signifie qu’il faut veiller à toujours remplir les propriétés mappées par EF Core quand vous travaillez directement avec le magasin.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="ed7e3-117">Ce comportement peut changer dans les versions ultérieures de Cosmos.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="ed7e3-118">Par exemple, si la stratégie d’indexation définit l’index composite {ID/ ?</span><span class="sxs-lookup"><span data-stu-id="ed7e3-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="ed7e3-119">ASC, TrackingNumber/ ?</span><span class="sxs-lookup"><span data-stu-id="ed7e3-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="ed7e3-120">ASC)}, une requête dont le a’order by c.ID ASC, c. discriminateur __ASC’retourne__ des éléments pour lesquels la `"TrackingNumber"` propriété est manquante.</span><span class="sxs-lookup"><span data-stu-id="ed7e3-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>