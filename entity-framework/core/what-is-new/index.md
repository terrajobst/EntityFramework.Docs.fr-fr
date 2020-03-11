---
title: Mise en production et planification d’EF Core
author: ajcvickers
ms.date: 03/03/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2c41f65d1fead8430a39c6230a0f22506686504e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413204"
---
# <a name="ef-core-releases-and-planning"></a>Mise en production et planification d’EF Core

## <a name="stable-releases"></a>Versions stables

| Édition | Framework cible | Fin de prise en charge | Liens
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.2) | .NET Standard 2.0 | 3 décembre 2022 (LTS) | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Expiré le 3 mars 2020 | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Changements cassants](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Expiration le 23 décembre 2019 | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 août 2021 (LTS) | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Expiration le 1er octobre 2018 | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Expiration le 27 juin 2019 | [Annonce](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Expiration le 27 juin 2019 | [Annonce](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Pour plus d’informations sur les plateformes prises en charge par chacune des versions d’EF Core, voir [Plateformes prises en charge](../platforms/index.md).

Pour plus d’informations sur les versions à expiration du support et les versions LTS (support à long terme), voir [Stratégie de support .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="guidance-on-updating-to-new-releases"></a>Aide sur la mise à jour vers les nouvelles versions

* La sécurité et d’autres bogues critiques sont corrigés dans les versions prises en charge. Utilisez toujours le dernier correctif d’une version donnée (par exemple, 2.1.14 pour EF Core 2.1).
* Les mises à jour de versions majeures (par exemple, de EF Core 2 à EF Core 3) présentent souvent des changements cassants. Un test approfondi est recommandé dans ce cas de figure. Suivez les liens ci-dessus pour savoir comment gérer les changements cassants.
* Les mises à jour de versions mineures ne contiennent généralement pas de changements cassants. Toutefois, des tests poussés sont toujours recommandés, car les nouvelles fonctionnalités sont susceptibles d’introduire des régressions.

## <a name="release-planning-and-schedules"></a>Planification et planification des versions

Les versions d’EF Core s’alignent sur la [planification d’expédition .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

Les mises à jour correctives sont généralement envoyées tous les mois, mais avec un délai assez long.
Nous nous efforçons d’améliorer ce point.

Pour plus d’informations sur la façon dont nous sélectionnons les éléments à envoyer dans chaque version, consultez le [processus de planification des versions](release-planning.md).
En règle générale, nous ne procédons pas à une planification détaillée au-delà de la prochaine version majeure ou mineure.

## <a name="ef-core-50"></a>EF Core 5.0

La prochaine version stable planifiée est **EF Core 5.0**, prévue pour novembre 2020.

Un [plan global pour EF Core 5.0](ef-core-5.0/plan.md) a été créé en suivant le [processus de planification des versions](release-planning.md) documenté.

Vos commentaires sur la planification sont importants.
La meilleure façon d’indiquer l’importance d’un problème est de voter (pouce vers le haut 👍) pour ce problème sur GitHub.
Ces données sont ensuite intégrées dans le processus de planification de la prochaine version.

### <a name="get-it-now"></a>Télécharger maintenant

Les packages EF Core 5.0 sont **disponibles maintenant** sous forme de [builds quotidiens](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

Les builds quotidiens représentent un excellent moyen de trouver des problèmes et de fournir un retour d’expérience le plus tôt possible.
Plus tôt nous recevons ces commentaires, plus ils ont de chances d’être exploitables avant la version officielle suivante.
Nous faisons tout notre possible pour que les builds quotidiens fonctionnent correctement, en exécutant pour chacun plus de 56 000 tests par plateforme.

Les packages en préversion seront expédiés à NuGet plus tard dans l’année.
