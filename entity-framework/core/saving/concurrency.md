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
# <a name="handling-concurrency"></a>Gestion de l'accès concurrentiel

Si une propriété est configurée comme un jeton d’accès concurrentiel EF Vérifiez qu’aucun autre utilisateur n’a modifié cette valeur dans la base de données lors de l’enregistrement des modifications apportées à cet enregistrement.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) sur GitHub.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Fonctionne de la gestion d’accès concurrentiel dans EF Core

Pour obtenir une description détaillée du fonctionne de la gestion d’accès concurrentiel dans Entity Framework Core, consultez [jetons d’accès concurrentiel](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Résolution des conflits d’accès concurrentiel

Résoudre un conflit d’accès concurrentiel implique l’utilisation d’un algorithme pour fusionner les modifications en attente à partir de l’utilisateur actuel avec les modifications apportées dans la base de données. L’approche exacte peut varier en fonction de votre application, mais une approche courante consiste à afficher les valeurs à l’utilisateur et demandez-lui de déterminer les valeurs correctes à stocker dans la base de données.

**Il existe trois ensembles de valeurs disponibles pour aider à résoudre un conflit d’accès concurrentiel.**

* **Valeurs actuelles** sont les valeurs que l’application a tenté d’écrire dans la base de données.

* **Les valeurs d’origine** sont les valeurs qui ont été récupérées à l’origine à partir de la base de données, avant que toutes les modifications ont été apportées.

* **Valeurs de base de données** sont les valeurs actuellement stockées dans la base de données.

Pour gérer un conflit d’accès concurrentiel, intercepter un `DbUpdateConcurrencyException` pendant `SaveChanges()`, utilisez `DbUpdateConcurrencyException.Entries` pour préparer un nouvel ensemble de modifications pour les entités concernées, puis réessayez la `SaveChanges()` opération.

Dans l’exemple suivant, `Person.FirstName` et `Person.LastName` sont configurés en tant que jeton d’accès concurrentiel. Il existe un `// TODO:` commentaire à l’emplacement où vous devez inclure la logique spécifique de l’application pour choisir la valeur à enregistrer dans la base de données.

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
