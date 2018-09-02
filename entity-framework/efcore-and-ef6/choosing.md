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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="ba890-102">EF Core et EF6 : lequel choisir ?</span><span class="sxs-lookup"><span data-stu-id="ba890-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="ba890-103">Les informations suivantes vous aideront à choisir entre Entity Framework Core et Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ba890-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="ba890-104">Conseils pour les nouvelles applications</span><span class="sxs-lookup"><span data-stu-id="ba890-104">Guidance for new applications</span></span>

<span data-ttu-id="ba890-105">Utilisez EF Core pour les nouvelles applications si vous souhaitez tirer parti de toutes les fonctionnalités d’EF Core et que votre application n’exige aucune fonctionnalité qui n’est pas encore implémentée dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="ba890-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="ba890-106">EF6 nécessite .NET Framework 4.0 (ou une version ultérieure) et est pris en charge uniquement sur Windows (autrement dit, EF6 ne s’exécute actuellement pas sur .NET Core et n’est pas pris en charge dans d’autres systèmes d’exploitation), mais il s’agit toujours d’un choix viable pour les nouvelles applications tant que ces contraintes sont acceptables et que l’application ne nécessite pas de nouvelles fonctionnalités dans EF Core qui ne sont pas disponibles pour EF6.</span><span class="sxs-lookup"><span data-stu-id="ba890-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="ba890-107">Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.</span><span class="sxs-lookup"><span data-stu-id="ba890-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="ba890-108">Conseils pour les applications EF6 existantes</span><span class="sxs-lookup"><span data-stu-id="ba890-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="ba890-109">En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement.</span><span class="sxs-lookup"><span data-stu-id="ba890-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="ba890-110">Si vous souhaitez migrer vers Core EF pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations avant de le faire.</span><span class="sxs-lookup"><span data-stu-id="ba890-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="ba890-111">Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.</span><span class="sxs-lookup"><span data-stu-id="ba890-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="ba890-112">**Il est préférable de considérer la migration de EF6 vers EF Core comme un port plutôt qu’une mise à niveau.**</span><span class="sxs-lookup"><span data-stu-id="ba890-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="ba890-113">Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="ba890-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
