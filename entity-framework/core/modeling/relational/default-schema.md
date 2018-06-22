---
title: Schéma par défaut - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052749"
---
# <a name="default-schema"></a><span data-ttu-id="91436-102">Schéma par défaut</span><span class="sxs-lookup"><span data-stu-id="91436-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="91436-103">La configuration de cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="91436-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="91436-104">Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).</span><span class="sxs-lookup"><span data-stu-id="91436-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="91436-105">Le schéma par défaut est le schéma de base de données qui seront créés dans les objets si un schéma n’est pas explicitement configuré pour cet objet.</span><span class="sxs-lookup"><span data-stu-id="91436-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="91436-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="91436-106">Conventions</span></span>

<span data-ttu-id="91436-107">Par convention, le fournisseur de base de données choisit le schéma par défaut plus approprié.</span><span class="sxs-lookup"><span data-stu-id="91436-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="91436-108">Par exemple, Microsoft SQL Server utilisera le `dbo` schéma et SQLite utiliseront pas un schéma (étant donné que les schémas ne sont pas pris en charge dans SQLite).</span><span class="sxs-lookup"><span data-stu-id="91436-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="91436-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="91436-109">Data Annotations</span></span>

<span data-ttu-id="91436-110">Vous ne pouvez pas définir le schéma par défaut à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="91436-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="91436-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="91436-111">Fluent API</span></span>

<span data-ttu-id="91436-112">Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.</span><span class="sxs-lookup"><span data-stu-id="91436-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
