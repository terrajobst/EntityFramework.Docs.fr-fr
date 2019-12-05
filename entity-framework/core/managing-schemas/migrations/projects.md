---
title: Utilisation d’un projet de migrations distinct-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824414"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="dbfa3-102">Utilisation d’un projet de migrations distinct</span><span class="sxs-lookup"><span data-stu-id="dbfa3-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="dbfa3-103">Vous souhaiterez peut-être stocker vos migrations dans un autre assembly que celui contenant votre `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="dbfa3-104">Vous pouvez également utiliser cette stratégie pour gérer plusieurs ensembles de migrations, par exemple, un pour le développement et un autre pour les mises à niveau vers la version finale.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="dbfa3-105">Pour...</span><span class="sxs-lookup"><span data-stu-id="dbfa3-105">To do this...</span></span>

1. <span data-ttu-id="dbfa3-106">Créer un nouveau projet de bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-106">Create a new class library.</span></span>

2. <span data-ttu-id="dbfa3-107">Ajoutez une référence à votre assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="dbfa3-108">Déplacez les fichiers de migration et d’instantané de modèle dans la bibliothèque de classes.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="dbfa3-109">Si vous n’avez pas de migrations existantes, générez-en une dans le projet contenant le DbContext, puis déplacez-le.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="dbfa3-110">Cela est important car si l’assembly de migrations ne contient pas de migration existante, la commande Add-migration ne pourra pas trouver DbContext.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="dbfa3-111">Configurez l’assembly des migrations :</span><span class="sxs-lookup"><span data-stu-id="dbfa3-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="dbfa3-112">Ajoutez une référence à votre assembly de migration à partir de l’assembly de démarrage.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="dbfa3-113">Si cela provoque une dépendance circulaire, mettez à jour le chemin de sortie de la bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="dbfa3-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="dbfa3-114">Si vous avez tout effectué correctement, vous devez être en mesure d’ajouter de nouvelles migrations au projet.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="dbfa3-115">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="dbfa3-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="dbfa3-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dbfa3-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
