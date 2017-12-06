---
title: Enregistrement - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>Enregistrement

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.

## <a name="aspnet-core-applications"></a>Applications ASP.NET Core

EF Core intègre automatiquement avec les mécanismes de journalisation sans de ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé. Par conséquent, lorsque vous utilisez ASP.NET Core, la journalisation doit être configurée comme décrit dans la [documentation d’ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Autres applications

Core EF journalisation actuellement requiert un ILoggerFactory qui est lui-même configuré avec un ou plusieurs ILoggerProvider. Fournisseurs courants sont inclus dans les packages suivants :

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un journal de console simple.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Services d’application Azure prend en charge les journaux de diagnostic et les fonctionnalités de « Log flux ».
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): journaux à une analyse de débogueur à l’aide de System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): les journaux pour le journal des événements Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): prend en charge EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): journaux pour un écouteur de trace à l’aide de System.Diagnostics.TraceSource.TraceEvent().

Après avoir installé l’ou les modules approprié, l’application doit créer une instance de singleton/globale d’un LoggerFactory. Par exemple, à l’aide de l’enregistreur d’événements de la console :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Cette instance de singleton/global doit être inscrits avec EF de base sur la `DbContextOptionsBuilder`. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Il est très important que les applications ne créent pas une nouvelle instance de ILoggerFactory pour chaque instance de contexte. Cela entraîne une fuite de mémoire et de performances médiocres.

## <a name="filtering-what-is-logged"></a>Filtrage des éléments enregistrés

Pour filtrer les éléments enregistrés, le plus simple consiste à configurer lors de l’inscription de l’ILoggerProvider. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Dans cet exemple, le journal est filtré pour retourner uniquement les messages :
 * dans la catégorie 'Microsoft.EntityFrameworkCore.Database.Command'
 * au niveau « Informations »

Pour EF Core, les catégories de l’enregistreur d’événements sont définies dans le `DbLoggerCategory` classe pour faciliter le travail rechercher des catégories, mais ces résoudre en chaînes simples.

Vous trouverez plus d’informations sur l’infrastructure sous-jacente de la journalisation dans le [documentation de journalisation ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).