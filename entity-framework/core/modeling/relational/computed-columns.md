---
title: Colonnes calculées-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197233"
---
# <a name="computed-columns"></a><span data-ttu-id="35222-102">Colonnes calculées</span><span class="sxs-lookup"><span data-stu-id="35222-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="35222-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="35222-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="35222-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="35222-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="35222-105">Une colonne calculée est une colonne dont la valeur est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="35222-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="35222-106">Une colonne calculée peut utiliser d’autres colonnes de la table pour calculer sa valeur.</span><span class="sxs-lookup"><span data-stu-id="35222-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="35222-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="35222-107">Conventions</span></span>

<span data-ttu-id="35222-108">Par Convention, les colonnes calculées ne sont pas créées dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="35222-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="35222-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="35222-109">Data Annotations</span></span>

<span data-ttu-id="35222-110">Les colonnes calculées ne peuvent pas être configurées avec des annotations de données.</span><span class="sxs-lookup"><span data-stu-id="35222-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="35222-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="35222-111">Fluent API</span></span>

<span data-ttu-id="35222-112">Vous pouvez utiliser l’API Fluent pour spécifier qu’une propriété doit être mappée à une colonne calculée.</span><span class="sxs-lookup"><span data-stu-id="35222-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
