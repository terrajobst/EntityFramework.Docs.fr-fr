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
# <a name="default-schema"></a>Schéma par défaut

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le schéma par défaut est le schéma de base de données qui seront créées dans les objets si un schéma n’est pas explicitement configuré pour cet objet.

## <a name="conventions"></a>Conventions

Par convention, le fournisseur de base de données choisit le schéma par défaut plus approprié. Par exemple, Microsoft SQL Server utilisera le `dbo` schéma et SQLite pas utilisent un schéma (étant donné que les schémas ne sont pas pris en charge dans SQLite).

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
