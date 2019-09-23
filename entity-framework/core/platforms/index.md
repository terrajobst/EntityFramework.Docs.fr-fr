---
title: Implémentations de .NET prises en charge - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: ac3cf3d0a84200bbf4ba7ec18b9115e06d1748f4
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149211"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implémentations de .NET prises en charge par EF Core

Nous souhaitons qu’EF Core soit disponible pour les développeurs sur toutes les implémentations de .NET modernes et nous travaillons toujours à cet objectif. Si la prise en charge d’EF Core sur .NET Core fait l’objet de tests automatisés et fonctionne correctement dans de nombreuses applications, Mono, Xamarin et UWP posent des problèmes.

## <a name="overview"></a>Vue d'ensemble

Le tableau suivant fournit des conseils pour chaque implémentation de .NET :

| EF Core                       | 1.x    | 2.x        | 3.x             |
|:------------------------------|:-------|:-----------|:----------------|
| .NET Standard                 | 1.3    | 2.0        | 2.1             |
| .NET Core                     | 1.0    | 2.0        | 3.0             |
| .NET Framework<sup>(1)</sup>  | 4.5.1  | 4.7.2      | (non pris en charge) |
| Mono                          | 4.6    | 5,4        | 6.4             |
| Xamarin.iOS<sup>(2)</sup>     | 10.0   | 10.14      | 12.16           |
| Xamarin.Android<sup>(2)</sup> | 7.0    | 8.0        | 10.0            |
| UWP<sup>(3)</sup>             | 10.0   | 10.0.16299 | TBD             |
| Unity<sup>(4)</sup>           | 2018.1 | 2018.1     | TBD             |

<sup>(1)</sup> Consultez la section [.NET Framework](#net-framework) ci-dessous.

<sup>(2)</sup> La présence de problèmes et de limitations connues avec Xamarin peut entraîner un dysfonctionnement de certaines applications développées à l’aide d’EF Core. Consultez la liste des [problèmes actifs](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pour connaître les solutions de contournement.

<sup>(3)</sup> EF Core 2.0.1 et versions ultérieures recommandées. Installez le [package .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/). Consultez la section [Plateforme Windows universelle](#universal-windows-platform) de cet article.

<sup>4</sup> Il existe des problèmes et des limitations connus avec Unity. Consultez la liste des [problèmes actifs](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).

## <a name="net-framework"></a>.NET Framework

Vous devez peut-être modifier les applications qui ciblent le .NET Framework pour qu’elle fonctionnent avec les bibliothèques .NET Standard :

Modifiez le fichier projet et vérifiez que l’entrée suivante apparaît dans le groupe de propriétés initiales :

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

Pour les projets de test, vérifiez également que l’entrée suivante est présente :

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Si vous voulez utiliser une ancienne version de Visual Studio, vous devez [mettre à niveau le client NuGet vers la version 3.6.0](https://www.nuget.org/downloads) pour utiliser les bibliothèques .NET Standard 2.0.

Nous vous recommandons également d’effectuer une migration depuis NuGet packages.config vers PackageReference, si possible. Ajoutez la propriété suivante à votre fichier projet :

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Plateforme Windows universelle

Les versions antérieures d’EF Core et de .NET UWP présentaient de nombreux problèmes de compatibilité, notamment avec les applications compilées avec la chaîne d’outils .NET Native. La nouvelle version de .NET UWP prend en charge .NET Standard 2.0 et contient .NET Native 2.0 qui résout la plupart des problèmes de compatibilité signalés précédemment. EF Core 2.0.1 a été testé de manière plus approfondie avec UWP, mais les tests ne sont pas automatisés.

Avec EF Core sur UWP :

* Pour optimiser les performances des requêtes, évitez les types anonymes dans les requêtes LINQ. Une application UWP doit être générée avec .NET Native pour pouvoir être déployée dans le magasin d’applications. Les requêtes avec des types anonymes ont des performances inférieures sur .NET Native.

* Pour optimiser les performances `SaveChanges()`, utilisez [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) et implémentez [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) et [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) dans vos types d’entité.

## <a name="report-issues"></a>Signaler des problèmes

Pour toute combinaison ne fonctionnant pas comme prévu, nous vous invitons à signaler les nouveaux problèmes dans le [système de suivi des problèmes EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Pour les problèmes spécifiques à Xamarin, utilisez le système de suivi des problèmes pour [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) ou [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
