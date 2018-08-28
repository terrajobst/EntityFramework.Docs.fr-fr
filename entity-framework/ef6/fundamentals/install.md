---
title: Obtenir Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 78ef1e7b20bd879c972870552c8f692e153b7abb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996563"
---
# <a name="get-entity-framework"></a>Obtenir Entity Framework
Entity Framework est constitué par les outils Entity Framework pour Visual Studio et le Runtime Entity Framework.

## <a name="ef-tools-for-visual-studio"></a>Outils Entity Framework pour Visual Studio

Entity Framework Tools pour Visual Studio incluent le Concepteur EF et l’Assistant de modèle EF et sont nécessaires pour la base de données tout d’abord et premier flux de travail de modèle. Outils Entity Framework sont inclus dans toutes les versions récentes de Visual Studio. Si vous effectuez une installation personnalisée de Visual Studio, vous devez vous assurer que l’élément « Entity Framework 6 Tools » est sélectionné par en choisissant une charge de travail qui l’inclut ou en le sélectionnant de manière individuelle.

Pour d’anciennes versions de Visual Studio, les outils EF mis à jour sont disponibles en téléchargement. Consultez [Versions de Visual Studio](~/ef6/what-is-new/visual-studio.md) pour obtenir des conseils sur la façon d’obtenir la dernière version des outils Entity Framework disponibles pour votre version de Visual Studio.

## <a name="ef-runtime"></a>Runtime EF

La dernière version d’Entity Framework est disponible en tant que le [package EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Si vous n’êtes pas familiarisé avec le Gestionnaire de Package NuGet, nous vous encourageons à lire le [vue d’ensemble de NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Installation du Package de NuGet EF

Vous pouvez installer le package EntityFramework en cliquant sur le **références** dossier de votre projet et en sélectionnant **gérer les Packages NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Installation à partir de la Console du Gestionnaire de Package

Vous pouvez également installer EntityFramework en exécutant la commande suivante dans le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Installation d’une version spécifique d’EF

À partir d’Entity Framework 4.1 et versions ultérieures, les nouvelles versions du runtime EF ont été publiées en tant que le [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/). Une de ces versions peuvent être ajoutée à un projet basé sur le .NET Framework en exécutant la commande suivante dans Visual Studio [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Notez que `<number>` représente la version spécifique d’EF à installer. Par exemple, 6.2.0 est la version du nombre de EF 6.2.   

Runtimes d’EF avant 4.1 faisaient partie du .NET Framework et ne peut pas être installé séparément.

### <a name="installing-the-latest-preview"></a>L’installation de la dernière version préliminaire

Les méthodes ci-dessus vous donnera la dernière version entièrement prise en charge de la version d’Entity Framework. Il existe souvent des versions préliminaires d’Entity Framework disponibles que nous aimerions vous permettent de tester et envoyez-nous vos commentaires sur.

Pour installer la dernière version préliminaire d’Entity Framework vous pouvez sélectionner **inclure la version préliminaire** dans la fenêtre Gérer les Packages NuGet. Si aucune version préliminaire n’est disponibles vous obtenez automatiquement la dernière version entièrement prise en charge d’Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Ou bien, vous pouvez exécuter la commande suivante le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
