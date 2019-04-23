---
title: Clés (principal) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929834"
---
# <a name="keys-primary"></a><span data-ttu-id="d5ead-102">Clés (primaire)</span><span class="sxs-lookup"><span data-stu-id="d5ead-102">Keys (primary)</span></span>

<span data-ttu-id="d5ead-103">Une clé sert d’identificateur principal unique pour chaque instance d’entité.</span><span class="sxs-lookup"><span data-stu-id="d5ead-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="d5ead-104">Lorsque vous utilisez une base de données relationnelle correspond à la notion d’un *clé primaire*.</span><span class="sxs-lookup"><span data-stu-id="d5ead-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="d5ead-105">Vous pouvez également configurer un identificateur unique qui n’est pas la clé primaire (consultez [clés secondaires](alternate-keys.md) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="d5ead-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="d5ead-106">Une des méthodes suivantes peut être utilisée pour le programme d’installation/créer une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="d5ead-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="d5ead-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="d5ead-107">Conventions</span></span>

<span data-ttu-id="d5ead-108">Par convention, une propriété nommée `Id` ou `<type name>Id` sera configuré en tant que la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="d5ead-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="d5ead-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="d5ead-109">Data Annotations</span></span>

<span data-ttu-id="d5ead-110">Vous pouvez utiliser des Annotations de données pour configurer une propriété unique pour être la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="d5ead-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="d5ead-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d5ead-111">Fluent API</span></span>

<span data-ttu-id="d5ead-112">Vous pouvez utiliser l’API Fluent pour configurer une propriété unique pour être la clé d’une entité.</span><span class="sxs-lookup"><span data-stu-id="d5ead-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="d5ead-113">Vous pouvez également utiliser l’API Fluent pour configurer plusieurs propriétés de la clé d’une entité (appelée une clé composite).</span><span class="sxs-lookup"><span data-stu-id="d5ead-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="d5ead-114">Clés composites ne peuvent être configurés à l’aide de l’API Fluent : conventions d’installation ajoute jamais une clé composite, et vous ne pouvez pas utiliser des Annotations de données pour configurer une.</span><span class="sxs-lookup"><span data-stu-id="d5ead-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
