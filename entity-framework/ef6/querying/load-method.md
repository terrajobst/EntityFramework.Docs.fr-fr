---
title: La méthode Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490894"
---
# <a name="the-load-method"></a><span data-ttu-id="ca485-102">Méthode Load</span><span class="sxs-lookup"><span data-stu-id="ca485-102">The Load Method</span></span>
<span data-ttu-id="ca485-103">Il existe plusieurs scénarios où vous souhaitez charger des entités à partir de la base de données dans le contexte sans immédiatement sert à rien avec ces entités.</span><span class="sxs-lookup"><span data-stu-id="ca485-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="ca485-104">Un bon exemple est chargement d’entités pour la liaison de données comme décrit dans [données locales](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="ca485-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="ca485-105">Une méthode courante pour ce faire consiste à écrire une requête LINQ, puis appelez ToList dessus, uniquement pour ignorer la liste créée.</span><span class="sxs-lookup"><span data-stu-id="ca485-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="ca485-106">La méthode d’extension Load fonctionne exactement comme ToList, sauf qu’elle évite la création de la liste complètement.</span><span class="sxs-lookup"><span data-stu-id="ca485-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="ca485-107">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ca485-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="ca485-108">Voici deux exemples d’utilisation de charge.</span><span class="sxs-lookup"><span data-stu-id="ca485-108">Here are two examples of using Load.</span></span> <span data-ttu-id="ca485-109">La première est effectuée à partir d’une application Windows Forms de liaison de données où la charge est utilisé pour interroger des entités avant la liaison à la collection locale, comme décrit dans [données locales](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="ca485-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="ca485-110">Le deuxième exemple s’affiche à l’aide de charge pour charger une collection filtrée des entités associées, comme décrit dans [le chargement des entités associées](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="ca485-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
