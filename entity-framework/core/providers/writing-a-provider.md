---
title: "L’écriture d’un fournisseur de base de données - EF Core"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a>Écriture d’un fournisseur de base de données

Pour plus d’informations sur l’écriture d’un fournisseur de base de données Entity Framework Core, consultez [, vous pouvez écrire un fournisseur EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) par [Arthur Vickers](https://github.com/ajcvickers).

La base de code EF Core est open source et contient plusieurs fournisseurs de base de données qui peuvent être utilisés en tant que référence. Vous trouverez le code source à https://github.com/aspnet/EntityFramework.

## <a name="the-providers-beware-label"></a>Les fournisseurs-Méfiez-vous de l’étiquette

Une fois que vous commencez à travailler sur un fournisseur, observer les [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) étiquette sur nos problèmes de GitHub et les requêtes d’extraction. Nous utilisons cette étiquette pour identifier les modifications qui peuvent avoir un impact sur les rédacteurs de fournisseur.

## <a name="suggested-naming-of-third-party-providers"></a>Suggéré d’affectation de noms de fournisseurs tiers

Nous suggérons d’utiliser la dénomination suivant pour les packages NuGet. Cela est cohérent avec les noms des packages fournis par l’équipe EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Exemple :
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
