---
title: Migrations avec plusieurs projets - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447142"
---
<a name="using-a-separate-project"></a><span data-ttu-id="ae5eb-102">À l’aide d’un projet distinct</span><span class="sxs-lookup"><span data-stu-id="ae5eb-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="ae5eb-103">Vous souhaiterez peut-être stocker vos migrations dans un autre assembly que celui contenant votre `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="ae5eb-104">Vous pouvez également utiliser cette stratégie pour gérer plusieurs ensembles de migrations, par exemple, un pour le développement et un autre pour les mises à niveau de version à l’autre.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="ae5eb-105">Pour...</span><span class="sxs-lookup"><span data-stu-id="ae5eb-105">To do this...</span></span>

1. <span data-ttu-id="ae5eb-106">Créer un nouveau projet de bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-106">Create a new class library.</span></span>

2. <span data-ttu-id="ae5eb-107">Ajoutez une référence à votre assembly de DbContext.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="ae5eb-108">Déplacer les migrations et les fichiers de capture instantanée de modèle vers la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="ae5eb-109">Si vous disposez d’aucun migrations, générer une dans le projet contenant le DbContext, puis déplacez-le.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="ae5eb-110">Ceci est important, car si l’assembly de migrations ne contient pas d’une migration existante, la commande Add-Migration sera impossible de trouver la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="ae5eb-111">Configurer l’assembly de migrations :</span><span class="sxs-lookup"><span data-stu-id="ae5eb-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="ae5eb-112">Ajoutez une référence à votre assembly de migrations à partir de l’assembly de démarrage.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="ae5eb-113">Si cela provoque une dépendance circulaire, mettez à jour le chemin de sortie de la bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="ae5eb-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="ae5eb-114">Si vous avez tout faisiez correctement, il se peut que vous devez être en mesure d’ajouter des migrations de nouveau au projet.</span><span class="sxs-lookup"><span data-stu-id="ae5eb-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
