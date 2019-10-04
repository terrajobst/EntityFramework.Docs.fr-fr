---
title: Fournisseur Azure Cosmos DB - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813544"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="2d0a6-102">Fournisseur Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="2d0a6-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="2d0a6-103">Ce fournisseur est nouveau dans EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="2d0a6-104">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="2d0a6-105">Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="2d0a6-106">Avant de lire cette section, il est fortement recommandé de vous familiariser avec la documentation d’[Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="2d0a6-107">Installez</span><span class="sxs-lookup"><span data-stu-id="2d0a6-107">Install</span></span>

<span data-ttu-id="2d0a6-108">Installez le [package NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="2d0a6-109">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d0a6-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="2d0a6-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d0a6-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="2d0a6-111">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="2d0a6-111">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="2d0a6-112">Vous pouvez afficher cet [exemple sur GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-112">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="2d0a6-113">Comme pour les autres fournisseurs, la première étape consiste à appeler `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="2d0a6-113">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="2d0a6-114">Le point de terminaison et la clé sont codés en dur ici par souci de simplicité, mais dans une application de production, ils doivent être [stockés de manière sécurisée](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="2d0a6-114">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="2d0a6-115">Dans cet exemple, `Order` est une entité simple avec une référence au [type détenu](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-115">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="2d0a6-116">L’enregistrement et l’interrogation des données suivent le modèle EF normal :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-116">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="2d0a6-117">L’appel de `EnsureCreated` est nécessaire pour créer les collections requises et insérer les [données initiales](../../modeling/data-seeding.md) si elles sont présentes dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-117">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="2d0a6-118">Toutefois, `EnsureCreated` ne doit être appelé qu’au cours du déploiement, pas pendant le fonctionnement normal, car cela peut entraîner des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-118">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="2d0a6-119">Personnalisation du modèle propre à Cosmos</span><span class="sxs-lookup"><span data-stu-id="2d0a6-119">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="2d0a6-120">Par défaut, tous les types d’entité sont mappés au même conteneur, nommé d’après le contexte dérivé (`"OrderContext"`dans le cas présent).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-120">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="2d0a6-121">Pour changer le nom de conteneur par défaut, utilisez `HasDefaultContainer` :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-121">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="2d0a6-122">Pour mapper un type d’entité à un autre conteneur, utilisez `ToContainer` :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-122">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="2d0a6-123">Pour identifier le type d’entité représenté par un élément donné, EF Core ajoute une valeur de discriminateur même en l’absence de type d’entité dérivé.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-123">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="2d0a6-124">Le nom et la valeur du discriminateur [peuvent être changés](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="2d0a6-124">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="2d0a6-125">Entités incorporées</span><span class="sxs-lookup"><span data-stu-id="2d0a6-125">Embedded Entities</span></span>

<span data-ttu-id="2d0a6-126">Pour Cosmos, les entités détenues sont incorporées dans le même élément que le propriétaire.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-126">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="2d0a6-127">Pour changer un nom de propriété, utilisez `ToJsonProperty` :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-127">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="2d0a6-128">Avec cette configuration, la commande dans l’exemple ci-dessus est stockée comme suit :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-128">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="2d0a6-129">Les collections d’entités détenues sont également incorporées.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-129">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="2d0a6-130">Pour l’exemple suivant, nous allons utiliser la classe `Distributor` avec la collection `StreetAddress` :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-130">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="2d0a6-131">Les entités détenues n’ont pas besoin de fournir des valeurs de clés explicites pour être stockées :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-131">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="2d0a6-132">Elles sont conservées de cette façon :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-132">They will be persisted in this way:</span></span>

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

<span data-ttu-id="2d0a6-133">En interne, EF Core doit toujours avoir des valeurs de clé uniques pour toutes les entités suivies.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-133">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="2d0a6-134">La clé primaire créée par défaut pour les collections de types détenus se compose des propriétés de clé étrangère qui pointent vers le propriétaire et d’une propriété `int` correspondant à l’index dans le tableau JSON.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-134">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="2d0a6-135">Pour récupérer ces valeurs, vous pouvez utiliser l’API d’entrée :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-135">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="2d0a6-136">Quand cela est nécessaire, la clé primaire par défaut pour les types d’entités détenus peut être changée, mais les valeurs de clé doivent être fournies explicitement.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-136">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="2d0a6-137">Utilisation d’entités déconnectées</span><span class="sxs-lookup"><span data-stu-id="2d0a6-137">Working with Disconnected Entities</span></span>

<span data-ttu-id="2d0a6-138">Chaque élément doit avoir une valeur `id` unique pour la clé de partition donnée.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-138">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="2d0a6-139">Par défaut, EF Core génère la valeur en concaténant les valeurs du discriminateur et de la clé primaire, au moyen du caractère « | » en guise de délimiteur.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-139">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="2d0a6-140">Les valeurs de clé sont générées uniquement quand une entité passe à l’état `Added`.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-140">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="2d0a6-141">Cela peut poser un problème lors de l’[attachement d’entités](../../saving/disconnected-entities.md) si elles n’ont pas de propriété `id` sur le type .NET pour stocker la valeur.</span><span class="sxs-lookup"><span data-stu-id="2d0a6-141">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="2d0a6-142">Pour contourner cette limitation, il est possible de créer et de définir la valeur `id` manuellement, ou bien de marquer l’entité comme étant ajoutée, puis de lui affecter l’état souhaité :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-142">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="2d0a6-143">Voici le code JSON résultant :</span><span class="sxs-lookup"><span data-stu-id="2d0a6-143">This is the resulting JSON:</span></span>

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