---
title: Table d’historique des migrations personnalisées-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416842"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="146bf-102">Table d’historique des migrations personnalisées</span><span class="sxs-lookup"><span data-stu-id="146bf-102">Custom Migrations History Table</span></span>

<span data-ttu-id="146bf-103">Par défaut, EF Core effectue le suivi des migrations qui ont été appliquées à la base de données en les enregistrant dans une table nommée `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="146bf-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="146bf-104">Pour différentes raisons, vous souhaiterez peut-être personnaliser ce tableau pour mieux répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="146bf-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="146bf-105">Si vous personnalisez la table d’historique des migrations *après avoir* appliqué des migrations, vous êtes responsable de la mise à jour de la table existante dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="146bf-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="146bf-106">Nom du schéma et de la table</span><span class="sxs-lookup"><span data-stu-id="146bf-106">Schema and table name</span></span>

<span data-ttu-id="146bf-107">Vous pouvez modifier le schéma et le nom de la table à l’aide de la méthode `MigrationsHistoryTable()` dans `OnConfiguring()` (ou `ConfigureServices()` sur ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="146bf-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="146bf-108">Voici un exemple d’utilisation du fournisseur SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="146bf-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="146bf-109">Autres modifications</span><span class="sxs-lookup"><span data-stu-id="146bf-109">Other changes</span></span>

<span data-ttu-id="146bf-110">Pour configurer des aspects supplémentaires de la table, substituez et remplacez le service `IHistoryRepository` spécifique au fournisseur.</span><span class="sxs-lookup"><span data-stu-id="146bf-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="146bf-111">Voici un exemple de modification du nom de la colonne MigrationId en *ID* sur SQL Server.</span><span class="sxs-lookup"><span data-stu-id="146bf-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="146bf-112">`SqlServerHistoryRepository` est à l’intérieur d’un espace de noms interne et peut changer dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="146bf-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
