---
title: Migrations avec plusieurs projets - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997985"
---
<a name="using-a-separate-project"></a>À l’aide d’un projet distinct
========================
Vous souhaiterez peut-être stocker vos migrations dans un autre assembly que celui contenant votre `DbContext`. Vous pouvez également utiliser cette stratégie pour gérer plusieurs ensembles de migrations, par exemple, un pour le développement et un autre pour les mises à niveau de version à l’autre.

Pour...

1. Créer un nouveau projet de bibliothèque de classes.

2. Ajoutez une référence à votre assembly de DbContext.

3. Déplacer les migrations et les fichiers de capture instantanée de modèle vers la bibliothèque de classes.
   > [!TIP]
   > Si vous disposez d’aucun migrations, générer une dans le projet contenant le DbContext, puis déplacez-le. Ceci est important, car si l’assembly de migrations ne contient pas d’une migration existante, la commande Add-Migration sera impossible de trouver la classe DbContext.

4. Configurer l’assembly de migrations :

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Ajoutez une référence à votre assembly de migrations à partir de l’assembly de démarrage.
   * Si cela provoque une dépendance circulaire, mettez à jour le chemin de sortie de la bibliothèque de classes :

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si vous avez tout faisiez correctement, il se peut que vous devez être en mesure d’ajouter des migrations de nouveau au projet.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
