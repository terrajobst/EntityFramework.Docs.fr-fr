---
title: "Implémentations de .NET prises en charge - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Implémentations de .NET prises en charge par EF Core

Nous souhaitons que vous puissiez utiliser EF Core partout où vous pouvez écrire du code .NET, c’est pourquoi nous y travaillons encore. Le tableau suivant fournit des conseils pour chaque implémentation de .NET où nous voulons permettre l’utilisation d’EF Core.

EF Core 2.0 ciblant [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard), il nécessite des implémentations de .NET qui le prennent en charge.

| Implémentation de .NET | Statut | 1.x nécessite | 2.x nécessite
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Console](../get-started/netcore/index.md), etc.) | **Entièrement prise en charge et recommandée :** couverte par des tests automatisés et de nombreuses applications connues pour l’utiliser avec succès. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework** (WinForms, WPF, ASP.NET, [Console](../get-started/full-dotnet/index.md), etc.) | **Entièrement prise en charge et recommandée :** couverte par des tests automatisés et de nombreuses applications connues pour l’utiliser avec succès. EF 6 est également disponible dans cette plateforme (consultez [Comparer EF Core et EF6](../../efcore-and-ef6/index.md) pour choisir la technologie appropriée). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono & Xamarin** | **En cours, des problèmes peuvent être rencontrés :** des tests ont été effectués par l’équipe et des clients EF Core. Les premiers utilisateurs ont fait état de bons résultats, mais des [problèmes ont été rencontrés](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) et d’autres vont probablement être découverts à mesure que les tests se poursuivront. Il existe notamment des limitations dans Xamarin.iOS qui peuvent entraîner un dysfonctionnement de certaines applications développées à l’aide d’EF Core 2.0. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Plateforme Windows universelle**](../get-started/uwp/index.md) | **En cours, des problèmes peuvent être rencontrés :** des tests ont été effectués par l’équipe et des clients EF Core. Des [problèmes](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) liés à la compilation avec la chaîne d’outils .NET Native ont été signalés ; celle-ci est généralement utilisée pendant une build de mise en production et est obligatoire pour le déploiement sur le Windows Store (si vous n’utilisez pas .NET Native, ou que vous souhaitez simplement faire des essais, la plupart des problèmes ne vous concernent pas). | [Dernier package .NET UWP 5](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Dernier package .NET UWP 6](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> Cette version de .NET UWP prend en charge .NET Standard 2.0 et contient .NET Native 2.0, qui résout la plupart des problèmes de compatibilité signalés, mais les tests ont révélé [quelques problèmes restants](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) avec EF Core 2.0 que nous envisageons de traiter dans un correctif à venir.

Pour toute combinaison ne fonctionnant pas comme prévu, nous vous invitons à signaler les problèmes dans le [système de suivi des problèmes EF Core](https://github.com/aspnet/entityframeworkcore/issues/new) et, pour les problèmes liés à Xamarin, dans le [système de suivi des problèmes Xamarin](https://bugzilla.xamarin.com/newbug).
