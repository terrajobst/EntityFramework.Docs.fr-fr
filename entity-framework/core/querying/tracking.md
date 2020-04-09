---
title: Tracking vs No-Tracking Questions - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417648"
---
# <a name="tracking-vs-no-tracking-queries"></a>Suivi vs questions de non-suivi

Suivi des contrôles de comportement si Entity Framework Core conservera des informations sur une instance d’entité dans son tracker de changement. Si une entité est suivie, toutes les modifications détectées dans l’entité sont rendues persistantes dans la base de données pendant `SaveChanges()`. EF Core fixera également les propriétés de navigation entre les entités dans un résultat de requête de suivi et les entités qui sont dans le tracker de changement.

> [!NOTE]
> [Les types d’entités sans clé](xref:core/modeling/keyless-entity-types) ne sont jamais suivis. Partout où cet article mentionne les types d’entités, il se réfère aux types d’entités qui ont une clé définie.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) sur GitHub.

## <a name="tracking-queries"></a>Requêtes avec suivi

Par défaut, les requêtes qui retournent des types d’entités ont le suivi activé. Ce qui signifie que vous pouvez apporter des modifications à ces instances d’entité et avoir ces changements persistés par `SaveChanges()`. Dans l’exemple suivant, la modification de l’évaluation des blogs sera détectée et rendue persistante dans la base de données pendant `SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Pas de suivi des requêtes

Les requêtes sans suivi sont utiles lorsque les résultats sont utilisés dans un scénario en lecture seule. Ils sont plus rapides à exécuter parce qu’il n’est pas nécessaire de configurer les informations de suivi des modifications. Si vous n’avez pas besoin de mettre à jour les entités récupérées à partir de la base de données, une requête sans suivi doit être utilisée. Vous pouvez échanger une requête individuelle pour ne pas suivre.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Vous pouvez également modifier le comportement de suivi par défaut au niveau de l’instance du contexte :

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Résolution de l'identité

Étant donné qu’une requête de suivi utilise le tracker de changement, EF Core effectuera une résolution d’identité dans une requête de suivi. Lors de la matérialisation d’une entité, EF Core retournera la même instance entité à partir du tracker de changement si elle est déjà suivie. Si le résultat contient la même entité plusieurs fois, vous revenez le même exemple pour chaque événement. Les requêtes sans suivi n’utilisent pas le tracker de changement et ne font pas de résolution d’identité. Ainsi, vous revenez nouvelle instance de l’entité, même lorsque la même entité est contenue dans le résultat plusieurs fois. Ce comportement était différent dans les versions avant EF Core 3.0, voir [les versions précédentes](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Suivi et projections personnalisées

Même si le type de résultat de la requête n’est pas un type d’entité, EF Core permettra toujours de suivre les types d’entités contenus dans le résultat par défaut. Dans la requête suivante, qui retourne un type anonyme, les instances de `Blog` dans le jeu de résultats sont suivies.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Si l’ensemble de résultats contient des types d’entités provenant de la composition de LINQ, EF Core les suivrea.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Si l’ensemble de résultat ne contient aucun type d’entité, aucun suivi n’est effectué. Dans la requête suivante, nous retournons un type anonyme avec certaines des valeurs de l’entité (mais aucun cas du type d’entité réelle). Il n’y a pas d’entités suivies qui sortent de la requête.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core prend en charge l’évaluation des clients dans la projection de haut niveau. Si EF Core matérialise une entité par exemple pour l’évaluation du client, elle sera suivie. Ici, puisque nous `blog` passons des `StandardizeURL`entités à la méthode client , EF Core va suivre les instances blog trop.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core ne suit pas les instances d’entités sans clé contenues dans le résultat. Mais EF Core suit tous les autres cas de types d’entités avec des clés selon les règles ci-dessus.

Certaines des règles ci-dessus ont fonctionné différemment avant EF Core 3.0. Pour plus d’informations, voir [les versions précédentes](#previous-versions).

## <a name="previous-versions"></a>Versions antérieures

Avant la version 3.0, EF Core avait quelques différences dans la façon dont le suivi a été fait. Les différences notables sont les suivantes :

- Comme expliqué dans la page [d’évaluation client vs serveur,](xref:core/querying/client-eval) EF Core a pris en charge l’évaluation des clients dans n’importe quelle partie de la requête avant la version 3.0. L’évaluation des clients a entraîné la matérialisation des entités, ce qui ne faisait pas partie du résultat. Ef Core a donc analysé le résultat pour détecter ce qu’il faut suivre. Cette conception avait certaines différences comme suit :
  - L’évaluation des clients dans la projection, qui a causé la matérialisation mais n’a pas retourné l’instance de l’entité matérialisée n’a pas été suivie. L’exemple suivant n’a pas suivi les `blog` entités.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core n’a pas suivi les objets sortant de la composition de LINQ dans certains cas. L’exemple suivant n’a pas suivi `Post`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Chaque fois que les résultats des requêtes contenaient des types d’entités sans clé, toute la requête était faite sans suivi. Cela signifie que les types d’entités avec des touches, qui sont en résultat n’ont pas été suivis non plus.
- EF Core a fait la résolution d’identité dans la requête sans suivi. Il a utilisé des références faibles pour garder une trace des entités qui avaient déjà été retournées. Donc, si un ensemble de résultats contenait les mêmes heures multiples entité, vous obtiendrez le même exemple pour chaque événement. Bien que si un résultat précédent avec la même identité est sorti de portée et a obtenu des ordures recueillies, EF Core retourné une nouvelle instance.
