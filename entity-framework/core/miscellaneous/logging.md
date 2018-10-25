---
title: Journalisation - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022143"
---
# <a name="logging"></a>Journalisation

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.

## <a name="aspnet-core-applications"></a>Applications ASP.NET Core

EF Core s’intègre automatiquement avec les mécanismes de journalisation d’ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé. Par conséquent, lorsque vous utilisez ASP.NET Core, journalisation doit être configurée comme décrit dans la [documentation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Autres applications

EF Core journalisation actuellement nécessite un ILoggerFactory qui est lui-même configuré avec un ou plusieurs ILoggerProvider. Fournisseurs courants sont fournis dans les packages suivants :

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un journal de console simple.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Services d’application Azure prend en charge les journaux de diagnostic et des fonctionnalités de « Log stream ».
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): journaux vers un moniteur de débogueur à l’aide de System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): journaux pour le journal des événements Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): prend en charge/EventListener EventSource.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): journaux à un écouteur de trace à l’aide de System.Diagnostics.TraceSource.TraceEvent().

Après avoir installé les packages appropriés, l’application doit créer une instance de singleton/globale d’un LoggerFactory. Par exemple, utilisez le journal de console :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Cette instance de singleton/globale doit être inscrits avec EF Core sur le `DbContextOptionsBuilder`. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Il est très important que les applications ne créent pas une nouvelle instance de ILoggerFactory pour chaque instance de contexte. Cela entraîne une fuite de mémoire et des performances médiocres.

## <a name="filtering-what-is-logged"></a>Filtrage de ce qui est enregistré

Pour filtrer ce qui est enregistré, le plus simple consiste à configurer lors de l’inscription de l’ILoggerProvider. Exemple :

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

Dans cet exemple, le journal est filtré pour retourner uniquement les messages :
 * dans la catégorie « Microsoft.EntityFrameworkCore.Database.Command »
 * au niveau des « Informations »

Pour EF Core, les catégories de l’enregistreur d’événements sont définies dans le `DbLoggerCategory` résoudre les classe pour le rendre plus faciles à trouver les catégories, mais ces chaînes simples.

Vous trouverez plus d’informations sur l’infrastructure sous-jacente de la journalisation dans le [documentation sur la journalisation ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
