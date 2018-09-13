---
title: Études de cas pour Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490874"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Études de cas Microsoft pour Entity Framework
Les études de cas sur cette page mettre en évidence quelques projets de production réel qui employaient Entity Framework.
> [!NOTE]
> Les versions détaillées de ces études de cas ne sont plus disponibles sur le site Web Microsoft. Par conséquent, les liens ont été supprimés.

## <a name="epicor"></a>Epicor
Epicor est une société de logiciels d’envergure mondiale (avec les développeurs plus de 400) qui développe des solutions de planification ERP (Enterprise Resource) pour les entreprises dans plus de 150 pays.
Produit phare, Epicor 9, est basé sur une Architecture orientée services (SOA) à l’aide de .NET Framework.
Face à nombreuses demandes de client pour fournir la prise en charge de requête LINQ (Language Integrated) et qui veulent également à réduire la charge sur leurs serveurs principaux SQL Server, l’équipe a décidé de mettre à niveau vers Visual Studio 2010 et .NET Framework 4.0.
À l’aide d’Entity Framework 4.0, ils pouvaient atteindre ces objectifs et de simplifier considérablement le développement et maintenance.
En particulier, riche T4 prise en charge d’Entity Framework leur permettait de prendre le contrôle de leur code généré et générer automatiquement des fonctionnalités de l’enregistrement des performances telles que les requêtes précompilées et la mise en cache.

> « Nous avons effectué des tests de performances récemment avec le code existant, et nous avons pu réduire les demandes vers SQL Server 90 %.
Cela est dû à ADO.NET Entity Framework 4. » – Études de produits Erik Johnson, vice-président,  

## <a name="veracity-solutions"></a>Solutions de la véracité
Avoir acquis un système de logiciels de planification d’événements qui devait être difficiles à maintenir et à étendre à long terme, les Solutions véracité utilisé Visual Studio 2010 au pour réécrire comme une Application d’Internet riche puissante et facile à utiliser basé sur Silverlight 4.
À l’aide des Services RIA .NET, ils ont été en mesure de générer rapidement une couche de service sur Entity Framework qui éviter la duplication de code et d’autorisation pour la validation courants et de la logique d’authentification au sein des niveaux.  

> « Nous étions vendues sur Entity Framework lors de sa première apparition et Entity Framework 4 s’est avérée être encore meilleures.
Outils a été amélioré, et il est plus facile à manipuler les fichiers .edmx qui définissent le modèle conceptuel, le modèle de stockage et le mappage entre ces modèles... Avec Entity Framework, je peux obtenir cette couche d’accès aux données fonctionne en une journée et générez-le out en entrant le long.
Entity Framework est notre couche d’accès aux données de fait ; Je ne sais pas pourquoi tout le monde n’aurait pas l’utiliser. » – Joe McBride, Senior Developer

## <a name="nec-display-solutions-of-america"></a>Solutions NEC affichage d’Amérique
NEC souhaitait accéder au marché à des fins publicitaires en fonction du lieu de numérique avec une solution à bénéficier des annonceurs et propriétaires de réseau et à augmenter sa propre chiffre d’affaires.
Pour ce faire, il lancé une paire d’applications web qui automatisent les processus manuels requis dans une campagne de publicité traditionnel.
Les sites ont été créés à l’aide d’ASP.NET, Silverlight 3, AJAX et WCF, ainsi que de Entity Framework dans la couche d’accès aux données pour communiquer avec SQL Server 2008.

> « Avec SQL Server, nous avons pensé nous aurions pu obtenir le débit que nous avions besoin pour traiter les annonceurs et les réseaux avec des informations en temps réel et la fiabilité afin de garantir que les informations contenues dans nos applications critiques sont toujours accessibles »-Mike Corcoran, Directeur informatique

## <a name="darwin-dimensions"></a>Dimensions de Darwin
À l’aide d’une large gamme de produits Microsoft, l’équipe de Darwin la valeur pour Evolver - un portail en ligne avatar que les clients peuvent utiliser pour créer les avatars étonnantes, plus réalistes pour une utilisation dans les pages de mise en réseau sociales, des animations et des jeux de créer.
Les avantages de productivité d’Entity Framework et en extrayant des composants tels que Windows Workflow Foundation (WF) et Windows Server AppFabric (un cache d’application hautement évolutive en mémoire), l’équipe a été en mesure de livrer un produit incroyable dans moins de 35 % temps de développement.
En dépit de membres de l’équipe divisé en plusieurs pays, l’équipe suivant un processus de développement agile avec les versions hebdomadaires.

 > « Nous essayer de ne pas créer de technologie considèrent. En tant que start-up, il est essentiel que nous exploiter la technologie qui enregistre le temps et argent.
 .NET a le choix pour le développement rapide et économique. » – Zachary Olsen, architecte  

## <a name="silverware"></a>Argenterie
Avec plus de 15 ans d’expérience dans le développement de point de vente (PDV) solutions pour les groupes de restaurant petites et moyennes entreprises, l’équipe de développement chez argenterie entrepris d’améliorer leur produit avec davantage de fonctionnalités de niveau entreprise afin d’attirer plus grande chaînes de restaurant.
À l’aide de la dernière version des outils de développement de Microsoft, ils ont été en mesure de générer la nouvelle solution quatre fois plus rapidement que jamais.
Nouvelles fonctionnalités clés telles que LINQ et Entity Framework simplifié la déplacer à partir de Crystal Reports vers SQL Server 2008 et SQL Server Reporting Services (SSRS) pour le stockage de leurs données et besoins de rapport.

> « Gestion des données efficace est essentielle à la réussite d’argenterie – et c’est pourquoi nous avons décidé d’adopter SQL Reporting ». -Nicholas Romanidis, directeur informatique / d’ingénierie logicielle
