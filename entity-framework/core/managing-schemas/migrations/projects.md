---
title: Migrations avec plusieurs projets - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161225"
---
<a name="using-a-separate-project"></a><span data-ttu-id="225de-102">À l’aide d’un projet distinct</span><span class="sxs-lookup"><span data-stu-id="225de-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="225de-103">Vous souhaitez peut-être stocker vos migrations dans un autre assembly que celui qui contient votre `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="225de-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="225de-104">Vous pouvez également utiliser cette stratégie pour conserver plusieurs jeux de migrations, par exemple, une pour le développement et l’autre des mises à niveau de version à l’autre.</span><span class="sxs-lookup"><span data-stu-id="225de-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="225de-105">Pour...</span><span class="sxs-lookup"><span data-stu-id="225de-105">To do this...</span></span>

1. <span data-ttu-id="225de-106">Créer un nouveau projet de bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="225de-106">Create a new class library.</span></span>

2. <span data-ttu-id="225de-107">Ajoutez une référence à votre assembly de DbContext.</span><span class="sxs-lookup"><span data-stu-id="225de-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="225de-108">Déplacer les migrations et les fichiers de capture instantanée de modèle vers la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="225de-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="225de-109">Si vous n’avez pas ajouté, ajoutez l’un pour le projet de DbContext, puis déplacez-le.</span><span class="sxs-lookup"><span data-stu-id="225de-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="225de-110">Configurer l’assembly de migrations :</span><span class="sxs-lookup"><span data-stu-id="225de-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="225de-111">Ajoutez une référence à votre assembly de migrations à partir de l’assembly de démarrage.</span><span class="sxs-lookup"><span data-stu-id="225de-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="225de-112">Si cela provoque une dépendance circulaire, mettez à jour le chemin de sortie de la bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="225de-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="225de-113">Si vous avez tout faisiez correctement, vous devez être en mesure d’ajouter de nouvelles migrations au projet.</span><span class="sxs-lookup"><span data-stu-id="225de-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
