---
title: Enregistrement - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232134"
---
# <a name="logging"></a><span data-ttu-id="29d22-102">Journalisation</span><span class="sxs-lookup"><span data-stu-id="29d22-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="29d22-103">Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="29d22-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="29d22-104">Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29d22-104">ASP.NET Core applications</span></span>

<span data-ttu-id="29d22-105">EF Core intègre automatiquement avec les mécanismes de journalisation d’ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="29d22-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="29d22-106">Par conséquent, lorsque vous utilisez ASP.NET Core, la journalisation doit être configurée comme décrit dans la [documentation d’ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="29d22-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="29d22-107">Autres applications</span><span class="sxs-lookup"><span data-stu-id="29d22-107">Other applications</span></span>

<span data-ttu-id="29d22-108">Core EF journalisation actuellement requiert un ILoggerFactory qui est lui-même configuré avec un ou plusieurs ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="29d22-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="29d22-109">Fournisseurs courants sont inclus dans les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="29d22-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="29d22-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un journal de console simple.</span><span class="sxs-lookup"><span data-stu-id="29d22-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="29d22-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Services d’application Azure prend en charge les journaux de diagnostic et les fonctionnalités de « Log flux ».</span><span class="sxs-lookup"><span data-stu-id="29d22-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="29d22-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): journaux à une analyse de débogueur à l’aide de System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="29d22-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="29d22-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): les journaux pour le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="29d22-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="29d22-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): prend en charge EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="29d22-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="29d22-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): journaux pour un écouteur de trace à l’aide de System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="29d22-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="29d22-116">Après avoir installé l’ou les modules approprié, l’application doit créer une instance de singleton/globale d’un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="29d22-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="29d22-117">Par exemple, à l’aide de l’enregistreur d’événements de la console :</span><span class="sxs-lookup"><span data-stu-id="29d22-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="29d22-118">Cette instance de singleton/global doit être inscrits avec EF de base sur la `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="29d22-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="29d22-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="29d22-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="29d22-120">Il est très important que les applications ne créent pas une nouvelle instance de ILoggerFactory pour chaque instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="29d22-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="29d22-121">Cela entraîne une fuite de mémoire et de performances médiocres.</span><span class="sxs-lookup"><span data-stu-id="29d22-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="29d22-122">Filtrage des éléments enregistrés</span><span class="sxs-lookup"><span data-stu-id="29d22-122">Filtering what is logged</span></span>

<span data-ttu-id="29d22-123">Pour filtrer les éléments enregistrés, le plus simple consiste à configurer lors de l’inscription de l’ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="29d22-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="29d22-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="29d22-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="29d22-125">Dans cet exemple, le journal est filtré pour retourner uniquement les messages :</span><span class="sxs-lookup"><span data-stu-id="29d22-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="29d22-126">dans la catégorie 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="29d22-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="29d22-127">au niveau « Informations »</span><span class="sxs-lookup"><span data-stu-id="29d22-127">at the 'Information' level</span></span>

<span data-ttu-id="29d22-128">Pour EF Core, les catégories de l’enregistreur d’événements sont définies dans le `DbLoggerCategory` classe pour faciliter le travail rechercher des catégories, mais ces résoudre en chaînes simples.</span><span class="sxs-lookup"><span data-stu-id="29d22-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="29d22-129">Vous trouverez plus d’informations sur l’infrastructure sous-jacente de la journalisation dans le [documentation de journalisation ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="29d22-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
