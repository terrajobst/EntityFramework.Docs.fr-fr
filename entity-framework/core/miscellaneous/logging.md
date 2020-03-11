---
title: Journalisation-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416581"
---
# <a name="logging"></a><span data-ttu-id="ec97f-102">Journalisation</span><span class="sxs-lookup"><span data-stu-id="ec97f-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="ec97f-103">Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) sur GitHub.</span><span class="sxs-lookup"><span data-stu-id="ec97f-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="ec97f-104">Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec97f-104">ASP.NET Core applications</span></span>

<span data-ttu-id="ec97f-105">EF Core s’intègre automatiquement avec les mécanismes de journalisation de ASP.NET Core chaque fois que `AddDbContext` ou `AddDbContextPool` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="ec97f-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="ec97f-106">Par conséquent, lors de l’utilisation de ASP.NET Core, la journalisation doit être configurée comme décrit dans la [documentation de ASP.net Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ec97f-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="ec97f-107">Autres applications</span><span class="sxs-lookup"><span data-stu-id="ec97f-107">Other applications</span></span>

<span data-ttu-id="ec97f-108">EF Core la journalisation requiert un ILoggerFactory qui est lui-même configuré avec un ou plusieurs fournisseurs de journalisation.</span><span class="sxs-lookup"><span data-stu-id="ec97f-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="ec97f-109">Les fournisseurs courants sont fournis dans les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="ec97f-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="ec97f-110">[Microsoft. extensions. Logging. console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un simple journal de console.</span><span class="sxs-lookup"><span data-stu-id="ec97f-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="ec97f-111">[Microsoft. extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): prend en charge les fonctionnalités « journaux de diagnostic » et « flux de journal » des Services de Azure App.</span><span class="sxs-lookup"><span data-stu-id="ec97f-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="ec97f-112">[Microsoft. extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): se connecte à un moniteur du débogueur à l’aide de System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="ec97f-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="ec97f-113">[Microsoft. extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): enregistre dans le journal des événements Windows.</span><span class="sxs-lookup"><span data-stu-id="ec97f-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="ec97f-114">[Microsoft. extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): prend en charge EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="ec97f-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="ec97f-115">[Microsoft. extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): se connecte à un écouteur de suivi à l’aide de `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="ec97f-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="ec97f-116">Après l’installation du ou des packages appropriés, l’application doit créer une instance singleton/globale d’un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="ec97f-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="ec97f-117">Par exemple, à l’aide de l’enregistreur d’événements de console :</span><span class="sxs-lookup"><span data-stu-id="ec97f-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="ec97f-118">Version 3.x</span><span class="sxs-lookup"><span data-stu-id="ec97f-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="ec97f-119">Version 2.x</span><span class="sxs-lookup"><span data-stu-id="ec97f-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="ec97f-120">L’exemple de code suivant utilise un constructeur `ConsoleLoggerProvider` qui a été obsolète dans la version 2,2 et remplacé dans 3,0.</span><span class="sxs-lookup"><span data-stu-id="ec97f-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="ec97f-121">Il est possible d’ignorer et de supprimer sans risque les avertissements lors de l’utilisation de 2,2.</span><span class="sxs-lookup"><span data-stu-id="ec97f-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="ec97f-122">Cette instance de Singleton/global doit ensuite être inscrite auprès de EF Core sur le `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ec97f-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="ec97f-123">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ec97f-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="ec97f-124">Il est très important que les applications ne créent pas une nouvelle instance ILoggerFactory pour chaque instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="ec97f-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="ec97f-125">Cela entraînera une fuite de mémoire et des performances médiocres.</span><span class="sxs-lookup"><span data-stu-id="ec97f-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="ec97f-126">Filtrage des éléments consignés</span><span class="sxs-lookup"><span data-stu-id="ec97f-126">Filtering what is logged</span></span>

<span data-ttu-id="ec97f-127">L’application peut contrôler ce qui est enregistré en configurant un filtre sur le ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="ec97f-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="ec97f-128">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ec97f-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="ec97f-129">Version 3.x</span><span class="sxs-lookup"><span data-stu-id="ec97f-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="ec97f-130">Version 2.x</span><span class="sxs-lookup"><span data-stu-id="ec97f-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="ec97f-131">L’exemple de code suivant utilise un constructeur `ConsoleLoggerProvider` qui a été obsolète dans la version 2,2 et remplacé dans 3,0.</span><span class="sxs-lookup"><span data-stu-id="ec97f-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="ec97f-132">Il est possible d’ignorer et de supprimer sans risque les avertissements lors de l’utilisation de 2,2.</span><span class="sxs-lookup"><span data-stu-id="ec97f-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="ec97f-133">Dans cet exemple, le journal est filtré pour renvoyer uniquement les messages :</span><span class="sxs-lookup"><span data-stu-id="ec97f-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="ec97f-134">dans la catégorie « Microsoft. EntityFrameworkCore. Database. Command »</span><span class="sxs-lookup"><span data-stu-id="ec97f-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="ec97f-135">au niveau « information »</span><span class="sxs-lookup"><span data-stu-id="ec97f-135">at the 'Information' level</span></span>

<span data-ttu-id="ec97f-136">Par EF Core, les catégories de journalisation sont définies dans la classe `DbLoggerCategory` pour faciliter la recherche des catégories, mais elles sont résolues en chaînes simples.</span><span class="sxs-lookup"><span data-stu-id="ec97f-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="ec97f-137">Pour plus d’informations sur l’infrastructure de journalisation sous-jacente, consultez la [documentation ASP.net Core Logging](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ec97f-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
