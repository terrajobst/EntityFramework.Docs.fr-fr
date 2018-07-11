---
title: Enregistrement de données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949393"
---
# <a name="saving-data"></a><span data-ttu-id="34482-102">Enregistrement de données</span><span class="sxs-lookup"><span data-stu-id="34482-102">Saving Data</span></span>

<span data-ttu-id="34482-103">Chaque instance de contexte a un `ChangeTracker` qui est responsable du suivi des modifications à écrire dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="34482-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="34482-104">Quand vous apportez des modifications à des instances de vos classes d’entité, ces modifications sont enregistrées dans le `ChangeTracker`, puis écrites dans la base de données quand vous appelez `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="34482-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="34482-105">Le fournisseur de base de données est chargé de traduire les modifications en opérations de base de données (par exemple les commandes `INSERT`, `UPDATE` et `DELETE` pour une base de données relationnelle).</span><span class="sxs-lookup"><span data-stu-id="34482-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
