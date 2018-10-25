---
title: Utilisation d’entités déconnectées - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: 11ca2a9a4161e02d32d98bf03dd4cf28545334b7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022169"
---
# <a name="working-with-disconnected-entities"></a>Utilisation d’entités déconnectées
Dans une application Entity Framework, une classe de contexte détecte les changements des entités suivies. La méthode SaveChanges conserve les changements détectés par le contexte dans la base de données. Quand vous utilisez des applications multicouches, les objets d’entité sont généralement modifiés quand ils sont déconnectés du contexte, et vous devez choisir à la fois comment suivre les changements et comment les signaler au contexte. Cette rubrique décrit les différentes options disponibles quand vous utilisez Entity Framework avec des entités déconnectées.   

## <a name="web-service-frameworks"></a>Frameworks de service web

Les technologies de services web prennent généralement en charge des modèles pouvant servir à conserver les changements sur des objets déconnectés individuels. Par exemple, l’API Web ASP.NET vous permet de codifier des actions de contrôleur pouvant inclure des appels à EF pour conserver les changements d’un objet dans une base de données. En fait, les outils d’API Web dans Visual Studio facilitent la génération automatique d’un contrôleur API Web à partir de votre modèle Entity Framework 6. Pour plus d’informations, consultez [Utilisation de l’API Web avec Entity Framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

Historiquement, plusieurs autres technologies de services web ont offert une intégration à Entity Framework, par exemple, [WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) et [Services RIA](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>API EF de bas niveau

Si vous ne voulez pas utiliser de solution multicouche existante ou que vous voulez personnaliser une action de contrôleur dans les services d’API Web, Entity Framework fournit des API qui vous permettent d’appliquer les changements d’une couche déconnectée. Pour plus d’informations, consultez [Add, Attach et état d’entité](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Entités de suivi automatique  

Suivre les changements sur des graphiques d’entité arbitraires déconnectés du contexte EF n’est pas chose simple. Le problème a été partiellement résolu par le modèle de génération de code des entités de suivi automatique. Ce modèle génère des classes d’entité qui contiennent une logique pour suivre les changements d’une couche déconnectée, comme l’état dans les entités elles-mêmes. Un ensemble de méthodes d’extension est également généré pour appliquer ces changements à un contexte.

Ce modèle peut être utilisé avec des modèles créés à l’aide d’EF Designer, mais ne peut pas être utilisé avec des modèles Code First. Pour plus d’informations, consultez [Entités de suivi automatique](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Nous ne recommandons plus d’utiliser le modèle des entités de suivi automatique. Il reste disponible uniquement pour prendre en charge les applications existantes. Si votre application doit utiliser des graphiques d’entités déconnectés, envisagez d’autres alternatives comme les [Entités traçables](http://trackableentities.github.io/), qui présentent une technologie similaire aux entités de suivi automatique, mais développée de manière plus active par la communauté, ou bien l’écriture de code personnalisé à l’aide des API de suivi de changements de bas niveau.
