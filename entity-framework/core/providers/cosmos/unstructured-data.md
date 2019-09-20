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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Utilisation de données non structurées dans EF Core fournisseur Azure Cosmos DB

EF Core a été conçu pour faciliter l’utilisation des données qui suivent un schéma défini dans le modèle. Toutefois, l’une des forces de Azure Cosmos DB est la flexibilité dans la forme des données stockées.

## <a name="accessing-the-raw-json"></a>Accès au JSON brut

Il est possible d’accéder aux propriétés qui ne sont pas suivies par EF Core via une propriété spéciale dans l' [État Shadow,](../../modeling/shadow-properties.md) nommée `"__jObject"` , `JObject` qui contient un représentant les données reçues du magasin et les données qui seront stockées :

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
> La `"__jObject"` propriété fait partie de l’infrastructure EF Core et ne doit être utilisée qu’en dernier recours, car elle est susceptible d’avoir un comportement différent dans les futures versions.

> [!NOTE]
> Les modifications apportées à l’entité remplacent les `"__jObject"` valeurs `SaveChanges`stockées dans pendant.

## <a name="using-cosmosclient"></a>Utilisation de CosmosClient

Pour découpler complètement de EF Core récupérez l' `CosmosClient` objet qui fait [partie du kit de développement logiciel (SDK) Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) à partir de : `DbContext`

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Valeurs de propriété manquantes

Dans l’exemple précédent, nous avons `"TrackingNumber"` supprimé la propriété de la commande. En raison du fonctionnement de l’indexation dans Cosmos DB, les requêtes qui font référence à la propriété manquante ailleurs que dans la projection peuvent retourner des résultats inattendus. Par exemple :

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

En fait, la requête triée ne retourne aucun résultat. Cela signifie qu’il faut veiller à toujours remplir les propriétés mappées par EF Core quand vous travaillez directement avec le magasin.

> [!NOTE]
> Ce comportement peut changer dans les versions ultérieures de Cosmos. Par exemple, si la stratégie d’indexation définit l’index composite {ID/ ? ASC, TrackingNumber/ ? ASC)}, une requête dont le a’order by c.ID ASC, c. discriminateur __ASC’retourne__ des éléments pour lesquels la `"TrackingNumber"` propriété est manquante.