---
title: EF6 et Core EF - Utilisation conjointe dans la même application
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054823"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="e85b9-102">Utilisation conjointe d’EF Core et d’EF6 dans la même application</span><span class="sxs-lookup"><span data-stu-id="e85b9-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="e85b9-103">Il est possible d’utiliser EF Core et EF6 dans la même bibliothèque ou application .NET Framework en installant les deux packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="e85b9-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="e85b9-104">Certains types portent les mêmes noms dans EF Core et EF6 et se distinguent uniquement par l’espace de noms, ce qui peut compliquer l’utilisation conjointe d’EF Core et d’EF6 dans le même fichier de code.</span><span class="sxs-lookup"><span data-stu-id="e85b9-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="e85b9-105">L’ambiguïté peut être facilement supprimée en utilisant des directives d’alias d’espace de noms, par exemple :</span><span class="sxs-lookup"><span data-stu-id="e85b9-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="e85b9-106">Si vous déplacez une application existante qui possède plusieurs modèles EF, vous pouvez, si vous le souhaitez, déplacer spécifiquement certains d'entre eux dans EF Core et continuer à utiliser EF6 pour les autres.</span><span class="sxs-lookup"><span data-stu-id="e85b9-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
