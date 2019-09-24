---
title: Clés (principale)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197271"
---
# <a name="keys-primary"></a><span data-ttu-id="c4418-102">Clés (principale)</span><span class="sxs-lookup"><span data-stu-id="c4418-102">Keys (primary)</span></span>

<span data-ttu-id="c4418-103">Une clé sert d’identificateur unique principal pour chaque instance d’entité.</span><span class="sxs-lookup"><span data-stu-id="c4418-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="c4418-104">Lorsque vous utilisez une base de données relationnelle, cela correspond au concept d’une *clé primaire*.</span><span class="sxs-lookup"><span data-stu-id="c4418-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="c4418-105">Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (pour plus d’informations, consultez [autres clés](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="c4418-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="c4418-106">L’une des méthodes suivantes peut être utilisée pour configurer/créer une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="c4418-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="c4418-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="c4418-107">Conventions</span></span>

<span data-ttu-id="c4418-108">Par Convention, une propriété nommée `Id` ou `<type name>Id` sera configurée comme clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="c4418-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="c4418-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="c4418-109">Data Annotations</span></span>

<span data-ttu-id="c4418-110">Vous pouvez utiliser des annotations de données pour configurer une seule propriété en tant que clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="c4418-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="c4418-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c4418-111">Fluent API</span></span>

<span data-ttu-id="c4418-112">Vous pouvez utiliser l’API Fluent pour configurer une propriété unique en tant que clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="c4418-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="c4418-113">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés en tant que clé d’une entité (appelée clé composite).</span><span class="sxs-lookup"><span data-stu-id="c4418-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="c4418-114">Les clés composites ne peuvent être configurées qu’à l’aide des conventions de l’API Fluent qui ne configureront jamais de clé composite, et vous ne pourrez pas utiliser d’annotations de données pour en configurer une.</span><span class="sxs-lookup"><span data-stu-id="c4418-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
