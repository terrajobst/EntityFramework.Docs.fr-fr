---
title: Les versions précédentes de Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
ms.openlocfilehash: 4fa27f8259ecc011d9d30080aee3c44353ef533d
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283990"
---
# <a name="past-releases-of-entity-framework"></a>Les versions précédentes de Entity Framework

La première version d’Entity Framework a été publiée en 2008, dans le cadre de .NET Framework 3.5 SP1 et Visual Studio 2008 SP1.

Depuis la version EF4.1, elle est disponible en tant que le [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/) -actuellement un des packages plus populaires sur NuGet.org.

Entre les versions 4.1 et 5.0, le package EntityFramework NuGet étendu les bibliothèques d’EF partie intégrante de .NET Framework.   

À compter de la version 6, EF est devenu un projet open source et également déplacé complètement hors écran de la bande du .NET Framework.
Cela signifie que lorsque vous ajoutez le package NuGet de version 6 EntityFramework à une application, vous obtenez une copie complète de la bibliothèque d’EF qui ne repose pas sur les bits d’EF qui font partie du .NET Framework.
Cela a permis de quelque peu accélérer le rythme de développement et de fourniture de nouvelles fonctions.

En juin 2016, nous avons publié d’EF Core 1.0. EF Core est basé sur une nouvelle base de code et est conçu comme une version plus légère et extensible d’EF.
Actuellement, EF Core est le principal objectif du développement de l’équipe Entity Framework Microsoft.
Cela ne signifie aucune nouvelle fonctionnalité majeure prévue pour EF6. Toutefois EF6 est conservée en tant qu’un projet open source et un produit Microsoft pris en charge.

Voici la liste des versions antérieures, dans l’ordre chronologique inverse, avec plus d’informations sur les nouvelles fonctionnalités qui ont été introduites dans chaque version.

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 runtime a été mis à NuGet en octobre 2015.
Cette version contient uniquement les correctifs pour les défauts de haute priorité et régressions signalées sur le 6.1.2 mise en production.
Les correctifs sont les suivantes :

- Requête : La régression dans EF 6.1.2 : OUTER APPLY introduit et des requêtes plus complexes pour les relations 1:1 et la clause « let »
- Problème TPT avec masquage de propriété de classe de base dans la classe héritée
- DbMigration.Sql échoue lorsque le mot « OK » est contenu dans le texte
- Créer des indicateur de compatibilité pour UnionAll insère et Intersect APLANISSEMENT prise en charge
- Requête avec plusieurs inclut ne fonctionne pas dans 6.1.2 (travail en 6.1.1)
- « Vous avez une erreur dans votre syntaxe SQL » une exception après la mise à niveau à partir d’Entity Framework 6.1.1 à 6.1.2

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 runtime a été mis à NuGet en décembre 2014.
Cette version concerne principalement des correctifs de bogues. Que nous avons choisi également quelques modifications importantes des membres de la Communauté :
- **Paramètres de cache de requête peuvent être configurés à partir du fichier app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Méthodes SqlFile et SqlResource sur DbMigration** vous permettent d’exécuter SQL script stocké dans un fichier ou ressource incorporée.

## <a name="ef-611"></a>ENTITY FRAMEWORK 6.1.1
L’Entity Framework 6.1.1 runtime a été mis à NuGet en juin 2014.
Cette version contient des correctifs pour un nombre de personnes ont rencontré des problèmes. Entre autres :
- Designer : Erreur l’ouverture d’EF5 edmx avec une précision décimale dans le Concepteur d’EF6
- Logique de détection d’instance par défaut pour la base de données locale ne fonctionne pas avec SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 runtime a été mis à NuGet en mars 2014.
Cette mise à jour mineure comprend un nombre important de nouvelles fonctionnalités :

