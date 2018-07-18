---
title: Automatique détecter les modifications - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
caps.latest.revision: 3
ms.openlocfilehash: 62f2f026426346fc1230a2f5743c8cb7d232ec7f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120779"
---
# <a name="automatic-detect-changes"></a>Détecter des modifications automatique
Lors de l’utilisation de la plupart des entités POCO la détermination de la manière dont une entité a changé (et par conséquent les mises à jour doivent être envoyées à la base de données) est gérée par l’algorithme de détecter les modifications. Détecter works de modifications en détectant les différences entre les valeurs de propriété actuelles de l’entité et les valeurs de propriété d’origine qui sont stockés dans un instantané lorsque l’entité a été interrogée ou attachée. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

Par défaut, Entity Framework effectue automatiquement les modifications de détecter lorsque les méthodes suivantes sont appelées :  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>La désactivation de la détection automatique des modifications  

Si vous suivez un grand nombre d’entités dans votre contexte et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives des performances en désactivant la détection des modifications apportées pendant la durée de la boucle. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

N’oubliez pas de réactiver la détection des modifications apportées après la boucle, nous avons utilisé un bloc try/finally pour vous assurer qu’il est toujours réactivé même si le code dans la boucle lève une exception.  

Une alternative à la désactivation et réactivation de l’est de laisser la détection automatique des modifications mis hors tension à toutes les heures et un contexte d’appel. ChangeTracker.DetectChanges explicitement ou utilisez l’assidûment les proxys de suivi. Ces deux options avancées et peut facilement introduire des bogues subtils dans votre application donc de les utiliser avec précaution.  

Si vous avez besoin ajouter ou supprimer un grand nombre d’objets à partir d’un contexte, envisagez d’utiliser DbSet.AddRange et DbSet.RemoveRange. Cette méthode détecte automatiquement les modifications qu’une seule fois après avoir effectué les opérations d’ajout / suppression. 
