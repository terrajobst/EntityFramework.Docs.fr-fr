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
# <a name="automatic-detect-changes"></a><span data-ttu-id="f7dd6-102">Détecter des modifications automatique</span><span class="sxs-lookup"><span data-stu-id="f7dd6-102">Automatic detect changes</span></span>
<span data-ttu-id="f7dd6-103">Lors de l’utilisation de la plupart des entités POCO la détermination de la manière dont une entité a changé (et par conséquent les mises à jour doivent être envoyées à la base de données) est gérée par l’algorithme de détecter les modifications.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="f7dd6-104">Détecter works de modifications en détectant les différences entre les valeurs de propriété actuelles de l’entité et les valeurs de propriété d’origine qui sont stockés dans un instantané lorsque l’entité a été interrogée ou attachée.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="f7dd6-105">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f7dd6-106">Par défaut, Entity Framework effectue automatiquement les modifications de détecter lorsque les méthodes suivantes sont appelées :</span><span class="sxs-lookup"><span data-stu-id="f7dd6-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="f7dd6-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="f7dd6-107">DbSet.Find</span></span>  
- <span data-ttu-id="f7dd6-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="f7dd6-108">DbSet.Local</span></span>  
- <span data-ttu-id="f7dd6-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="f7dd6-109">DbSet.Add</span></span>  
- <span data-ttu-id="f7dd6-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="f7dd6-110">DbSet.AddRange</span></span>
- <span data-ttu-id="f7dd6-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="f7dd6-111">DbSet.Remove</span></span>  
- <span data-ttu-id="f7dd6-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="f7dd6-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="f7dd6-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="f7dd6-113">DbSet.Attach</span></span>  
- <span data-ttu-id="f7dd6-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f7dd6-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="f7dd6-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="f7dd6-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="f7dd6-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="f7dd6-116">DbContext.Entry</span></span>  
- <span data-ttu-id="f7dd6-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="f7dd6-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="f7dd6-118">La désactivation de la détection automatique des modifications</span><span class="sxs-lookup"><span data-stu-id="f7dd6-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="f7dd6-119">Si vous suivez un grand nombre d’entités dans votre contexte et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives des performances en désactivant la détection des modifications apportées pendant la durée de la boucle.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="f7dd6-120">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f7dd6-120">For example:</span></span>  

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

<span data-ttu-id="f7dd6-121">N’oubliez pas de réactiver la détection des modifications apportées après la boucle, nous avons utilisé un bloc try/finally pour vous assurer qu’il est toujours réactivé même si le code dans la boucle lève une exception.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="f7dd6-122">Une alternative à la désactivation et réactivation de l’est de laisser la détection automatique des modifications mis hors tension à toutes les heures et un contexte d’appel. ChangeTracker.DetectChanges explicitement ou utilisez l’assidûment les proxys de suivi.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="f7dd6-123">Ces deux options avancées et peut facilement introduire des bogues subtils dans votre application donc de les utiliser avec précaution.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="f7dd6-124">Si vous avez besoin ajouter ou supprimer un grand nombre d’objets à partir d’un contexte, envisagez d’utiliser DbSet.AddRange et DbSet.RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="f7dd6-125">Cette méthode détecte automatiquement les modifications qu’une seule fois après avoir effectué les opérations d’ajout / suppression.</span><span class="sxs-lookup"><span data-stu-id="f7dd6-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
