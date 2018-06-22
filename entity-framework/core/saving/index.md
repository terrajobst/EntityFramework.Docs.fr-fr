---
title: Enregistrement de données - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048871"
---
# <a name="saving-data"></a>Enregistrement de données

Chaque instance de contexte a un `ChangeTracker` qui est responsable du suivi des modifications à écrire dans la base de données. Quand vous apportez des modifications à des instances de vos classes d’entité, ces modifications sont enregistrées dans le `ChangeTracker`, puis écrites dans la base de données quand vous appelez `SaveChanges`. Le fournisseur de base de données est chargé de traduire les modifications en opérations de base de données (telles que les commandes `INSERT`, `UPDATE` et `DELETE` pour une base de données relationnelle).
