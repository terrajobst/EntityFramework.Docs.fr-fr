---
title: Colonnes calculées - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993951"
---
# <a name="computed-columns"></a><span data-ttu-id="ed54a-102">Colonnes calculées</span><span class="sxs-lookup"><span data-stu-id="ed54a-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="ed54a-103">La configuration indiquée dans cette section s’applique aux bases de données relationnelles en général.</span><span class="sxs-lookup"><span data-stu-id="ed54a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ed54a-104">Les méthodes d’extension indiquées ici sont disponibles quand vous installez un fournisseur de base de données relationnelle (en raison du package partagé *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="ed54a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ed54a-105">Une colonne calculée est une colonne dont la valeur est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="ed54a-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="ed54a-106">Une colonne calculée peut utiliser d’autres colonnes dans la table pour calculer sa valeur.</span><span class="sxs-lookup"><span data-stu-id="ed54a-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="ed54a-107">Conventions</span><span class="sxs-lookup"><span data-stu-id="ed54a-107">Conventions</span></span>

<span data-ttu-id="ed54a-108">Par convention, les colonnes calculées ne sont pas créés dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="ed54a-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ed54a-109">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="ed54a-109">Data Annotations</span></span>

<span data-ttu-id="ed54a-110">Les colonnes calculées n’ont pas peuvent être configurés avec des Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="ed54a-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ed54a-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="ed54a-111">Fluent API</span></span>

<span data-ttu-id="ed54a-112">Vous pouvez utiliser l’API Fluent pour spécifier qu’une propriété doit correspondre à une colonne calculée.</span><span class="sxs-lookup"><span data-stu-id="ed54a-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
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
