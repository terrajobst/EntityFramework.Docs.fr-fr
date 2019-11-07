---
title: Table d’historique des migrations personnalisées-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655717"
---
# <a name="custom-migrations-history-table"></a>Table d’historique des migrations personnalisées

Par défaut, EF Core effectue le suivi des migrations qui ont été appliquées à la base de données en les enregistrant dans une table nommée `__EFMigrationsHistory`. Pour différentes raisons, vous souhaiterez peut-être personnaliser ce tableau pour mieux répondre à vos besoins.

> [!IMPORTANT]
> Si vous personnalisez la table d’historique des migrations *après avoir* appliqué des migrations, vous êtes responsable de la mise à jour de la table existante dans la base de données.

## <a name="schema-and-table-name"></a>Nom du schéma et de la table

Vous pouvez modifier le schéma et le nom de la table à l’aide de la méthode `MigrationsHistoryTable()` dans `OnConfiguring()` (ou `ConfigureServices()` sur ASP.NET Core). Voici un exemple d’utilisation du fournisseur SQL Server EF Core.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Autres modifications

Pour configurer des aspects supplémentaires de la table, substituez et remplacez le service `IHistoryRepository` spécifique au fournisseur. Voici un exemple de modification du nom de la colonne MigrationId en *ID* sur SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` est à l’intérieur d’un espace de noms interne et peut changer dans les versions ultérieures.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
