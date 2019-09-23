---
title: Fournisseur Azure Cosmos DB - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: c753bb71089c91cbb26b970cddd118645fb18d56
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150726"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Fournisseur Azure Cosmos DB EF Core

>[!NOTE]
> Ce fournisseur est nouveau dans EF Core 3.0.

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Azure Cosmos DB. Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Avant de lire cette section, il est fortement recommandé de vous familiariser avec la documentation d’[Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a>Bien démarrer

> [!TIP]  
> Vous pouvez afficher cet [exemple sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Comme pour les autres fournisseurs, la première étape consiste à appeler `UseCosmos`: [!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Le point de terminaison et la clé sont codés en dur ici par souci de simplicité, mais dans une application de production, ils doivent être [stockés de manière sécurisée](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)

Dans cet exemple, `Order` est une entité simple avec une référence au [type détenu](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

L’enregistrement et l’interrogation des données suivent le modèle EF normal : [!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> L’appel de `EnsureCreated` est nécessaire pour créer les collections requises et insérer les [données initiales](../../modeling/data-seeding.md) si elles sont présentes dans le modèle. Toutefois, `EnsureCreated` ne doit être appelé qu’au cours du déploiement, pas pendant le fonctionnement normal, car cela peut entraîner des problèmes de performances.

## <a name="cosmos-specific-model-customization"></a>Personnalisation du modèle propre à Cosmos

Par défaut, tous les types d’entité sont mappés au même conteneur, nommé d’après le contexte dérivé (`"OrderContext"`dans le cas présent). Pour changer le nom de conteneur par défaut, utilisez `HasDefaultContainer` :

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Pour mapper un type d’entité à un autre conteneur, utilisez `ToContainer` :

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Pour identifier le type d’entité représenté par un élément donné, EF Core ajoute une valeur de discriminateur même en l’absence de type d’entité dérivé. Le nom et la valeur du discriminateur [peuvent être changés](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Entités incorporées

Pour Cosmos, les entités détenues sont incorporées dans le même élément que le propriétaire. Pour changer un nom de propriété, utilisez `ToJsonProperty` :

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Avec cette configuration, la commande dans l’exemple ci-dessus est stockée comme suit :

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Les collections d’entités détenues sont également incorporées. Pour l’exemple suivant, nous allons utiliser la classe `Distributor` avec la collection `StreetAddress` :

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Les entités détenues n’ont pas besoin de fournir des valeurs de clés explicites pour être stockées :

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Elles sont conservées de cette façon :

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

En interne, EF Core doit toujours avoir des valeurs de clé uniques pour toutes les entités suivies. La clé primaire créée par défaut pour les collections de types détenus se compose des propriétés de clé étrangère qui pointent vers le propriétaire et d’une propriété `int` correspondant à l’index dans le tableau JSON. Pour récupérer ces valeurs, vous pouvez utiliser l’API d’entrée :

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Quand cela est nécessaire, la clé primaire par défaut pour les types d’entités détenus peut être changée, mais les valeurs de clé doivent être fournies explicitement.

## <a name="working-with-disconnected-entities"></a>Utilisation d’entités déconnectées

Chaque élément doit avoir une valeur `id` unique pour la clé de partition donnée. Par défaut, EF Core génère la valeur en concaténant les valeurs du discriminateur et de la clé primaire, au moyen du caractère « | » en guise de délimiteur. Les valeurs de clé sont générées uniquement quand une entité passe à l’état `Added`. Cela peut poser un problème lors de l’[attachement d’entités](../../saving/disconnected-entities.md) si elles n’ont pas de propriété `id` sur le type .NET pour stocker la valeur.

Pour contourner cette limitation, il est possible de créer et de définir la valeur `id` manuellement, ou bien de marquer l’entité comme étant ajoutée, puis de lui affecter l’état souhaité :

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Voici le code JSON résultant :

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```