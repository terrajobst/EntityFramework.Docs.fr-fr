---
title: Séquences-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502480"
---
# <a name="sequences"></a><span data-ttu-id="57c10-102">Séquences</span><span class="sxs-lookup"><span data-stu-id="57c10-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="57c10-103">Les séquences sont une fonctionnalité généralement prise en charge uniquement par les bases de données relationnelles.</span><span class="sxs-lookup"><span data-stu-id="57c10-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="57c10-104">Si vous utilisez une base de données non relationnelle telle que Cosmos, consultez la documentation de votre base de données sur la génération de valeurs uniques.</span><span class="sxs-lookup"><span data-stu-id="57c10-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="57c10-105">Une séquence génère des valeurs numériques séquentielles uniques dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="57c10-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="57c10-106">Les séquences ne sont pas associées à une table spécifique, et plusieurs tables peuvent être configurées pour dessiner des valeurs à partir de la même séquence.</span><span class="sxs-lookup"><span data-stu-id="57c10-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="57c10-107">Utilisation de base</span><span class="sxs-lookup"><span data-stu-id="57c10-107">Basic usage</span></span>

<span data-ttu-id="57c10-108">Vous pouvez configurer une séquence dans le modèle, puis l’utiliser pour générer des valeurs pour les propriétés :</span><span class="sxs-lookup"><span data-stu-id="57c10-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="57c10-109">Notez que le SQL spécifique utilisé pour générer une valeur à partir d’une séquence est spécifique à la base de données ; l’exemple ci-dessus fonctionne sur SQL Server mais échoue sur les autres bases de données.</span><span class="sxs-lookup"><span data-stu-id="57c10-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="57c10-110">Pour plus d’informations, consultez la documentation de votre base de données spécifique.</span><span class="sxs-lookup"><span data-stu-id="57c10-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="57c10-111">Configuration des paramètres de séquence</span><span class="sxs-lookup"><span data-stu-id="57c10-111">Configuring sequence settings</span></span>

<span data-ttu-id="57c10-112">Vous pouvez également configurer d’autres aspects de la séquence, tels que son schéma, sa valeur de départ, son incrément, etc. :</span><span class="sxs-lookup"><span data-stu-id="57c10-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
