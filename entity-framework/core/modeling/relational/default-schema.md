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
# <a name="default-schema"></a>Schéma par défaut

> [!NOTE]  
> La configuration de cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiqués ici devient disponibles lorsque vous installez un fournisseur de base de données relationnelle (en raison de l’élément partagé *Microsoft.EntityFrameworkCore.Relational* package).

Le schéma par défaut est le schéma de base de données qui seront créés dans les objets si un schéma n’est pas explicitement configuré pour cet objet.

## <a name="conventions"></a>Conventions

Par convention, le fournisseur de base de données choisit le schéma par défaut plus approprié. Par exemple, Microsoft SQL Server utilisera le `dbo` schéma et SQLite utiliseront pas un schéma (étant donné que les schémas ne sont pas pris en charge dans SQLite).

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas définir le schéma par défaut à l’aide des Annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.

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
