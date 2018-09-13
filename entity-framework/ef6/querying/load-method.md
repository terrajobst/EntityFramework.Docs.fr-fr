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
# <a name="the-load-method"></a>Méthode Load
Il existe plusieurs scénarios où vous souhaitez charger des entités à partir de la base de données dans le contexte sans immédiatement sert à rien avec ces entités. Un bon exemple est chargement d’entités pour la liaison de données comme décrit dans [données locales](~/ef6/querying/local-data.md). Une méthode courante pour ce faire consiste à écrire une requête LINQ, puis appelez ToList dessus, uniquement pour ignorer la liste créée. La méthode d’extension Load fonctionne exactement comme ToList, sauf qu’elle évite la création de la liste complètement.  

Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

Voici deux exemples d’utilisation de charge. La première est effectuée à partir d’une application Windows Forms de liaison de données où la charge est utilisé pour interroger des entités avant la liaison à la collection locale, comme décrit dans [données locales](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Le deuxième exemple s’affiche à l’aide de charge pour charger une collection filtrée des entités associées, comme décrit dans [le chargement des entités associées](~/ef6/querying/related-data.md):  

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
