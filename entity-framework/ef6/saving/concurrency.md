---
title: Gestion des conflits d’accès concurrentiel - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489152"
---
# <a name="handling-concurrency-conflicts"></a>Gestion de conflits d'accès concurrentiel
Tentative d’enregistrement de votre entité dans la base de données dans l’espoir que les données n’a pas changé depuis l’entité a été chargée optimiste implique l’accès concurrentiel optimiste. S’il s’avère que les données ont changé, une exception est levée et vous devez résoudre le conflit avant d’enregistrer à nouveau. Cette rubrique explique comment gérer ces exceptions dans Entity Framework. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

Cette publication n’est pas l’emplacement approprié pour obtenir une présentation complète de l’accès concurrentiel optimiste. Les sections ci-dessous supposent une connaissance de la résolution d’accès concurrentiel et affichent des modèles pour les tâches courantes.  

Bon nombre de ces modèles font utilisent les sujets abordés dans [travaillez avec des valeurs de propriété](~/ef6/saving/change-tracking/property-values.md).  

Résolution des problèmes d’accès concurrentiel lorsque vous utilisez des associations indépendantes (où la clé étrangère n'est pas mappée à une propriété de votre entité) est beaucoup plus difficile que lorsque vous utilisez des associations de clé étrangère. Par conséquent, si vous vous apprêtez à effectuer la résolution d’accès concurrentiel dans votre application, il est conseillé de toujours correspondre les clés étrangères dans vos entités. Tous les exemples ci-dessous partent du principe que vous utilisez des associations de clé étrangère.  

Une DbUpdateConcurrencyException est levée par SaveChanges lorsqu’une exception d’accès concurrentiel optimiste est détectée lors de la tentative d’enregistrement d’une entité qui utilise les associations de clé étrangère.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Résolution des exceptions d’accès concurrentiel optimiste avec recharger (wins de la base de données)  

La méthode Reload peut être utilisée pour remplacer les valeurs actuelles de l’entité avec les valeurs maintenant dans la base de données. L’entité est ensuite généralement envoyée à l’utilisateur dans une certaine forme et ils doivent essayer de le rendre de nouveau leurs modifications et enregistrez à nouveau. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Un bon moyen de simuler une exception d’accès concurrentiel consiste à définir un point d’arrêt sur l’appel de SaveChanges, puis modifiez une entité qui est enregistrée dans la base de données à l’aide d’un autre outil tel que SQL Management Studio. Vous pouvez également insérer une ligne avant SaveChanges pour mettre à jour de la base de données directement à l’aide de SqlCommand. Exemple :  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

La méthode entrées sur DbUpdateConcurrencyException retourne les instances DbEntityEntry pour les entités qui n’a pas pu mettre à jour. (Cette propriété actuellement retourne toujours une valeur unique pour les problèmes d’accès concurrentiel. Elle peut retourner plusieurs valeurs pour les exceptions de mise à jour générale.) Une alternative dans certains cas peut consister à obtenir les entrées pour toutes les entités devant être rechargées à partir de la base de données et appel recharger pour chacun d’eux.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Résolution des exceptions d’accès concurrentiel optimiste en tant que priorité au client  

L’exemple ci-dessus qui utilise le rechargement est parfois appelé wins de la base de données ou priorité au magasin, car les valeurs de l’entité sont remplacées par les valeurs de la base de données. Vous pouvez parfois souhaiter l’inverse, remplacer les valeurs dans la base de données avec les valeurs actuellement dans l’entité. Cela est parfois appelé priorité au client et peut être effectuée en obtenant les valeurs actuelles de la base de données et de les définir en tant que les valeurs d’origine pour l’entité. (Consultez [travaillez avec des valeurs de propriété](~/ef6/saving/change-tracking/property-values.md) pour plus d’informations sur les valeurs actuelles et d’origine.) Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Résolution personnalisée des exceptions d’accès concurrentiel optimiste  

Vous pouvez être amené à combiner les valeurs actuellement dans la base de données avec les valeurs actuellement dans l’entité. Cela nécessite généralement une interaction utilisateur ou de la logique personnalisée. Par exemple, vous pouvez présenter un formulaire à l’utilisateur contenant les valeurs actuelles, les valeurs dans la base de données, et une valeur par défaut définie de valeurs résolues. L’utilisateur est ensuite modifier les valeurs résolues en fonction des besoins, et il serait ces valeurs résolues enregistrées dans la base de données. Cela est possible en utilisant les objets DbPropertyValues retourné par CurrentValues et GetDatabaseValues sur l’entrée de l’entité. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Résolution personnalisée des exceptions d’accès concurrentiel optimiste à l’aide d’objets  

Le code ci-dessus utilise des instances de DbPropertyValues pour passer des cours, base de données et valeurs résolues. Parfois, il peut être plus facile à utiliser des instances de votre type d’entité pour cela. Cela est possible à l’aide de méthodes ToObject et SetValues de DbPropertyValues. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