- **Outils de consolidation** fournit un moyen cohérent de créer un nouveau modèle EF. Cette fonctionnalité [étend l’Assistant d’ADO.NET Entity Data Model pour prendre en charge la création de modèles de Code First](~/ef6/modeling/code-first/workflows/existing-database.md), y compris ingénierie à rebours à partir d’une base de données existante. Ces fonctionnalités étaient précédemment disponibles dans la qualité bêta les EF Power Tools.
- **[Gestion des échecs de validation de transaction](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  fournit le CommitFailureHandler qui utilise la capacité récemment introduite pour intercepter les opérations de transaction. Le CommitFailureHandler permet la récupération automatique à partir d’échecs de connexion lors de la validation d’une transaction.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  autorise les index de la définir en plaçant un `[Index]` attribut sur une propriété (ou propriétés) dans votre modèle Code First. Code tout d’abord crée ensuite un index correspondant dans la base de données.
- **L’API de mappage publique** fournit l’accès aux informations EF a sur la façon dont les propriétés et les types sont mappés aux colonnes et tables dans la base de données. Dans les versions précédentes cette API a été interne.
- **[Possibilité de configurer des intercepteurs via le fichier App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  permet des intercepteurs doit être ajouté sans recompiler l’application.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**est un intercepteur de qui facilite la consigner toutes les opérations de base de données dans un fichier. En association avec la fonctionnalité précédente, cela vous permet de facilement [basculer sur la journalisation des opérations de base de données pour une application déployée](~/ef6/fundamentals/configuring/config-file.md), sans avoir à recompiler.
- **Détection de modification de modèle de migrations** a été améliorée afin que les migrations généré automatiquement sont plus précises ; les performances du processus de détection de modification a également été améliorées.
- **Améliorations des performances** y compris les opérations de base de données réduites lors de l’initialisation, les optimisations pour la comparaison d’égalité de valeur null dans les requêtes LINQ, afficher plus rapidement génération (la création de modèles) dans d’autres scénarios et plus efficace matérialisation d’entités suivies avec plusieurs associations.

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 runtime a été publié sur NuGet en décembre 2013.
Cette version du correctif est limitée à la résolution des problèmes qui ont été introduites dans la version d’EF6 (régressions de performances/comportement depuis EF5).

## <a name="ef-601"></a>EF 6.0.1
La version EF 6.0.1 runtime a été publié en octobre 2013 simultanément avec EF 6.0.0, à NuGet, car ce dernier a été incorporé dans une version de Visual Studio qui a verrouillé la liste des quelques mois avant.
Cette version du correctif est limitée à la résolution des problèmes qui ont été introduites dans la version d’EF6 (régressions de performances/comportement depuis EF5).
Les modifications les plus importants ont été pour résoudre des problèmes de performances pendant la mise en route pour les modèles EF.
Il était important car les performances de mise en route sont une zone d’intérêt dans EF6 et ces problèmes ont été négation certains autres gains de performance apportées dans EF6.

## <a name="ef-60"></a>ENTITY FRAMEWORK 6.0
EF 6.0.0 runtime a été publié à NuGet en octobre 2013.
C’est la première version dans laquelle un runtime EF complet est inclus dans le [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/) qui ne repose pas sur les bits d’EF qui font partie du .NET Framework.
Déplacer les parties restantes du runtime pour le package NuGet un nombre requis de modification pour le code existant avec rupture.
Consultez la section sur [mise à niveau vers Entity Framework 6](upgrading-to-ef6.md) pour plus d’informations sur les étapes manuelles requises pour mettre à niveau.

Cette version inclut de nombreuses nouvelles fonctionnalités.
Les fonctionnalités suivantes fonctionnent pour les modèles créés avec Code First ou le Concepteur EF :

- **[Requête asynchrone et enregistrement](~/ef6/fundamentals/async.md)**  ajoute la prise en charge pour les modèles asynchrones basé sur des tâches qui ont été introduites dans .NET 4.5.
- **[Résilience des connexions](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  permet la récupération automatique à partir d’échecs de connexion temporaires.
- **[Configuration basée sur le code](~/ef6/fundamentals/configuring/code-based.md)**  vous donne la possibilité d’effectuer la configuration – traditionnellement exécutée dans un fichier de configuration : dans le code.
- **[Résolution des dépendances](~/ef6/fundamentals/configuring/dependency-resolution.md)**  introduit la prise en charge du localisateur de Service de modèle et nous avons pris en compte certains éléments de fonctionnalité qui peut être remplacée par des implémentations personnalisées.
- **[Journalisation de l’interception/SQL](~/ef6/fundamentals/logging-and-interception.md)**  fournit les blocs de construction de bas niveau pour l’interception des opérations d’EF avec la journalisation SQL simple basée sur le.
- **Améliorations de testabilité** rendent plus facile de créer des doubles de test pour DbContext et DbSet lorsque [à l’aide d’une infrastructure de simulation](~/ef6/fundamentals/testing/mocking.md) ou [écrire votre propre des doubles de test](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext peut maintenant être créé avec une DbConnection qui est déjà ouvert](~/ef6/fundamentals/connection-management.md)**  qui permet des scénarios où il peut s’avérer utile si la connexion a pu être ouverte lors de la création du contexte (tels que le partage d’une connexion entre les composants où Vous ne pouvez pas garantir l’état de la connexion).
- **[Amélioré la prise en charge de la Transaction](~/ef6/saving/transactions.md)**  prend en charge pour une transaction externe pour le framework, ainsi que les méthodes améliorées de la création d’une transaction dans le cadre.
- **Énumérations, Spatial et optimiser les performances de .NET 4.0** - en déplaçant les principaux composants qui étaient dans le .NET Framework dans le package NuGet d’EF nous sommes maintenant en mesure d’offrir une prise en charge de l’enum, les types de données spatiales et les améliorations de performances d’EF5 sur .NET 4.0.
- **Amélioration des performances de Enumerable.Contains dans les requêtes LINQ**.
- **Préchauffage améliorée de temps (génération d’affichages)**, en particulier pour les modèles volumineux.
- **Pluralisation enfichable &amp; singularisation Service**.
- **Les implémentations personnalisées de Equals ou GetHashCode** entity classes sont maintenant pris en charge.
- **DbSet.AddRange/RemoveRange** fournit une manière optimisée pour ajouter ou supprimer plusieurs entités à partir d’un ensemble.
- **Pratique DbChangeTracker.HasChanges** offre un moyen facile et efficace pour voir s’il existe des modifications en attente pour être enregistrés dans la base de données.
- **Apporté SqlCeFunctions** fournit un SQL Compact équivalent aux SqlFunctions.

Les fonctionnalités suivantes s’appliquent uniquement à Code First :

- **[Conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md)**  autoriser écrire vos propres conventions afin d’éviter de configuration répétitive. Nous fournissons une API simple pour les conventions légères, ainsi que certains blocs de construction plus complexes afin que vous puissiez créer des conventions plus complexes.
- **[Code du premier mappage à des procédures stockées Insert/Update/Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  est désormais pris en charge.
- **[Scripts de migrations idempotent](~/ef6/modeling/code-first/migrations/index.md)**  vous permettent de générer un script SQL peut être mis à niveau une base de données à n’importe quelle version jusqu'à la dernière version.
- **[Table d’historique de Migrations configurable](~/ef6/modeling/code-first/migrations/history-customization.md)**  vous permet de personnaliser la définition de la table d’historique de migrations. Cela est particulièrement utile pour les fournisseurs de base de données qui nécessitent des types de données appropriés etc. soit spécifiée pour la table d’historique de Migrations de fonctionner correctement.
- **Plusieurs contextes par base de données** supprime la limite précédente d’un modèle de Code First par base de données lors de l’utilisation de Migrations ou lorsque Code First crée automatiquement la base de données pour vous.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  est une nouvelle API Code First qui permet le schéma de base de données par défaut pour un modèle Code First être configuré au même endroit. Le schéma par défaut le premier Code était auparavant codées en dur pour &quot;dbo&quot; et la seule façon de configurer le schéma auquel appartenance une table a été via l’API ToTable.
- **Méthode de DbModelBuilder.Configurations.AddFromAssembly** vous permet d’ajouter facilement toutes les classes de configuration définies dans un assembly lorsque vous utilisez des classes de configuration avec l’API Fluent de Code First.
- **[Les opérations de Migrations personnalisées](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  vous permettait d’ajouter des opérations supplémentaires à utiliser dans vos migrations en code.
- **Niveau d’isolation de transaction par défaut est modifié à READ_COMMITTED_SNAPSHOT** pour les bases de données créées à l’aide de Code First, ce qui permet une meilleure extensibilité et moins de blocages.
- **Entité et les types complexes peuvent maintenant être nestedinside classes**. |

## <a name="ef-50"></a>ENTITY FRAMEWORK 5.0
EF 5.0.0 runtime a été publié sur NuGet en août de 2012.
Cette version introduit de nouvelles fonctionnalités, y compris la prise en charge de l’enum, les fonctions table, les types de données spatiales et diverses améliorations des performances.

Entity Framework Designer dans Visual Studio 2012 introduit également la prise en charge pour les diagrammes de plusieurs par modèle, la coloration des formes lors de l’importation de surface et le lot de conception de procédures stockées.

Voici une liste de contenu que nous avons rassemblées spécialement pour la version d’EF 5.

-   [EF 5 version Post](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nouvelles fonctionnalités dans EF5
    -   [Enum prennent en charge dans le Code tout d’abord](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Prise en charge de l’enum dans Entity Framework Designer](~/ef6/modeling/designer/data-types/enums.md)
    -   [Types de données spatiales dans le Code tout d’abord](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Types de données spatiales dans Entity Framework Designer](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Prise en charge de fournisseur pour les Types spatiaux](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Fonctions table](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Plusieurs diagrammes par modèle](~/ef6/modeling/designer/multiple-diagrams.md)
-   Configuration de votre modèle
    -   [Création d'un modèle](~/ef6/modeling/index.md)
    -   [Connexions et des modèles](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Considérations sur les performances](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Utilisation de Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Fichier de configuration](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glossaire](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First pour une base de données (procédure pas à pas et vidéo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First pour une base de données existante (procédure pas à pas et vidéo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Conventions](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Annotations de données](~/ef6/modeling/code-first/data-annotations.md)
        -   [API Fluent - configuration/mappage des Types de & Propriétés](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [API Fluent - configuration des relations](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [API Fluent avec VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Automatique de Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Définition de DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   EF Designer
        -   [Premier modèle (procédure pas à pas et vidéo)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (procédure pas à pas et vidéo)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Types complexes](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Associations/relations](~/ef6/modeling/designer/relationships.md)
        -   [Modèle d’héritage TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Modèle d’héritage TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Requête avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procédures stockées avec plusieurs jeux de résultats](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Insérer, mettre à jour et supprimer avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapper une entité à plusieurs Tables (fractionnement d’entité)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapper plusieurs entités à une Table (fractionnement de Table)](~/ef6/modeling/designer/table-splitting.md)
        -   [Définir des requêtes](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Modèles de génération de code](~/ef6/modeling/designer/codegen/index.md)
        -   [Retour à ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   À l’aide de votre modèle
    -   [Utilisation de DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Entités de recherche/interrogation](~/ef6/querying/index.md)
    -   [Utilisation des relations](~/ef6/fundamentals/relationships.md)
    -   [Chargement des entités connexes](~/ef6/querying/related-data.md)
    -   [Utilisation des données locales](~/ef6/querying/local-data.md)
    -   [Applications multicouches](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Requêtes SQL brutes](~/ef6/querying/raw-sql.md)
    -   [Modèles d’accès concurrentiel optimiste](~/ef6/saving/concurrency.md)
    -   [Utilisation de proxys](~/ef6/fundamentals/proxies.md)
    -   [Détecter des modifications automatique](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Pas de suivi des requêtes](~/ef6/querying/no-tracking.md)
    -   [Méthode Load](~/ef6/querying/load-method.md)
    -   [Ajouter et d’attachement et les États de l’entité](~/ef6/saving/change-tracking/entity-state.md)
    -   [Utilisation de valeurs de propriété](~/ef6/saving/change-tracking/property-values.md)
    -   [Liaison de données avec WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Liaison de données avec WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 runtime a été publié sur NuGet en février 2012 peu après EF 4.3.0.
Cette version du correctif inclus certains correctifs de bogues vers la version EF 4.3 et a introduit une meilleure prise en charge de base de données locale pour les clients qui utilisent EF 4.3 avec Visual Studio 2012.

Voici une liste de contenu que nous avons rassemblés spécifiquement pour à la version EF 4.3.1, la plupart du contenu fourni pour EF 4.1 s’applique toujours à EF 4.3 également.

-   [Billet de Blog de version EF 4.3.1](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 runtime a été publié sur NuGet en février de 2012.
Cette version comprenait la nouvelle fonctionnalité de Migrations Code First qui permet à une base de données créée par Code First à modifier progressivement à mesure que votre modèle Code First évolue.

Voici une liste de contenu que nous avons rassemblées spécialement pour la version EF 4.3, la plupart du contenu fourni pour EF 4.1 s’applique toujours à EF 4.3 ainsi :
-   [EF version 4.3 Post](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Procédure pas à pas EF 4.3 Migrations basée sur le Code](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Procédure pas à pas EF 4.3 Migrations automatiques](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>ENTITY FRAMEWORK 4.2
EF 4.2.0 runtime a été publié sur NuGet en novembre 2011.
Cette version inclut des correctifs de bogues pour EF 4.1.1 mise en production.
Étant donné que cette version est uniquement inclus des correctifs de bogues qu’il aurait pu être EF 4.1.2 patch version, mais nous avons opté pour déplacer à 4.2 pour nous permettre d’éloigner la date en fonction des numéros de version de correctif nous avons utilisé dans le 4.1.x libère et adoptent la [sémantique Versionsing](https://semver.org) standard pour le contrôle de version sémantique.

Voici une liste de contenu que nous avons rassemblées spécialement pour la version 4.2 d’EF, le contenu fourni pour EF 4.1 s’applique toujours à EF 4.2 ainsi.

-   [EF version 4.2 Post](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Première procédure pas à pas de code](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Procédure pas à pas premier modèle de & base de données](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 runtime a été publié sur NuGet en juillet 2011.
En plus des correctifs de bogues, cette version du correctif introduit certains composants pour le rendre plus facile pour le moment de la conception des outils pour travailler avec un modèle Code First.
Ces composants sont utilisés par les Migrations Code First (inclus dans EF 4.3) et les outils EF Power Tools.

Vous remarquerez que la version étrange numéro 4.1.10715 du package.
Nous avons utilisé pour utiliser les versions des correctifs en fonction de date avant que nous avons décidé d’adopter [Semver](https://semver.org).
Considérez cette version comme EF 4.1 patch 1 (ou EF 4.1.1).

Voici une liste de contenu que nous avons rassemblées pour le 4.1.1 mise en production :

-   [EF 4.1.1 version Post](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>ENTITY FRAMEWORK 4.1
EF 4.1.10331 runtime a été le premier à être publiés sur NuGet, en avril 2011.
Cette version comprenait l’API DbContext simplifiée et le workflow de Code First.

Vous remarquerez le numéro de version étrange, 4.1.10331, ce qui aurait dû être 4.1. En outre, il existe un 4.1.10311 version qui aurait dû être 4.1.0-rc (le 'rc' signifie « version finale »).
Nous avons utilisé pour utiliser les versions des correctifs en fonction de date avant que nous avons décidé d’adopter [Semver](https://semver.org).

Voici une liste de contenu que nous avons rassemblées pour la version 4.1. Une grande partie de celui-ci s’applique toujours à des versions ultérieures d’Entity Framework :

-   [EF version 4.1 Post](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Première procédure pas à pas de code](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Procédure pas à pas premier modèle de & base de données](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure fédérations et Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Cette version a été incluse dans .NET Framework 4 et Visual Studio 2010, en avril 2010.
Nouvelles fonctionnalités importantes dans cette version incluent la prise en charge POCO, mappage de clé étrangère, le chargement différé, les améliorations de testabilité, génération de code personnalisables et le flux de travail Model First.

Bien qu’il était la deuxième version d’Entity Framework, il appelait EF 4 pour s’aligner avec la version de .NET Framework fourni avec.
Après cette version, nous avons démarré la disposition d’Entity Framework sur NuGet et adopté le contrôle de version sémantique dans la mesure où nous n’étions n’est plus liés à la Version du .NET Framework.

Notez que certaines versions ultérieures du .NET Framework ont livré avec des mises à jour importantes pour les bits d’EF inclus.
En fait, la plupart des nouvelles fonctionnalités d’Entity Framework 5.0 ont été implémentés en tant que des améliorations sur ces bits.
Toutefois, afin de rationaliser l’histoire de contrôle de version pour Entity Framework, nous continuons faire référence aux bits EF qui font partie du .NET Framework en tant que le runtime EF 4.0, tandis que toutes les versions plus récentes sont constitués de la [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
La version initiale d’Entity Framework a été incluse dans .NET 3.5 Service Pack 1 et Visual Studio 2008 SP1, publiée en août de 2008.
Cette version fournie base O/RM prise en charge à l’aide de la première base de données flux de travail.
