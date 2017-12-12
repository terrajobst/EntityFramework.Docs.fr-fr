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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="fe627-102">EF Core et EF6 : lequel choisir ?</span><span class="sxs-lookup"><span data-stu-id="fe627-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="fe627-103">Les informations suivantes vous aideront à choisir entre Entity Framework Core et Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fe627-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="fe627-104">Conseils pour les nouvelles applications</span><span class="sxs-lookup"><span data-stu-id="fe627-104">Guidance for new applications</span></span>

<span data-ttu-id="fe627-105">EF Core étant un nouveau produit qui n’intègre toujours pas certaines fonctionnalités de mappage objet-relationnel (O/RM) essentielles, EF6 reste le meilleur choix pour de nombreuses applications.</span><span class="sxs-lookup"><span data-stu-id="fe627-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="fe627-106">**Voici les types d’applications pour lesquels nous vous recommandons d’utiliser EF Core :**</span><span class="sxs-lookup"><span data-stu-id="fe627-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="fe627-107">Les nouvelles applications qui ne nécessitent pas les fonctionnalités qui ne sont pas encore implémentées dans EF Core.</span><span class="sxs-lookup"><span data-stu-id="fe627-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="fe627-108">Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.</span><span class="sxs-lookup"><span data-stu-id="fe627-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="fe627-109">Les applications qui ciblent .NET Core, telles que les applications de plateforme Windows universelle (UWP) et ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe627-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="fe627-110">Ces applications ne peuvent pas utiliser EF6 qui requiert .NET Framework (plus précisément .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="fe627-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="fe627-111">Conseils pour les applications EF6 existantes</span><span class="sxs-lookup"><span data-stu-id="fe627-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="fe627-112">En raison des modifications importantes apportées à EF Core, nous vous déconseillons de migrer une application EF6 vers EF Core, à moins d’avoir une raison justifiant réellement ce changement.</span><span class="sxs-lookup"><span data-stu-id="fe627-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="fe627-113">Si vous souhaitez migrer vers Core EF pour utiliser de nouvelles fonctionnalités, vérifiez bien ses limitations avant de le faire.</span><span class="sxs-lookup"><span data-stu-id="fe627-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="fe627-114">Consultez [Comparaison de fonctionnalités](features.md) pour vérifier si EF Core est le bon choix pour votre application.</span><span class="sxs-lookup"><span data-stu-id="fe627-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="fe627-115">**Il est préférable de considérer la migration de EF6 vers EF Core comme un port plutôt qu’une mise à niveau.**</span><span class="sxs-lookup"><span data-stu-id="fe627-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="fe627-116">Pour plus d’informations, consultez [Portage d’EF6 vers EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe627-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
