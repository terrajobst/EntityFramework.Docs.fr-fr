---
title: Écriture d’un fournisseur de la base de données - EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993664"
---
# <a name="writing-a-database-provider"></a>Écriture d’un fournisseur de base de données

Pour plus d’informations sur l’écriture d’un fournisseur de base de données Entity Framework Core, consultez [, vous pouvez écrire un fournisseur EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) par [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Ces publications n’ont pas été mis à jour depuis EF Core 1.1 et des modifications significatives ont été depuis ce moment [problème 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) effectue le suivi des mises à jour à cette documentation.

La base de code EF Core est open source et contient plusieurs fournisseurs de base de données qui peuvent être utilisées en tant que référence. Vous trouverez le code source à https://github.com/aspnet/EntityFrameworkCore. Il peut également être utile d’examiner le code pour les fournisseurs tiers couramment utilisées, telles que [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), et [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). En particulier, ces projets sont configurés pour étendre à partir d’et exécuter des tests fonctionnels que nous publions sur NuGet. Ce type de configuration est fortement recommandé.

## <a name="keeping-up-to-date-with-provider-changes"></a>Mise à jour avec les modifications de fournisseur

Commençant par travail après la version 2.1, nous avons créé un [journal des modifications](provider-log.md) nécessitant que les modifications correspondantes dans le code du fournisseur. Cela vise à aider lors de la mise à jour un fournisseur existant pour travailler avec une nouvelle version d’EF Core.

Avant 2.1, nous avons utilisé le [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) et [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) étiquettes sur nos problèmes GitHub et les demandes de tirage pour un objectif similaire. Nous allons continiue pour utiliser ces étiquettes sur les problèmes de donner une indication que les éléments de travail dans une version donnée peuvent également exiger de travail à effectuer dans les fournisseurs. Un `providers-beware` étiquette signifie généralement que l’implémentation d’un élément de travail peut entraîner la rupture des fournisseurs, tandis qu’un `providers-fyi` étiquette signifie généralement que les fournisseurs ne sera pas interrompues, mais code peut nécessiter à modifier en tout cas, par exemple, pour activer les nouvelles fonctionnalités.

## <a name="suggested-naming-of-third-party-providers"></a>Suggéré d’affectation de noms de fournisseurs tiers

Nous vous suggérons d’utiliser le suivantes d’affectation de noms pour les packages NuGet. Cela est cohérent avec les noms des packages fournis par l’équipe EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Exemple :
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
