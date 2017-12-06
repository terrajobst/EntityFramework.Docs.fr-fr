---
title: Migrations avec plusieurs projets - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a>À l’aide d’un projet distinct
========================
Vous souhaitez peut-être stocker vos migrations dans un autre assembly que celui qui contient votre `DbContext`. Vous pouvez également utiliser cette stratégie pour conserver plusieurs jeux de migrations, par exemple, une pour le développement et l’autre des mises à niveau de version à l’autre.

Pour...

1. Créer un nouveau projet de bibliothèque de classes.

2. Ajoutez une référence à votre assembly de DbContext.

3. Déplacer les migrations et les fichiers de capture instantanée de modèle vers la bibliothèque de classes.
   * Si vous n’avez pas ajouté, ajoutez l’un pour le projet de DbContext, puis déplacez-le.

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
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si vous avez tout faisiez correctement, vous devez être en mesure d’ajouter de nouvelles migrations au projet.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
