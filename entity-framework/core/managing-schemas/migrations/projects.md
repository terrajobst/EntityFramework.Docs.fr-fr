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
# <a name="using-a-separate-migrations-project"></a>Utilisation d’un projet de migrations distinct

Vous souhaiterez peut-être stocker vos migrations dans un autre assembly que celui contenant votre `DbContext`. Vous pouvez également utiliser cette stratégie pour gérer plusieurs ensembles de migrations, par exemple, un pour le développement et un autre pour les mises à niveau vers la version finale.

Pour...

1. Créer un nouveau projet de bibliothèque de classes.

2. Ajoutez une référence à votre assembly DbContext.

3. Déplacez les fichiers de migration et d’instantané de modèle dans la bibliothèque de classes.
   > [!TIP]
   > Si vous n’avez pas de migrations existantes, générez-en une dans le projet contenant le DbContext, puis déplacez-le.
   > Cela est important car si l’assembly de migrations ne contient pas de migration existante, la commande Add-migration ne pourra pas trouver DbContext.

4. Configurez l’assembly des migrations :

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Ajoutez une référence à votre assembly de migration à partir de l’assembly de démarrage.
   * Si cela provoque une dépendance circulaire, mettez à jour le chemin de sortie de la bibliothèque de classes :

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si vous avez tout effectué correctement, vous devez être en mesure d’ajouter de nouvelles migrations au projet.

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
