---
title: EF6 et Core EF - Utilisation conjointe dans la même application
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995707"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Utilisation conjointe d’EF Core et d’EF6 dans la même application

Il est possible d’utiliser EF Core et EF6 dans la même bibliothèque ou application .NET Framework en installant les deux packages NuGet.

Certains types portent les mêmes noms dans EF Core et EF6 et se distinguent uniquement par l’espace de noms, ce qui peut compliquer l’utilisation conjointe d’EF Core et d’EF6 dans le même fichier de code. L’ambiguïté peut être facilement supprimée en utilisant des directives d’alias d’espace de noms. Exemple :

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Si vous déplacez une application existante qui possède plusieurs modèles EF, vous pouvez, si vous le souhaitez, déplacer spécifiquement certains d'entre eux dans EF Core et continuer à utiliser EF6 pour les autres.
