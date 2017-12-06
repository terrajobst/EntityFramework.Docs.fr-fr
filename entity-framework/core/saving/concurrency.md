---
title: "La gestion des accès concurrentiel - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a><span data-ttu-id="b4d79-102">Gestion de l'accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="b4d79-102">Handling Concurrency</span></span>

<span data-ttu-id="b4d79-103">Si une propriété est configurée comme un jeton d’accès concurrentiel EF Vérifiez qu’aucun autre utilisateur n’a modifié cette valeur dans la base de données lors de l’enregistrement des modifications apportées à cet enregistrement.</span><span class="sxs-lookup"><span data-stu-id="b4d79-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span>

> [!TIP]  
> <span data-ttu-id="b4d79-104">Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="b4d79-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

## <a name="how-concurrency-handling-works-in-ef-core"></a><span data-ttu-id="b4d79-105">Fonctionne de la gestion d’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="b4d79-105">How concurrency handling works in EF Core</span></span>

<span data-ttu-id="b4d79-106">Pour obtenir une description détaillée du fonctionne de la gestion d’accès concurrentiel dans Entity Framework Core, consultez [jetons d’accès concurrentiel](../modeling/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="b4d79-106">For a detailed description of how concurrency handling works in Entity Framework Core, see [Concurrency Tokens](../modeling/concurrency.md).</span></span>

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="b4d79-107">Résolution des conflits d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="b4d79-107">Resolving concurrency conflicts</span></span>

<span data-ttu-id="b4d79-108">Résoudre un conflit d’accès concurrentiel implique l’utilisation d’un algorithme pour fusionner les modifications en attente à partir de l’utilisateur actuel avec les modifications apportées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b4d79-108">Resolving a concurrency conflict involves using an algorithm to merge the pending changes from the current user with the changes made in the database.</span></span> <span data-ttu-id="b4d79-109">L’approche exacte peut varier en fonction de votre application, mais une approche courante consiste à afficher les valeurs à l’utilisateur et demandez-lui de déterminer les valeurs correctes à stocker dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b4d79-109">The exact approach will vary based on your application, but a common approach is to display the values to the user and have them decide the correct values to be stored in the database.</span></span>

<span data-ttu-id="b4d79-110">**Il existe trois ensembles de valeurs disponibles pour aider à résoudre un conflit d’accès concurrentiel.**</span><span class="sxs-lookup"><span data-stu-id="b4d79-110">**There are three sets of values available to help resolve a concurrency conflict.**</span></span>

* <span data-ttu-id="b4d79-111">**Valeurs actuelles** sont les valeurs que l’application a tenté d’écrire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b4d79-111">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="b4d79-112">**Les valeurs d’origine** sont les valeurs qui ont été récupérées à l’origine à partir de la base de données, avant que toutes les modifications ont été apportées.</span><span class="sxs-lookup"><span data-stu-id="b4d79-112">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="b4d79-113">**Valeurs de base de données** sont les valeurs actuellement stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b4d79-113">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="b4d79-114">Pour gérer un conflit d’accès concurrentiel, intercepter un `DbUpdateConcurrencyException` pendant `SaveChanges()`, utilisez `DbUpdateConcurrencyException.Entries` pour préparer un nouvel ensemble de modifications pour les entités concernées, puis réessayez la `SaveChanges()` opération.</span><span class="sxs-lookup"><span data-stu-id="b4d79-114">To handle a concurrency conflict, catch a `DbUpdateConcurrencyException` during `SaveChanges()`, use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities, and then retry the `SaveChanges()` operation.</span></span>

<span data-ttu-id="b4d79-115">Dans l’exemple suivant, `Person.FirstName` et `Person.LastName` sont configurés en tant que jeton d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="b4d79-115">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency token.</span></span> <span data-ttu-id="b4d79-116">Il existe un `// TODO:` commentaire à l’emplacement où vous devez inclure la logique spécifique de l’application pour choisir la valeur à enregistrer dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b4d79-116">There is a `// TODO:` comment in the location where you would include application specific logic to choose the value to be saved to the database.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
