---
title: Écriture d’un fournisseur de base de données-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417882"
---
# <a name="writing-a-database-provider"></a>Écriture d’un fournisseur de base de données

Pour plus d’informations sur l’écriture d’un fournisseur de base de données Entity Framework Core, consultez. pour [ce faire, vous devez écrire un fournisseur EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) par [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Ces publications n’ont pas été mises à jour depuis EF Core 1,1 et des modifications significatives ont été apportées depuis le moment où le [problème 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) est le suivi des mises à jour de cette documentation.

La base de code EF Core est open source et contient plusieurs fournisseurs de bases de données qui peuvent être utilisés comme référence. Vous pouvez trouver le code source à <https://github.com/aspnet/EntityFrameworkCore>. Il peut également être utile d’examiner le code pour les fournisseurs tiers couramment utilisés, tels que [npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)et [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). En particulier, ces projets sont configurés pour étendre et exécuter des tests fonctionnels que nous publions sur NuGet. Ce type d’installation est fortement recommandé.

## <a name="keeping-up-to-date-with-provider-changes"></a>Tenir à jour les modifications apportées au fournisseur

À compter du travail après la version 2,1, nous avons créé un [Journal des modifications](provider-log.md) qui peut nécessiter des modifications correspondantes dans le code du fournisseur. Cela est destiné à faciliter la mise à jour d’un fournisseur existant pour qu’il fonctionne avec une nouvelle version de EF Core.

Avant le 2,1, nous avons utilisé les étiquettes [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) et [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) sur nos problèmes GitHub et les demandes de tirage (pull requests) à des fins similaires. Nous continiue utiliser ces étiquettes sur les problèmes pour donner une indication que les éléments de travail d’une version donnée peuvent également nécessiter un travail dans les fournisseurs. Une étiquette `providers-beware` signifie généralement que l’implémentation d’un élément de travail peut arrêter des fournisseurs, tandis qu’une `providers-fyi` étiquette signifie généralement que les fournisseurs ne seront pas rompus, mais le code devra peut-être être modifié de toute manière, par exemple, pour activer les nouvelles fonctionnalités.

## <a name="suggested-naming-of-third-party-providers"></a>Attribution d’un nom suggéré aux fournisseurs tiers

Nous vous suggérons d’utiliser le nom suivant pour les packages NuGet. Cela est cohérent avec les noms des packages fournis par l’équipe EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Par exemple :

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
