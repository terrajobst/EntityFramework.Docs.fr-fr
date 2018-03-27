---
title: 'EF Core et EF6 : lequel choisir ?'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core et EF6 : lequel choisir ?

Les informations suivantes vous aideront à choisir entre Entity Framework Core et Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Conseils pour les nouvelles applications

Utilisez EF Core pour les nouvelles applications si vous souhaitez tirer parti de toutes les fonctionnalités d’EF Core et que votre application n’exige aucune fonctionnalité qui n’est pas encore implémentée dans EF Core.

EF6 nécessite le .NET Framework 4.0 (ou une version ultérieure) et est uniquement pris en charge sur Windows (autrement dit, il ne s’exécute pas sur .NET Core et n’est pas pris en charge dans d’autres systèmes d’exploitation), mais il s’agit toujours d’un choix viable pour les nouvelles applications tant que ces contraintes sont acceptables et que l’application ne nécessite pas de nouvelles fonctionnalités dans EF Core qui ne sont pas accessibles à EF6.

Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

## <a name="guidance-for-existing-ef6-applications"></a>Conseils pour les applications EF6 existantes

En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement. Si vous souhaitez migrer vers Core EF pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations avant de le faire. Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

**Il est préférable de considérer la migration de EF6 vers EF Core comme un port plutôt qu’une mise à niveau.** Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md).
