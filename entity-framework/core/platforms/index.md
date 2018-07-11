---
title: Implémentations de .NET prises en charge - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900328"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implémentations de .NET prises en charge par EF Core

Nous souhaitons que vous puissiez utiliser EF Core partout où vous pouvez écrire du code .NET, c’est pourquoi nous y travaillons encore. Si la prise en charge d’EF Core sur .NET Core et le .NET Framework fait l’objet de tests automatisés et fonctionne correctement dans de nombreuses applications, Mono, Xamarin et UWP posent des problèmes.

Le tableau suivant fournit des conseils pour chaque implémentation de .NET :

| Implémentation de .NET                                                                                                  | Status                                                             | Exigences de EF Core 1.x                                                                                | Exigences de EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md), etc.) | Entièrement prise en charge et recommandée                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md), etc.)                    | Entièrement prise en charge et recommandée. EF6 également disponible <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | En cours <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Plateforme Windows universelle**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 recommandé <sup>(4)</sup>                           | [Package .NET Core UWP 5.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Package .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> EF Core 2.0 cible .NET et nécessite donc des implémentations .NET prenant en charge [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2)</sup> Consultez [Comparer EF Core et EF6](../../efcore-and-ef6/index.md) pour choisir la technologie appropriée.

<sup>(3)</sup> La présence de problèmes et de limitations connues avec Xamarin peut entraîner un dysfonctionnement de certaines applications développées à l’aide d’EF Core 2.0. Consultez la liste des [problèmes actifs](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pour connaître les solutions de contournement.

<sup>(4) </sup> Les versions antérieures d’EF Core et de .NET UWP présentaient plusieurs problèmes de compatibilité, notamment avec les applications compilées avec la chaîne d’outils .NET Native. La nouvelle version de .NET UWP prend en charge .NET Standard 2.0 et contient .NET Native 2.0 qui résout la plupart des problèmes de compatibilité signalés précédemment. EF Core 2.0.1 a été testé de manière plus approfondie avec UWP, mais les tests ne sont pas automatisés.

Pour toute combinaison ne fonctionnant pas comme prévu, nous vous invitons à signaler les nouveaux problèmes dans le [système de suivi des problèmes EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Pour les problèmes spécifiques à Xamarin, utilisez le système de suivi des problèmes pour [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) ou [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
