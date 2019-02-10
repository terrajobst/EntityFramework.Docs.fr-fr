---
title: Journalisation - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985572"
---
# <a name="logging"></a>Journalisation

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.

## <a name="aspnet-core-applications"></a>Applications ASP.NET Core

EF Core s’intègre automatiquement avec les mécanismes de journalisation d’ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé. Par conséquent, lorsque vous utilisez ASP.NET Core, journalisation doit être configurée comme décrit dans la [documentation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Autres applications

EF Core journalisation actuellement nécessite un ILoggerFactory qui est lui-même configuré avec un ou plusieurs ILoggerProvider. Fournisseurs courants sont fournis dans les packages suivants :

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un journal de console simple.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Prend en charge les journaux de diagnostic des Services d’application Azure et les fonctionnalités de « Log stream ».
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Journaux pour une analyse de débogueur à l’aide de System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Journaux pour le journal des événements Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Prend en charge/EventListener EventSource.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Journaux pour un écouteur de suivi à l’aide de System.Diagnostics.TraceSource.TraceEvent().

> [!NOTE]
> Le code suivant exemple utilise un `ConsoleLoggerProvider` constructeur qui a été obsolète dans la version 2.2. Remplacements appropriés pour les API de journalisation obsolète seront disponibles dans la version 3.0. En attendant, il est possible sans ignorer et supprimer les avertissements.

Après avoir installé les packages appropriés, l’application doit créer une instance de singleton/globale d’un LoggerFactory. Par exemple, utilisez le journal de console :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Cette instance de singleton/globale doit être inscrits avec EF Core sur le `DbContextOptionsBuilder`. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Il est très important que les applications ne créent pas une nouvelle instance de ILoggerFactory pour chaque instance de contexte. Cela entraîne une fuite de mémoire et des performances médiocres.

## <a name="filtering-what-is-logged"></a>Filtrage de ce qui est enregistré

> [!NOTE]
> Le code suivant exemple utilise un `ConsoleLoggerProvider` constructeur qui a été obsolète dans la version 2.2. Remplacements appropriés pour les API de journalisation obsolète seront disponibles dans la version 3.0. En attendant, il est possible sans ignorer et supprimer les avertissements.

Pour filtrer ce qui est enregistré, le plus simple consiste à configurer lors de l’inscription de l’ILoggerProvider. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Dans cet exemple, le journal est filtré pour retourner uniquement les messages :
 * dans la catégorie « Microsoft.EntityFrameworkCore.Database.Command »
 * au niveau des « Informations »

Pour EF Core, les catégories de l’enregistreur d’événements sont définies dans le `DbLoggerCategory` résoudre les classe pour le rendre plus faciles à trouver les catégories, mais ces chaînes simples.

Vous trouverez plus d’informations sur l’infrastructure sous-jacente de la journalisation dans le [documentation sur la journalisation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
