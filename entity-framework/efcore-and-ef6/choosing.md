---
title: 'EF Core et EF6 : lequel choisir ?'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core et EF6 : lequel choisir ?

Les informations suivantes vous aideront à choisir entre Entity Framework Core et Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Conseils pour les nouvelles applications

EF Core étant un nouveau produit qui n’intègre toujours pas certaines fonctionnalités de mappage objet-relationnel (O/RM) essentielles, EF6 reste le meilleur choix pour de nombreuses applications.

**Voici les types d’applications pour lesquels nous vous recommandons d’utiliser EF Core :**

* Les nouvelles applications qui ne nécessitent pas les fonctionnalités qui ne sont pas encore implémentées dans EF Core. Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

* Les applications qui ciblent .NET Core, telles que les applications de plateforme Windows universelle (UWP) et ASP.NET Core. Ces applications ne peuvent pas utiliser EF6 qui requiert .NET Framework (plus précisément .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Conseils pour les applications EF6 existantes

En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement. Si vous souhaitez migrer vers Core EF pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations avant de le faire. Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

**Il est préférable de considérer la migration de EF6 vers EF Core comme un port plutôt qu’une mise à niveau.** Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md).
