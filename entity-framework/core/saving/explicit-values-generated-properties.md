---
title: "Définition des valeurs explicites pour les propriétés générées - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Définition des valeurs explicites pour les propriétés générées

Une propriété générée est une propriété dont la valeur est générée (soit par la base de données EF) lorsque l’entité est ajoutée ou mis à jour. Consultez [généré de propriétés](../modeling/generated-properties.md) pour plus d’informations.

Il peut arriver dans laquelle vous souhaitez définir une valeur explicite pour une propriété générée, au lieu d’utiliser celui généré.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) sur GitHub.

## <a name="the-model"></a>Le modèle

Le modèle utilisé dans cet article contienne un seul `Employee` entité.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>L’enregistrement d’une valeur explicite lors d’ajout

Le `Employee.EmploymentStarted` propriété est configurée pour avoir des valeurs générées par la base de données pour les nouvelles entités (à l’aide de la valeur par défaut).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Le code suivant insère deux employés dans la base de données.
* Pour la première, aucune valeur n’est attribuée à `Employee.EmploymentStarted` propriété, celui-ci reste définie sur la valeur par défaut CLR `DateTime`.
* Pour la seconde, nous avons défini une valeur explicite de `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Sortie indique que la base de données a généré une valeur pour le premier employé et notre valeur explicite a été utilisé pour la deuxième.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Valeurs explicites dans les colonnes IDENTITY de SQL Server

Par convention le `Employee.EmployeeId` propriété est un magasin généré `IDENTITY` colonne.

Pour la plupart des cas, l’approche illustrée ci-dessus fonctionne pour les propriétés de clé. Toutefois, pour insérer des valeurs explicites dans un serveur SQL Server `IDENTITY` colonne, vous devez activer manuellement `IDENTITY_INSERT` avant d’appeler `SaveChanges()`.

> [!NOTE]  
> Nous avons un [demande de fonctionnalité](https://github.com/aspnet/EntityFramework/issues/703) sur notre backlog procéder automatiquement dans le fournisseur SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Sortie indique que l’ID fournis ont été enregistrés dans la base de données.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Définition d’une valeur explicite pendant la mise à jour

Le `Employee.LastPayRaise` propriété est configurée pour avoir des valeurs générées par la base de données pendant les mises à jour.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Par défaut, EF Core lève une exception si vous essayez d’enregistrer une valeur explicite pour une propriété qui est configurée pour être générés pendant la mise à jour. Pour éviter ce problème, vous avez besoin pour la liste déroulante à l’API de métadonnées au niveau inférieur et définir le `AfterSaveBehavior` (comme indiqué ci-dessus).

> [!NOTE]  
> **Modifications dans EF Core 2.0 :** dans les versions précédentes, le comportement après enregistrement a été contrôlé par le biais du `IsReadOnlyAfterSave` indicateur. Cet indicateur est obsolète et remplacé par `AfterSaveBehavior`.

Il existe également un déclencheur dans la base de données pour générer des valeurs pour le `LastPayRaise` colonne pendant `UPDATE` operations.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Le code suivant augmente le salaire des deux employés dans la base de données.
* Pour la première, aucune valeur n’est attribuée à `Employee.LastPayRaise` propriété, celui-ci reste définie sur null.
* Pour la seconde, nous avons défini une valeur explicite d’une semaine plus tôt (arrière rencontre le salaire raise).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Sortie indique que la base de données a généré une valeur pour le premier employé et notre valeur explicite a été utilisé pour la deuxième.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
