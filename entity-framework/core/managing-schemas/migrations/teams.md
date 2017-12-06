---
title: "Migrations dans des environnements d’équipe - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migrations dans des environnements d’équipe
===============================
Lorsque vous utilisez Migrations dans les environnements d’équipe, une attention supplémentaire dans le fichier de capture instantanée de modèle. Ce fichier peut vous indiquer si la migration de votre idéal fusionne correctement avec les vôtres de si vous avez besoin résoudre un conflit en recréant la migration avant de le partager.

<a name="merging"></a>fusion
-------
Lorsque vous fusionnez des migrations à partir de vos coéquipiers, vous pouvez obtenir des conflits dans votre fichier de capture instantanée de modèle. Si les deux modifications ne sont pas liées, la fusion est triviale, et les deux migrations peuvent coexister. Par exemple, vous pouvez recevoir un conflit de fusion dans la configuration de type d’entité customer qui ressemble à ceci :

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Étant donné que ces deux propriétés doivent exister dans le modèle final, fin de la fusion en ajoutant les deux propriétés. Dans de nombreux cas, votre système de contrôle de version peut automatiquement fusionner ces modifications pour vous.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Dans ces cas, la migration et la migration de votre idéal sont indépendants. Étant donné qu’un d’eux peut être appliqué en premier, vous n’avez pas besoin apporter des modifications supplémentaires à la migration avant de le partager avec votre équipe.

<a name="resolving-conflicts"></a>Résolution des conflits
-------------------
Parfois, vous rencontrez un conflit true lors de la fusion du modèle d’instantané. Par exemple, vous et votre idéal chacun avez renommé la même propriété.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Si vous rencontrez ce genre de conflit, résolvez-le en recréant la migration. Procédez comme suit :

1. Abandon de la fusion et restauration dans votre répertoire de travail avant la fusion
2. Supprimer votre migration (en conservant vos modifications de modèle)
3. Fusionner les modifications de votre idéal dans votre répertoire de travail
4. Ajoutez de nouveau votre migration.

Après cette opération, les deux migrations peuvent être appliquées dans l’ordre approprié. Leur migration est appliquée en premier, modification du nom de la colonne à *Alias*, par la suite votre migration renomme à *nom d’utilisateur*.

Votre migration peut être partagée en toute sécurité avec le reste de l’équipe.
