---
title: 'EF Core et EF6 : lequel choisir ?'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993831"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core et EF6 : lequel choisir ?

Les informations suivantes vous aideront à choisir entre Entity Framework Core et Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Conseils pour les nouvelles applications

Utilisez EF Core pour les nouvelles applications si vous souhaitez tirer parti de toutes les fonctionnalités d’EF Core et que votre application n’exige aucune fonctionnalité qui n’est pas encore implémentée dans EF Core.

EF6 nécessite .NET Framework 4.0 (ou une version ultérieure) et est pris en charge uniquement sur Windows (autrement dit, EF6 ne s’exécute actuellement pas sur .NET Core et n’est pas pris en charge dans d’autres systèmes d’exploitation), mais il s’agit toujours d’un choix viable pour les nouvelles applications tant que ces contraintes sont acceptables et que l’application ne nécessite pas de nouvelles fonctionnalités dans EF Core qui ne sont pas disponibles pour EF6.

Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

## <a name="guidance-for-existing-ef6-applications"></a>Conseils pour les applications EF6 existantes

En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement. Si vous souhaitez migrer vers Core EF pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations avant de le faire. Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.

**Il est préférable de considérer la migration de EF6 vers EF Core comme un port plutôt qu’une mise à niveau.** Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md).
