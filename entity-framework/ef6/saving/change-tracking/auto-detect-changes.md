---
title: Détection automatique des modifications-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416976"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="901ab-102">Détection automatique des modifications</span><span class="sxs-lookup"><span data-stu-id="901ab-102">Automatic detect changes</span></span>
<span data-ttu-id="901ab-103">Lors de l’utilisation de la plupart des entités POCO, la détermination de la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) est gérée par l’algorithme de détection des modifications.</span><span class="sxs-lookup"><span data-stu-id="901ab-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="901ab-104">Détecter les modifications fonctionne en détectant les différences entre les valeurs de propriété actuelles de l’entité et les valeurs de propriétés d’origine qui sont stockées dans un instantané lorsque l’entité a été interrogée ou attachée.</span><span class="sxs-lookup"><span data-stu-id="901ab-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="901ab-105">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="901ab-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="901ab-106">Par défaut, Entity Framework effectue la détection automatique des modifications quand les méthodes suivantes sont appelées :</span><span class="sxs-lookup"><span data-stu-id="901ab-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="901ab-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="901ab-107">DbSet.Find</span></span>  
- <span data-ttu-id="901ab-108">DbSet. local</span><span class="sxs-lookup"><span data-stu-id="901ab-108">DbSet.Local</span></span>  
- <span data-ttu-id="901ab-109">DbSet. Add</span><span class="sxs-lookup"><span data-stu-id="901ab-109">DbSet.Add</span></span>  
- <span data-ttu-id="901ab-110">DbSet. AddRange</span><span class="sxs-lookup"><span data-stu-id="901ab-110">DbSet.AddRange</span></span>
- <span data-ttu-id="901ab-111">DbSet. Remove</span><span class="sxs-lookup"><span data-stu-id="901ab-111">DbSet.Remove</span></span>  
- <span data-ttu-id="901ab-112">DbSet. RemoveRange</span><span class="sxs-lookup"><span data-stu-id="901ab-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="901ab-113">DbSet. Attach</span><span class="sxs-lookup"><span data-stu-id="901ab-113">DbSet.Attach</span></span>  
- <span data-ttu-id="901ab-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="901ab-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="901ab-115">DbContext. GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="901ab-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="901ab-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="901ab-116">DbContext.Entry</span></span>  
- <span data-ttu-id="901ab-117">DbChangeTracker. entrées</span><span class="sxs-lookup"><span data-stu-id="901ab-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="901ab-118">Désactivation de la détection automatique des modifications</span><span class="sxs-lookup"><span data-stu-id="901ab-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="901ab-119">Si vous effectuez le suivi d’un grand nombre d’entités dans votre contexte et que vous appelez l’une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives en matière de performances en désactivant la détection des modifications pour la durée de la boucle.</span><span class="sxs-lookup"><span data-stu-id="901ab-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="901ab-120">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="901ab-120">For example:</span></span>  

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

<span data-ttu-id="901ab-121">N’oubliez pas de réactiver la détection des modifications après la boucle. nous avons utilisé un try/finally pour vous assurer qu’il est toujours réactivé, même si le code de la boucle lève une exception.</span><span class="sxs-lookup"><span data-stu-id="901ab-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="901ab-122">Une alternative à la désactivation et à la réactivation consiste à maintenir la détection automatique des modifications désactivées à tout moment et dans le contexte de l’appel. ChangeTracker. DetectChanges explicitement ou utilisez les proxys de suivi des modifications avec soin.</span><span class="sxs-lookup"><span data-stu-id="901ab-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="901ab-123">Ces deux options sont avancées et peuvent facilement introduire des bogues subtils dans votre application et les utiliser avec précaution.</span><span class="sxs-lookup"><span data-stu-id="901ab-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="901ab-124">Si vous devez ajouter ou supprimer un grand nombre d’objets d’un contexte, envisagez d’utiliser DbSet. AddRange et DbSet. RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="901ab-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="901ab-125">Ces méthodes détectent automatiquement les modifications une seule fois après l’exécution des opérations d’ajout ou de suppression.</span><span class="sxs-lookup"><span data-stu-id="901ab-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
