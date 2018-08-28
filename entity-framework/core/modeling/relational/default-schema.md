---
title: Schéma par défaut - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995364"
---
# <a name="default-schema"></a><span data-ttu-id="39d3f-102">Schéma par défaut</span><span class="sxs-lookup"><span data-stu-id="39d3f-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="39d3f-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="39d3f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="39d3f-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="39d3f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="39d3f-105">Le schéma par défaut est le schéma de base de données qui seront créées dans les objets si un schéma n’est pas explicitement configuré pour cet objet.</span><span class="sxs-lookup"><span data-stu-id="39d3f-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="39d3f-106">Conventions</span><span class="sxs-lookup"><span data-stu-id="39d3f-106">Conventions</span></span>

<span data-ttu-id="39d3f-107">Par convention, le fournisseur de base de données choisit le schéma par défaut plus approprié.</span><span class="sxs-lookup"><span data-stu-id="39d3f-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="39d3f-108">Par exemple, Microsoft SQL Server utilisera le `dbo` schéma et SQLite pas utilisent un schéma (étant donné que les schémas ne sont pas pris en charge dans SQLite).</span><span class="sxs-lookup"><span data-stu-id="39d3f-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="39d3f-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="39d3f-109">Data Annotations</span></span>

<span data-ttu-id="39d3f-110">Vous ne pouvez pas définir le schéma par défaut à l’aide des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="39d3f-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="39d3f-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="39d3f-111">Fluent API</span></span>

<span data-ttu-id="39d3f-112">Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.</span><span class="sxs-lookup"><span data-stu-id="39d3f-112">You can use the Fluent API to specify a default schema.</span></span>

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
