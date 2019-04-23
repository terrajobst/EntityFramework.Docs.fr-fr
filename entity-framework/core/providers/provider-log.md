---
title: Journal des modifications ayant un impact sur le fournisseur - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 61a58bd6119763d90731fac62343b983af510cb6
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929873"
---
# <a name="provider-impacting-changes"></a>Modifications ayant un impact sur le fournisseur

Cette page contient des liens pour extraire les requêtes effectuées sur le dépôt EF Core qui peut-être nécessiter des auteurs d’autres fournisseurs de base de données de réagir. L’objectif est de fournir un point de départ pour les auteurs des fournisseurs de base de données de tiers existants lors de la mise à jour de leur fournisseur vers une nouvelle version.

Nous avons commencé ce journal avec des modifications entre 2.1 et 2.2. Avant 2.1, nous avons utilisé le [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) et [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) étiquettes sur nos problèmes et les demandes de tirage.

## <a name="22-----30"></a>2.2 ---> 3.0

Notez que de nombreux de la [modifications avec rupture au niveau application](../what-is-new/ef-core-3.0/breaking-changes.md) affectera également des fournisseurs.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * API obsolètes supprimés et les surcharges de paramètre facultatif réduite
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * API obsolètes supprimés
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Les sous-classes de CharTypeMapping a peut-être été interrompues en raison de changements de comportement requis pour la résolution de quelques bogues dans l’implémentation de base.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Ajouter une classe de base pour IDatabaseModelFactory et mis à jour pour utiliser un objet de paramètre pour atténuer les sauts de futures.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Les objets de paramètre utilisés dans MigrationsSqlGenerator pour atténuer les sauts de futures.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Une configuration explicite de niveaux de journal requis certaines modifications apportées aux API qui utilisent des fournisseurs. Plus précisément, si les fournisseurs sont à l’aide de l’infrastructure de journalisation directement, cette modification peut interrompre qui utilisent. En outre, les fournisseurs qui utilisent l’infrastructure (qui est rendus public) à l’avenir devra dériver à partir de `LoggingDefinitions` ou `RelationalLoggingDefinitions`. Consultez les fournisseurs en mémoire pour obtenir des exemples SQL Server.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Les chaînes de ressources Core, relationnelle et Abstractions sont désormais publiques.
  * `CoreLoggerExtensions` et `RelationalLoggerExtensions` sont désormais publiques. Fournisseurs doivent utiliser ces API lors de la journalisation des événements qui sont définis au niveau relationnel ou core. N’accèdent pas directement ; aux ressources de journalisation Il s’agit toujours internes.
  * `IRawSqlCommandBuilder` changements apportés à partir d’un service singleton pour un service délimité
  * `IMigrationsSqlGenerator` changements apportés à partir d’un service singleton pour un service délimité
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * L’infrastructure pour la création de commandes relationnelles a été rendue publique afin qu’il peut être utilisé par les fournisseurs en toute sécurité et refactorisé légèrement.
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` changements apportés à partir d’un service délimité à un service temporaire
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` changements apportés à partir d’un service délimité à un service singleton
  * En outre, `ISingletonUpdateSqlGenerator` a été supprimé
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Beaucoup de code interne qui a été utilisé par les fournisseurs a maintenant été rendue publique
  * Il ne doit plus être nécessaire de faire référence à `IndentedStringBuilder` dans la mesure où il a été factorisé en dehors des emplacements qui la faisait
  * Utilisations de `NonCapturingLazyInitializer` doit être remplacé par `LazyInitializer` à partir de la BCL
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Cette modification est entièrement couverte dans l’application document modifications importantes. Pour les fournisseurs, cela peut être plus affecter, car le test EF core peut souvent amener à atteindre ce problème, afin de l’infrastructure de test a changé et vous que moins probable.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` a été simplifiée
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * StartsWith traduction a changé de manière fournisseurs se peuvent que vous souhaitez/devoir réagir
* https://github.com/aspnet/EntityFrameworkCore/pull/15168
  * Configurer les services de convention ont changé. Fournisseurs doivent désormais hériter de « ProviderConventionSet » ou « RelationalConventionSet ».
  * Personnalisations peuvent être ajoutées via `IConventionSetCustomizer` services, mais cela est destiné à être utilisé par d’autres extensions, pas les fournisseurs.
  * Conventions utilisées lors de l’exécution doivent être résolues à partir de `IConventionSetBuilder`.
* https://github.com/aspnet/EntityFrameworkCore/pull/15288 -Données d’amorçage a été refactorisé dans une API publique afin d’éviter la nécessité d’utiliser des types internes. Cela doit uniquement avoir un impact sur les fournisseurs non relationnelles, étant donné que l’amorçage est géré par la classe de base relationnelle pour tous les fournisseurs relationnelles.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Modifications de test uniquement

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Autoriser les séparateurs SQL personnalisables dans les tests
  * Tester les changements qui permettent des comparaisons de virgule flottante non strict dans BuiltInDataTypesTestBase
  * Modifications de test qui permettent des tests de requête à être réutilisée avec différents séparateurs SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Ajouter des tests de DbFunction pour les tests de spécification relationnelle
  * Telles que ces tests peuvent être exécutés sur tous les fournisseurs de base de données
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Nettoyage du test asynchrone
  * Supprimer `Wait` , les appels inutiles async et renommé certaines méthodes de test
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Unifier l’infrastructure de test de journalisation
  * Ajouté `CreateListLoggerFactory` et supprimé une infrastructure de journalisation précédente, nécessitant des fournisseurs à l’aide de ces tests pour réagir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) -Exécuter des tests de requête plus à la fois de façon synchrone et asynchrone
  * Noms de test et la factorisation a changé, ce qui nécessitera des fournisseurs à l’aide de ces tests pour réagir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Renommer les navigations dans le modèle ComplexNavigations
  * Fournisseurs à l’aide de ces tests peuvent devoir réagir
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Retourner le contexte pour le pool au lieu d’en cours de suppression dans les tests fonctionnels
  * Cette modification inclut une refactorisation de test qui peut nécessiter des fournisseurs de réagir

### <a name="test-and-product-code-changes"></a>Modifications de code de test et de produit

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Consolider RelationalTypeMapping.Clone méthodes
  * Les modifications dans 2.1 à la RelationalTypeMapping autorisées pour une simplification dans les classes dérivées. Nous pensons que cela endommageait aux fournisseurs, mais les fournisseurs peuvent tirer parti de cette modification dans leur type dérivé de la mapper des classes.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Les requêtes avec balises ou nommés
  * Ajoute l’infrastructure de marquage des requêtes LINQ et apparaissent sous forme de commentaires dans le code SQL de ces balises. Cela peut nécessiter des fournisseurs de réagir dans la génération SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -Prise en charge des données spatiales via NTS
  * Permet les mappages de types et membres traducteurs d’être inscrit en dehors du fournisseur
    * Les fournisseurs doivent appeler la base. FindMapping() dans leur implémentation ITypeMappingSource pour qu’il fonctionne
  * Suivez ce modèle pour ajouter la prise en charge spatiale pour votre fournisseur qui est cohérente entre les fournisseurs.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) -Ajouter un débogage amélioré pour la création de fournisseur de service
  * Permet de DbContextOptionsExtensions implémenter une nouvelle interface qui permet aux utilisateurs de comprendre pourquoi le fournisseur de services interne est en cours de nouveau généré
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Ajoute CanConnect API pour une utilisation par les contrôles d’intégrité
  * Cette demande de tirage ajoute le concept de `CanConnect` qui sera utilisé par le contrôle d’intégrité d’ASP.NET Core vérifie pour déterminer si la base de données est disponible. Par défaut, l’implémentation relationnelle appelle simplement `Exist`, mais les fournisseurs peuvent implémenter quelque chose de différent si nécessaire. Les fournisseurs non relationnelles devez implémenter la nouvelle API pour le contrôle d’intégrité être utilisable.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Mettez à jour de base RelationalTypeMapping pour ne définir pas DbParameter taille
  * Arrêter de taille par défaut dans la mesure où il peut provoquer la troncation. Fournisseurs devrez peut-être ajouter leur propre logique si la taille doit être définie.
* (https://github.com/aspnet/EntityFrameworkCore/pull/13372) -RevEng : Spécifiez toujours le type de colonne pour les colonnes décimales
  * Configurez toujours le type de colonne pour les colonnes décimales dans le code généré automatiquement, plutôt que de configurer par convention.
  * Les fournisseurs ne doivent pas nécessiter des modifications de leur côté.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Ajoute CaseExpression pour générer des expressions de cas de SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Ajoute la possibilité de spécifier des mappages de types sur SqlFunctionExpression afin d’améliorer l’inférence de type de magasin d’arguments et résultats.
