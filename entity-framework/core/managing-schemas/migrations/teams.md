---
title: Migrations dans les environnements d’équipe - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228378"
---
<a name="migrations-in-team-environments"></a>Migrations dans les environnements d’équipe
===============================
Lorsque vous travaillez avec des Migrations dans les environnements d’équipe, une attention supplémentaire dans le fichier de capture instantanée du modèle. Ce fichier peut vous indiquer si la migration de votre coéquipier fusionne correctement avec les vôtres ou si vous avez besoin résoudre un conflit en recréant la migration avant de le partager.

<a name="merging"></a>fusion
-------
Lorsque vous fusionnez des migrations à partir de vos collègues, vous pouvez obtenir des conflits dans votre fichier de capture instantanée du modèle. Si les deux modifications ne sont pas liées, la fusion est simple et les deux migrations peuvent coexister. Par exemple, vous pouvez recevoir un conflit de fusion dans la configuration de type d’entité customer qui ressemble à ceci :

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Étant donné que ces deux propriétés doivent exister dans le modèle final, effectuer la fusion en ajoutant les deux propriétés. Dans de nombreux cas, votre système de contrôle de version peut fusionner automatiquement ces modifications pour vous.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Dans ce cas, votre migration et la migration de votre coéquipier sont indépendants de l’autre. Étant donné qu’un d’eux peut être appliqué en premier, vous n’avez pas besoin d’apporter des modifications supplémentaires à votre migration avant de le partager avec votre équipe.

<a name="resolving-conflicts"></a>Résolution des conflits
-------------------
Parfois, vous rencontrez un conflit true lors de la fusion le modèle de capture instantanée du modèle. Par exemple, vous et votre collègue chacun avez renommé la même propriété.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Si vous rencontrez ce type de conflit, résolvez-le en recréant votre migration. Procédez comme suit :

1. Abandonner la fusion et restauration dans votre répertoire de travail avant la fusion
2. Supprimer votre migration (mais conserver vos modifications de modèle)
3. Fusionner les modifications de votre coéquipier dans votre répertoire de travail
4. Ajoutez de nouveau votre migration.

Après cela, les deux migrations peuvent être appliquées dans le bon ordre. Leur migration est d’abord appliquée, modification du nom de la colonne à *Alias*, par la suite votre migration renomme à *nom d’utilisateur*.

Votre migration peut être partagée en toute sécurité avec le reste de l’équipe.
