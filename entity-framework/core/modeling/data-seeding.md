---
title: L’amorçage des données - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857427"
---
# <a name="data-seeding"></a>Amorçage des données

L’amorçage des données est le processus de remplissage d’une base de données avec un ensemble initial de données.

Il existe plusieurs façons qu'y parvenir dans EF Core :
* Données d’amorçage de modèle
* Personnalisation d’une migration manuelle
* Logique d’initialisation personnalisé

## <a name="model-seed-data"></a>Données d’amorçage de modèle

> [!NOTE]
> Cette fonctionnalité est une nouveauté d’EF Core 2.1.

Contrairement à dans EF6, dans EF Core, l’amorçage des données peut être associé à un type d’entité dans le cadre de la configuration du modèle. Puis EF Core [migrations](xref:core/managing-schemas/migrations/index) peut calculer automatiquement les éléments insérant, mettre à jour ou supprimer la nécessité d’opérations à appliquer lors de la mise à niveau de la base de données vers une nouvelle version du modèle.

> [!NOTE]
> Migrations considère uniquement les modifications apportées au modèle pour déterminer quelle opération doit être effectuée pour obtenir les données d’amorçage dans l’état souhaité. Par conséquent, les modifications apportées aux données effectuées en dehors de migrations peuvent être perdues ou provoquer une erreur.

Par exemple, cette opération configure les données d’amorçage pour un `Blog` dans `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Pour ajouter des entités ayant une relation de valeurs de clés étrangères doivent être spécifiés :

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Si le type d’entité a des propriétés dans l’état de clichés instantanés qu'une classe anonyme peut être utilisée pour fournir les valeurs :

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Détenues entité types peuvent être exécutées de manière similaire :

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Consultez le [exemple complet de projet](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) pour plus de contexte.

Une fois que les données ont été ajoutées au modèle, [migrations](xref:core/managing-schemas/migrations/index) doit être utilisé pour appliquer les modifications.

> [!TIP]
> Si vous avez besoin appliquer des migrations en tant que partie d’un déploiement automatisé, vous pouvez [créer un script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) qui peuvent être visualisés avant l’exécution.

Vous pouvez également utiliser `context.Database.EnsureCreated()` pour créer une nouvelle base de données contenant les données d’amorçage, par exemple pour une base de données de test ou lorsque vous utilisez le fournisseur en mémoire ou une base de données non-relation. Notez que si la base de données existe déjà, `EnsureCreated()` sera ni mettre à jour les données de schéma ni de valeur initiale dans la base de données. Pour les bases de données relationnelles vous ne devez pas appeler `EnsureCreated()` si vous envisagez d’utiliser des Migrations.

Ce type de données d’amorçage est géré par des migrations et le script pour mettre à jour les données qui se trouve déjà dans la base de données doit être généré sans vous connecter à la base de données. Cela impose certaines restrictions :
* La valeur de clé primaire doit être spécifié même s’il est généralement généré par la base de données. Il sera utilisé pour détecter des modifications de données entre des migrations.
* Les données de départ seront supprimées si la clé primaire est modifiée en aucune façon.

Par conséquent, cette fonctionnalité est particulièrement utile pour les données statiques qui n’a pas censé modifier en dehors des migrations et ne repose pas sur rien d’autre dans la base de données, par exemple les codes postaux.

Si votre scénario comprend les éléments suivants, il est recommandé d’utiliser la logique d’initialisation personnalisé décrite dans la dernière section :
* Données temporaires pour le test
* Données qui varie selon l’état de la base de données
* Données dont a besoin des valeurs de clé devant être généré par la base de données, y compris les entités qui utilisent des clés secondaires en tant que l’identité
* Les données qui nécessite une transformation personnalisée (qui n’est pas gérée par [valeur conversions](xref:core/modeling/value-conversions)), par exemple un hachage de mot de passe
* Données qui nécessite des appels d’API externe, telle que la création des rôles et les utilisateurs de ASP.NET Core Identity

## <a name="manual-migration-customization"></a>Personnalisation d’une migration manuelle

Lorsqu’une migration est ajoutée les modifications apportées aux données spécifiées avec `HasData` sont transformées en appels à `InsertData()`, `UpdateData()`, et `DeleteData()`. Une façon de contourner certaines des limitations de `HasData` consiste à ajouter manuellement ces appels ou [opérations personnalisées](xref:core/managing-schemas/migrations/operations) pour la migration à la place.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Logique d’initialisation personnalisé

Un moyen simple et puissant pour effectuer l’amorçage des données consiste à utiliser [ `DbContext.SaveChanges()` ](xref:core/saving/index) avant le principal de l’application logique commence à s’exécuter.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Le code d’amorçage ne doit pas être de partie de l’exécution d’application normal car cela peut provoquer des problèmes d’accès concurrentiel lorsque plusieurs instances sont en cours d’exécution et qu’il nécessitent également l’application ayant l’autorisation de modifier le schéma de base de données.

Selon les contraintes de votre déploiement, le code d’initialisation peut être exécuté de différentes façons :
* Exécution de l’application de l’initialisation localement
* Déploiement de l’application de l’initialisation avec l’application principale, l’appel de la routine d’initialisation et la désactivation ou la suppression de l’application de l’initialisation.

Cette tâche peut généralement être automatisée à l’aide de [profils de publication](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
