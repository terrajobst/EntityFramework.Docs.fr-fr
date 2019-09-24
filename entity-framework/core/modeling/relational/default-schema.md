---
title: Schéma par défaut-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197128"
---
# <a name="default-schema"></a>Schéma par défaut

> [!NOTE]  
> La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général. Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).

Le schéma par défaut est le schéma de base de données dans lequel les objets seront créés si un schéma n’est pas explicitement configuré pour cet objet.

## <a name="conventions"></a>Conventions

Par Convention, le fournisseur de base de données choisit le schéma par défaut le plus approprié. Par exemple, Microsoft SQL Server utilise le schéma `dbo` et SQLite n’utilise pas de schéma (puisque les schémas ne sont pas pris en charge dans SQLite).

## <a name="data-annotations"></a>Annotations de données

Vous ne pouvez pas définir le schéma par défaut à l’aide d’annotations de données.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser l’API Fluent pour spécifier un schéma par défaut.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
