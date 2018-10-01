---
title: Considérations relatives aux performances pour EF4, EF5 et EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: fb184fe8720b552a2050607bb17648f0413c31d1
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459589"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Considérations relatives aux performances pour Entity Framework 4, 5 et 6
Par David Obando, Eric Dettinger et autres

Date de publication : Avril 2012

Dernière mise à jour : mai 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Introduction

Infrastructures de mappage objet / relationnel sont un moyen pratique pour fournir une abstraction pour accéder aux données dans une application orientée objet. Pour les applications .NET, recommandée par Microsoft Qu'o/RM est Entity Framework. Cependant, avec n’importe quel abstraction performances peuvent devenir un problème.

Ce livre blanc a été écrit pour afficher les considérations de performances lors du développement d’applications à l’aide d’Entity Framework, les développeurs d’estimer les algorithmes internes Entity Framework qui peut affecter les performances et fournissent des conseils pour l’investigation et amélioration des performances dans les applications qui utilisent Entity Framework. Il existe un nombre de bons sujets sur les performances déjà disponibles sur le web, et nous avons également essayé pointant vers ces ressources lorsque cela est possible.

Les performances sont un sujet complexe. Ce livre blanc constitue une ressource pour aider à vous apporter des performances relatives des décisions pour vos applications qui utilisent Entity Framework. Nous avons inclus des mesures de test pour illustrer les performances, mais ces mesures ne sont pas destinées comme indicateurs absolus des performances que vous voyez dans votre application.

Pour des raisons pratiques, ce document part du principe que Entity Framework 4 est exécuté sous .NET 4.0 et Entity Framework 5 et 6 sont exécutés sous le .NET 4.5. De nombreuses améliorations de performances apportées pour Entity Framework 5 résident dans les principaux composants fournis avec le .NET 4.5.

Entity Framework 6 est une version de la bande à l’emploi et ne repose pas sur les composants d’Entity Framework fournis avec .NET. Entity Framework 6 fonctionnent sur .NET 4.0 et 4.5 de .NET et peut offrir un gain de performances big à ceux qui n’ont pas mis à niveau à partir de .NET 4.0 mais souhaitez que les derniers bits d’Entity Framework dans leur application. Lorsque ce document mentionne Entity Framework 6, il fait référence à la dernière version disponible au moment de la rédaction : version 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Visual Studio à froid. Exécution des requêtes à chaud

La première fois que les requêtes sont envoyées à un modèle donné, Entity Framework effectue beaucoup de travail en arrière-plan pour charger et valider le modèle. Souvent, nous faisons référence à cette première requête sous forme de requête « froid ».  Les requêtes supplémentaires sur un modèle déjà chargé sont appelées requêtes « à chaud » et sont beaucoup plus rapides.

Prenons une vue d’ensemble de la répartition du temps lors de l’exécution d’une requête à l’aide d’Entity Framework et voyons où les choses sont amélioration dans Entity Framework 6.

**Première exécution de requête – requête à froid**

| Code utilisateur écrit                                                                                     | Action                    | EF4 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Création du contexte          | Moyenne                                                                                                                                                                                                                                                                                                                                                                                                                        | Moyenne                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Création d’expression de requête | Faible                                                                                                                                                                                                                                                                                                                                                                                                                           | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Exécution des requêtes LINQ      | -Chargement des métadonnées : haute, mais il est mis en cache <br/> -Afficher la génération : potentiellement très élevé, mais mis en cache <br/> -Évaluation du paramètre : moyenne <br/> -Traduction de requête : moyenne <br/> -Génération de matérialiseur : taille moyenne, mais il est mis en cache <br/> -Exécution de la requête de base de données : potentiellement élevé <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne <br/> -Recherche de identity : moyenne | -Chargement des métadonnées : haute, mais il est mis en cache <br/> -Afficher la génération : potentiellement très élevé, mais mis en cache <br/> -Évaluation du paramètre : faible <br/> -Traduction de requête : taille moyenne, mais il est mis en cache <br/> -Génération de matérialiseur : taille moyenne, mais il est mis en cache <br/> -Exécution de la requête de base de données : potentiellement élevé (requêtes dans certaines situations de meilleures) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne <br/> -Recherche de identity : moyenne | -Chargement des métadonnées : haute, mais il est mis en cache <br/> -Afficher la génération : taille moyenne, mais il est mis en cache <br/> -Évaluation du paramètre : faible <br/> -Traduction de requête : taille moyenne, mais il est mis en cache <br/> -Génération de matérialiseur : taille moyenne, mais il est mis en cache <br/> -Exécution de la requête de base de données : potentiellement élevé (requêtes dans certaines situations de meilleures) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne (Faster à EF5) <br/> -Recherche de identity : moyenne |
| `}`                                                                                                  | Connection.Close, mais          | Faible                                                                                                                                                                                                                                                                                                                                                                                                                           | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Exécution de la deuxième requête – requête urgente**

| Code utilisateur écrit                                                                                     | Action                    | EF4 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Impact sur les performances                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Création du contexte          | Moyenne                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Moyenne                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Création d’expression de requête | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Exécution des requêtes LINQ      | -Métadonnées ~~chargement~~ recherche : ~~élevée mais la mise en cache~~ faible <br/> -Afficher ~~génération~~ recherche : ~~potentiellement très élevé, mais mis en cache~~ faible <br/> -Évaluation du paramètre : moyenne <br/> -Requête ~~traduction~~ recherche : moyenne <br/> -Matérialiseur ~~génération~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Exécution de la requête de base de données : potentiellement élevé <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne <br/> -Recherche de identity : moyenne | -Métadonnées ~~chargement~~ recherche : ~~élevée mais la mise en cache~~ faible <br/> -Afficher ~~génération~~ recherche : ~~potentiellement très élevé, mais mis en cache~~ faible <br/> -Évaluation du paramètre : faible <br/> -Requête ~~traduction~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Matérialiseur ~~génération~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Exécution de la requête de base de données : potentiellement élevé (requêtes dans certaines situations de meilleures) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne <br/> -Recherche de identity : moyenne | -Métadonnées ~~chargement~~ recherche : ~~élevée mais la mise en cache~~ faible <br/> -Afficher ~~génération~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Évaluation du paramètre : faible <br/> -Requête ~~traduction~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Matérialiseur ~~génération~~ recherche : ~~moyenne, mais il est mis en cache~~ faible <br/> -Exécution de la requête de base de données : potentiellement élevé (requêtes dans certaines situations de meilleures) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Matérialisation d’objet : moyenne (Faster à EF5) <br/> -Recherche de identity : moyenne |
| `}`                                                                                                  | Connection.Close, mais          | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Faible                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Il existe plusieurs façons de réduire le coût de performances des requêtes à froid et à chaud, et nous allons examiner ces éléments dans la section suivante. Plus précisément, nous allons examiner ce qui réduit le coût du modèle de chargement dans les requêtes à froid à l’aide des vues prégénérées, qui devraient aider à atténuer les problèmes de performances rencontrés au cours de la génération de vues. Pour les requêtes à chaud, nous aborderons la mise en cache du plan de requête, aucune requête de suivi et options d’exécution de requête différents.

### <a name="21-what-is-view-generation"></a>2.1 Nouveautés génération de vues

Pour comprendre le mode d’affichage génération est, nous devons tout d’abord comprendre quelles sont les « Mappage de vues ». Mappage des vues sont des représentations exécutables des transformations spécifiées dans le mappage pour chaque jeu d’entités et les associations. En interne, ces affichages de mappage de prennent la forme de CQTs (arborescences canoniques requête). Il existe deux types de vues de mappage :

-   Interrogez les vues : elles représentent la transformation nécessaire pour accéder à partir du schéma de base de données au modèle conceptuel.
-   Mettre à jour des vues : elles représentent la transformation nécessaire pour accéder à partir du modèle conceptuel au schéma de base de données.

N’oubliez pas que le modèle conceptuel peut différer le schéma de base de données de différentes manières. Par exemple, une seule table peut être utilisée pour stocker les données pour les deux différents types d’entité. L’héritage et les mappages non triviale jouent un rôle dans la complexité des vues de mappage.

Le processus de l’informatique ces vues basées sur la spécification du mappage est ce que nous appelons la génération de vues. Génération de vues peut soit prendre place dynamiquement lorsqu’un modèle est chargé, ou au moment de la build, à l’aide de « vues prégénérées » ; ce dernier est sérialisé sous la forme d’instructions SQL de l’entité pour un C\# ou fichier VB.

Lorsque les vues sont générées, elles sont également validés. À partir d’un point de vue performances, la grande majorité du coût de la génération de vues est en fait la validation des vues qui garantit que les connexions entre les entités de sens et la cardinalité correcte pour toutes les opérations prises en charge.

Lorsqu’une requête sur un jeu d’entités est exécutée, la requête est associée à l’affichage de requête correspondant, et le résultat de cette composition est exécuté par le compilateur de plan pour créer la représentation sous forme de la requête que le magasin de stockage peut comprendre. Pour SQL Server, le résultat final de cette compilation sera une instruction T-SQL SELECT. La première fois qu’une mise à jour sur un jeu d’entités est effectuée, la vue de la mise à jour est exécutée via un processus similaire pour le transformer en instructions DML pour la base de données cible.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 les facteurs qui affectent les performances de génération de vues

Les performances de l’étape de génération de vue dépendent non seulement sur la taille de votre modèle, mais également sur interconnectés comment le modèle est. Si les deux entités sont connectées via une chaîne d’héritage ou d’une Association, on dit d’être connecté. De même, si les deux tables sont connectées via une clé étrangère, ils sont connectés. Comme le nombre des entités et des tables dans vos schémas connectés augmente, la génération de vues coûts augmentent.

L’algorithme que nous utilisons pour générer et valider des vues est exponentielle dans le pire des cas, bien que nous utilisons certaines optimisations d’améliorer cela. Les principaux facteurs qui semblent affecter négativement les performances sont :

-   Taille du modèle, qui fait référence au nombre d’entités et la quantité des associations entre ces entités.
-   Modèle complexité, en particulier l’héritage impliquant un grand nombre de types.
-   À l’aide des Associations indépendantes, au lieu des Associations de clé étrangère.

Pour les modèles simples, le coût peut être assez petit pour ne dérange pas à l’aide de vues prégénérées. Comme taille de modèle et la complexité augmentent, plusieurs options sont disponibles pour réduire le coût de la génération de vues et de validation.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>Temps de chargement de 2,3 utilisation Pre-Generated des affichages pour réduire le modèle

Pour plus d’informations sur l’utilisation des vues prégénérées dans Entity Framework 6, visitez [Pre-Generated mappage des vues](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 vues prégénérées à l’aide d’Entity Framework Power Tools Community Edition

Vous pouvez utiliser la [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) pour générer des vues de modèles EDMX et Code First en double-cliquant sur le fichier de classe de modèle en utilisant le menu d’Entity Framework pour sélectionner « Générer des vues ». Entity Framework Power Tools Community Edition fonctionnent uniquement sur les contextes de DbContext dérivée.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 comment utiliser les vues prégénérées avec un modèle créé par EDMGen

EDMGen est un utilitaire qui est livré avec .NET et fonctionne avec Entity Framework 4 et 5, mais pas avec Entity Framework 6. EDMGen vous permet de générer un fichier de modèle, la couche objet et les vues à partir de la ligne de commande. Une des sorties sera un fichier de vues dans le langage de votre choix, Visual Basic ou C\#. Il s’agit d’un fichier de code contenant des extraits de code Entity SQL pour chaque jeu d’entités. Pour activer les vues prégénérées, vous incluez simplement le fichier dans votre projet.

Si vous effectuez manuellement des modifications pour les fichiers de schéma pour le modèle, vous devez générer de nouveau le fichier de vues. Vous pouvez faire en cours d’exécution EDMGen avec le **/mode:ViewGeneration** indicateur.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 et comment utiliser les vues Pre-Generated avec un fichier EDMX

EDMGen permet également de générer des vues pour un fichier EDMX : la rubrique MSDN mentionnée précédemment décrit comment ajouter un événement pré-build pour ce faire, mais cela est compliqué, et il existe certains cas où il n’est pas possible. Il est généralement plus facile à utiliser un modèle T4 pour générer les vues lorsque votre modèle est dans un fichier edmx.

Blog de l’équipe ADO.NET a une requête post qui explique comment utiliser un modèle T4 pour la génération de vues ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Cet article inclut un modèle qui peut être téléchargé et ajouté à votre projet. Le modèle a été écrit pour la première version d’Entity Framework, afin qu’ils ne sont pas garantis pour fonctionner avec les dernières versions d’Entity Framework. Toutefois, vous pouvez télécharger un ensemble plus à jour de modèles de génération de vue pour Entity Framework 4 et 5from la galerie Visual Studio :

-   VB.NET : \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Si vous utilisez Entity Framework 6 vous pouvez obtenir l’affichage de modèles de génération T4 à partir de la galerie Visual Studio à \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 en réduisant le coût de la génération de vues

À l’aide de vues prégénérées déplace le coût de la génération de vues à partir du modèle de chargement (exécution) au moment du design. Bien que cela améliore les performances de démarrage lors de l’exécution, vous rencontrez toujours le problème de génération de vues lors du développement. Il existe plusieurs astuces supplémentaires qui peuvent aider à réduire le coût de la génération de vues, à la fois au moment de la compilation et le moment de l’exécution.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 à l’aide des Associations de clé étrangère afin de réduire le coût de la génération de vue

Nous avons vu un nombre de cas où la commutation les associations dans le modèle à partir des Associations indépendantes pour les Associations de clé étrangère considérablement amélioré le temps passé dans la génération de vues.

Pour illustrer cette amélioration, nous avons généré deux versions du modèle Navision à l’aide de EDMGen. *Remarque : seeappendix Cfor une description du modèle Navision.* Le modèle Navision est intéressant pour cet exercice en raison de sa très grande quantité d’entités et relations entre eux.

Une seule version de ce modèle de très grande taille a été générée avec les Associations de clés étrangères et l’autre a été générée avec les Associations indépendantes. Nous avons ensuite a dépassé le délai temps nécessaire pour générer les vues pour chaque modèle. Test de Framework5 d’entité utilisé la méthode GenerateViews() à partir de la classe EntityViewGenerator pour générer les vues, tandis que le test d’Entity Framework 6 a utilisé la méthode GenerateViews() à partir de la classe StorageMappingItemCollection. Cela est dû à la restructuration de code qui s’est produite dans la base de code Entity Framework 6.

Génération de vues pour le modèle comportant des clés étrangères dans une machine de laboratoire à l’aide d’Entity Framework 5, a pris les 65 minutes. Inconnu la durée pendant laquelle il aurait fallu pour générer des vues pour le modèle qui a utilisé des associations indépendantes. Nous l’avons laissé le test en cours d’exécution pour plus d’un mois avant que l’ordinateur a été redémarré dans notre laboratoire pour installer les mises à jour mensuelles.

Génération de vues pour le modèle comportant des clés étrangères dans le même ordinateur de laboratoire à l’aide d’Entity Framework 6, a pris les 28 secondes. Génération de vues pour le modèle qui utilise les Associations indépendantes a duré 58 secondes. Les améliorations effectuées vers Entity Framework 6 sur son code de génération de vue signifient que de nombreux projets n’aurez pas besoin des vues prégénérées pour obtenir des temps de démarrage plus rapides.

Il est important de Remarque pré-générer des affichages dans Entity Framework 4 et 5 est possible avec EDMGen ou Entity Framework Power Tools. Pour afficher d’Entity Framework 6 génération peut être effectuée via Entity Framework Power Tools ou par programme, comme décrit dans [Pre-Generated mappage de vues](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 comment utiliser des clés étrangères au lieu des Associations indépendantes

Lorsque vous utilisez EDMGen ou le Concepteur d’entités dans Visual Studio, vous obtenez les clés étrangères par défaut, et il suffit d’un simple indicateur de case à cocher ou de la ligne de commande pour basculer entre les clés étrangères et IAs.

Si vous avez un grand modèle Code First, à l’aide des Associations indépendantes aura le même effet sur la génération de vues. Vous pouvez éviter cet impact en incluant des propriétés de clé étrangère sur les classes pour vos objets dépendants, bien que certains développeurs considère cette option pour être polluer leur modèle d’objet. Vous trouverez plus d’informations sur ce sujet dans \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Lors de l’utilisation      | Procédez comme suit                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concepteur d'entités | Après avoir ajouté une association entre deux entités, assurez-vous que vous avez une contrainte référentielle. Contraintes référentielles indiquent à Entity Framework d’utiliser des clés étrangères au lieu d’Associations indépendantes. Pour plus d’informations, consultez \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Lorsque vous utilisez EDMGen pour générer vos fichiers à partir de la base de données, vos clés étrangères sont respectées et ajoutés au modèle en tant que tel. Pour plus d’informations sur les différentes options exposées par EDMGen visitez [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Consultez la section « Convention relation » de la [Conventions Code First](~/ef6/modeling/code-first/conventions/built-in.md) rubrique pour plus d’informations sur la façon d’inclure des propriétés de clé étrangère sur les objets dépendants lors de l’utilisation de Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 déplacement de votre modèle à un assembly distinct

Lorsque votre modèle est inclus directement dans le projet de votre application et que vous générez des vues via un événement pré-build ou un modèle T4, génération de vues et la validation aura lieu chaque fois que le projet est régénéré, même si le modèle n’a pas été modifié. Si vous déplacez le modèle à un assembly distinct et référencez à partir du projet de votre application, vous pouvez apporter des autres modifications à votre application sans avoir à régénérer le projet contenant le modèle.

*Remarque :* lors du déplacement de votre modèle pour séparer les assemblys n’oubliez pas de copier les chaînes de connexion pour le modèle dans le fichier de configuration d’application du projet client.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 désactiver la validation d’un modèle basé sur un edmx

Les modèles EDMX sont validées au moment de la compilation, même si le modèle est inchangé. Si votre modèle a déjà été validé, vous pouvez supprimer la validation au moment de la compilation en définissant la propriété « Valider sur Build » sur false dans la fenêtre Propriétés. Lorsque vous modifiez votre mappage ou un modèle, vous pouvez temporairement activer de nouveau validation pour vérifier vos modifications.

Notez que les améliorations des performances ont été apportées au concepteur Entity Framework pour Entity Framework 6 et le coût de la « valider sur la Build » est beaucoup plus faible dans les versions précédentes du concepteur.

## <a name="3-caching-in-the-entity-framework"></a>La mise en cache 3 dans Entity Framework

Entity Framework présente des formes de mise en cache intégrées suivantes :

1.  La mise en cache de l’objet : ObjectStateManager intégré à une instance ObjectContext assure le suivi dans la mémoire des objets qui ont été récupérées à l’aide de cette instance. Cela est également appelé cache de premier niveau.
2.  Caching de Plan de requête - réutiliser la commande de stockage généré lorsqu’une requête est exécutée plusieurs fois.
3.  Métadonnées de la mise en cache : les métadonnées pour un modèle de partage entre les différentes connexions au même modèle.

Outre les caches EF fournit prêts à l’emploi, un type spécial de fournisseur de données ADO.NET connu comme un habillage du fournisseur peut également être utilisé pour étendre l’Entity Framework avec un cache pour les résultats récupérés à partir de la base de données, également appelée mise en cache de second niveau.

### <a name="31-object-caching"></a>3.1 la mise en cache de l’objet

Par défaut lorsqu’une entité est retournée dans les résultats d’une requête, juste avant que EF matérialise, ObjectContext vérifie si une entité avec la même clé a déjà été chargée dans son ObjectStateManager. Si une entité avec les mêmes clés est déjà présente EF inclura dans les résultats de la requête. Bien que EF émet toujours la requête sur la base de données, ce comportement peut contournent une grande partie du coût de l’entité la matérialisation plusieurs fois.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 l’obtention des entités à partir du cache de l’objet à l’aide de la recherche de DbContext

Contrairement à une requête régulière, la méthode Find dans DbSet (API inclus pour la première fois dans EF 4.1) effectue une recherche dans la mémoire avant même émet une requête par rapport à la base de données. Il est important de noter que deux instances différentes de ObjectStateManager, ce qui signifie qu’ils ont des caches d’objet distinct dans deux instances ObjectContext différents.

Find utilise la valeur de clé primaire pour tenter de trouver une entité suivie par le contexte. Si l’entité n’est pas dans le contexte puis une requête sera exécutée et évaluée par rapport à la base de données, et la valeur null est retournée si l’entité est introuvable dans le contexte ou dans la base de données. Notez que la recherche retourne également des entités qui ont été ajoutées au contexte, mais n’ont pas encore été enregistrées dans la base de données.

Il existe une considération de performances à prendre lors de l’utilisation de recherche. Les appels à cette méthode par défaut déclenche une validation du cache d’objet afin de détecter les modifications qui sont toujours en attente de validation à la base de données. Ce processus peut être très coûteux, s’il existe un très grand nombre d’objets dans le cache d’objets ou dans un graphique d’objets volumineux sont ajoutés au cache de l’objet, mais il peut également être désactivé. Dans certains cas, vous pouvez percevoir sur un ordre de grandeur de différence dans l’appel de la méthode lorsque vous désactivez automatiquement détecter de Find modifications. Encore une deuxième ordre de grandeur est perçue lorsque l’objet est réellement dans le cache par rapport à quand l’objet doit être récupérée à partir de la base de données. Voici un exemple de graphique avec les mesures effectuées à l’aide de certains de nos microbenchmarks, exprimée en millisecondes, avec une charge de 5000 entités :

![Échelle logarithmique de .NET 4.5](~/ef6/media/net45logscale.png ".NET 4.5 - mise à l’échelle logarithmique")

Exemple de recherche avec les modifications de détection automatique désactivé :

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Ce que vous devez prendre en compte lors de l’utilisation de la méthode Find est :

1.  Si l’objet n’est pas dans le cache de l’opération de négation sont les avantages de la recherche, mais la syntaxe est encore plus simple qu’une requête par clé.
2.  Si les modifications de la détection automatique est activé le coût de la méthode Find peut augmenter par un ordre de grandeur, ou encore plus selon la complexité de votre modèle et la quantité d’entités dans votre cache d’objet.

En outre, n’oubliez pas que vous recherchez uniquement retourne l’entité que vous recherchez et ne seront pas automatiquement charges ses entités associées si elles ne sont pas déjà dans le cache d’objets. Si vous avez besoin de récupérer des entités associées, vous pouvez utiliser une requête par clé avec un chargement hâtif. Pour plus d’informations, consultez **8.1 vs le chargement différé. Le chargement hâtif**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problèmes de performances de lorsque le cache d’objets a de nombreuses entités

Le cache d’objet permet d’accroître la réactivité globale de Entity Framework. Toutefois, lorsque le cache d’objet a une très grande quantité d’entités chargées, qu'elle peut affecter certaines opérations telles que d’ajouter, supprimer, rechercher, entrée, SaveChanges et bien plus encore. En particulier, les opérations qui déclenchent un appel à DetectChanges seront être affectées par les caches de l’objet très volumineux. DetectChanges synchronise le graphique d’objets avec le Gestionnaire d’état objet et ses performances déterminée directement par la taille du graphique d’objets. Pour plus d’informations sur DetectChanges, consultez [suivi des modifications dans les entités POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Lorsque vous utilisez Entity Framework 6, les développeurs sont en mesure d’appeler AddRange et RemoveRange directement sur un DbSet, au lieu d’une itération sur une collection et ajouter l’appel une seule fois par instance. L’avantage d’utiliser les méthodes de la plage est que le coût de DetectChanges intervient qu’une seule fois pour l’ensemble des entités par opposition à une fois par chaque entité ajouté.

### <a name="32-query-plan-caching"></a>3.2 mise en cache de Plan de requête de

La première fois qu’une requête est exécutée, elle passe par le compilateur plan interne pour traduire la requête conceptuelle dans la commande de stockage (par exemple, le T-SQL qui est exécuté lors de l’exécuter sur SQL Server).  Si la mise en cache du plan de requête est activée, la prochaine fois que la requête est exécutée à la banque de commande sera récupérée directement sur le cache du plan de requête pour l’exécution, en contournant le compilateur de plan.

Le cache du plan de requête est partagé entre instances ObjectContext dans le même AppDomain. Vous n’avez pas besoin de conserver une instance ObjectContext pour tirer parti de la mise en cache du plan de requête.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 quelques remarques concernant la mise en cache de requête planifier

-   Le cache du plan de requête est partagé pour tous les types de requête : Entity SQL, LINQ to Entities et les objets de CompiledQuery.
-   Par défaut, la mise en cache du plan de requête est activée pour les requêtes Entity SQL, si les exécuter via un EntityCommand ou via un ObjectQuery. Il est également activée par défaut pour LINQ aux requêtes d’entités dans Entity Framework dans .NET 4.5 et dans Entity Framework 6
    -   La mise en cache du plan de requête peut être désactivée en définissant la propriété EnablePlanCaching (sur EntityCommand ou ObjectQuery) sur false. Exemple :
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Pour les requêtes paramétrables, modification de la valeur du paramètre toujours atteindra la requête mis en cache. Mais la modification facettes d’un paramètre (par exemple, taille, précision ou mise à l’échelle) atteindra une entrée différente dans le cache.
-   Lorsque vous utilisez Entity SQL, la chaîne de requête fait partie de la clé. Modification de la requête tout entraîne différentes entrées de cache, même si les requêtes sont fonctionnellement équivalents. Cela inclut les modifications apportées à la casse ou un espace blanc.
-   Lorsque vous utilisez LINQ, la requête est traitée pour générer une partie de la clé. Modification de l’expression LINQ génère donc une autre clé.
-   Autres limitations techniques peuvent s’appliquer ; Pour plus d’informations, consultez Autocompiled requêtes.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 algorithme d’éviction du cache

Comprendre comment le fonctionnement de l’algorithme interne peut vous aider à déterminer quand pour activer ou de la mise en cache de plan de requête disable. L’algorithme de nettoyage est comme suit :

1.  Une fois que le cache contient un nombre défini d’entrées (800), nous commençons un minuteur que périodiquement (une fois par minute) balaye du cache.
2.  Pendant les balayages de cache sont supprimées du cache sur un LFRU (moins fréquemment – récemment utilisé) base. Cet algorithme prend le nombre d’accès et âge en compte lorsque vous décidez quelles entrées sont supprimés du cache.
3.  À la fin de chaque balayage de cache, le cache contient à nouveau les 800 entrées.

Toutes les entrées de cache sont traitées de la même lorsque vous déterminez les entrées à supprimer. Cela signifie que la commande de stockage pour un CompiledQuery a la même probabilité d’éviction en tant que la commande de stockage pour une requête Entity SQL.

Notez que le minuteur d’éviction du cache est déclenchée dans lorsqu’il y a 800 entités dans le cache, mais que le cache est rangé uniquement de 60 secondes après le démarrage de ce minuteur. Cela signifie que pendant 60 secondes, votre cache peut augmenter pour être très volumineux.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 les métriques illustrant les performances de la mise en cache de plan de requête de test

Pour illustrer l’effet du plan de requête mise en cache sur les performances de votre application, nous avons effectué un test dans lequel nous avons exécuté un nombre de requêtes Entity SQL par rapport au modèle Navision. Consultez l’annexe pour obtenir une description du modèle Navision et les types de requêtes qui ont été exécutées. Dans ce test, nous avons tout d’abord effectuer une itération dans la liste des requêtes et exécuter chacun d’eux une fois pour les ajouter au cache (si la mise en cache est activé). Cette étape est untimed. Ensuite, nous avons mis en veille du thread principal pour plus de 60 secondes autoriser le cache de balayage pour prendre place ; Enfin, nous ne quittons l’heure de la liste un 2e pour exécuter les requêtes mises en cache. En outre, il cache du plan SQL Server est vidé avant l’exécution de chaque ensemble de requêtes afin que les temps de que nous obtenons avec précision reflètent l’avantage fourni par le cache du plan de requête.

##### <a name="3231-------test-results"></a>3.2.3.1 résultats des tests

| Tester                                                                   | EF5 aucun cache | EF5 mis en cache | EF6 aucun cache | EF6 mis en cache |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| L’énumération de toutes les 18723 requêtes                                          | 124          | 125.4      | 124,3        | 125.3      |
| En évitant de balayage (seulement 800 tout d’abord certaines requêtes, quelle que soit la complexité)  | 41.7         | 5.5        | 40,5         | 5,4        |
| Uniquement les requêtes AggregatingSubtotals (178 total - ce qui évite de balayage) | 39,5         | 4.5        | 38.1         | 4.6        |

*Toutes les heures en secondes.*

Moral - lors de l’exécution de nombreuses fonctions de requêtes distinctes (par exemple, créé dynamiquement des requêtes), la mise en cache ne suffit pas et la consommation en résultant du cache peut conserver les requêtes qui tireront le meilleur parti de la mise en cache de plan à partir de l’application.

Les requêtes de AggregatingSubtotals sont les plus complexes, les requêtes que nous avons testé avec. Comme prévu, plus la requête est complexe, le plus vous tirerez à partir de la mise en cache du plan de requête.

Car un CompiledQuery est vraiment une requête LINQ avec son plan mis en cache, la comparaison d’un CompiledQuery par rapport à la requête Entity SQL équivalente doit avoir des résultats similaires. En fait, si une application possède un grand nombre de requêtes Entity SQL dynamiques, remplit le cache de requêtes également déployer efficacement entraîne CompiledQueries à « décompiler » lorsqu’elles sont transférées à partir du cache. Dans ce scénario, vous pouvez améliorer les performances en désactivant la mise en cache sur les requêtes dynamiques pour hiérarchiser le CompiledQueries. Mieux encore, bien sûr, serait de réécrire l’application pour utiliser des requêtes paramétrables au lieu des requêtes dynamiques.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 utilisation CompiledQuery pour améliorer les performances des requêtes LINQ

Nos tests indiquent qu’à l’aide de CompiledQuery peut mettre un avantage de 7 % sur autocompiled requêtes LINQ ; Cela signifie que vous passerez 7 % moins de temps à l’exécution de code à partir de la pile d’Entity Framework ; Cela ne signifie pas que votre application sera 7 % plus rapides. En règle générale, le coût d’écriture et de conservation d’objets CompiledQuery dans Entity Framework 5.0 peut-être pas intéressant de la peine par rapport aux avantages. Votre kilométrage peut varier, de tester cette option si votre projet nécessite la notification push supplémentaire. Notez que CompiledQueries ne sont pas compatible avec les modèles dérivés de DbContext et compatible avec les modèles dérivés d’ObjectContext.

Pour plus d’informations sur la création et l’appel d’un CompiledQuery, consultez [requêtes compilées (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Il existe deux considérations que vous avez à effectuer lors de l’utilisation d’un CompiledQuery, à savoir la nécessité d’utiliser des instances statiques et les problèmes qu’ils utilisent dans composabilité. Voici une explication approfondie de ces deux considérations.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 Utilisez instances CompiledQuery statiques

Étant donné que la compilation d’une requête LINQ est un processus long, nous ne voulons le faire chaque fois que nous avons besoin extraire des données à partir de la base de données. Les instances de CompiledQuery vous compiler une seule fois et exécuter plusieurs fois, mais vous devez faire attention et livrez pour réutiliser la même instance de CompiledQuery chaque fois au lieu de compiler de façon répétée. L’utilisation de membres statiques pour stocker les instances de CompiledQuery devient nécessaire ; Sinon, vous ne voyez aucun avantage.

Par exemple, supposons que votre page comporte le corps de méthode suivant pour gérer l’affichage des produits de la catégorie sélectionnée :

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

Dans ce cas, vous allez créer une nouvelle instance de CompiledQuery à la volée chaque fois que la méthode est appelée. Au lieu de voir les avantages de performances en récupérant la commande de stockage du cache du plan de requête, le CompiledQuery passe par le compilateur plan chaque fois qu’une nouvelle instance est créée. En fait, vous serez polluer votre cache de plan de requête avec une nouvelle entrée de CompiledQuery chaque fois que la méthode est appelée.

Au lieu de cela, vous souhaitez créer une instance statique de la requête compilée, afin que vous appelez la même requête compilée chaque fois que la méthode est appelée. Une façon il s’agit en ajoutant l’instance de CompiledQuery en tant que membre de votre contexte de l’objet.  Vous pouvez ensuite rendre les choses un peu de nettoyage en accédant à la CompiledQuery via une méthode d’assistance :

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Cette méthode d’assistance peut être appelée comme suit :

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 composition sur un CompiledQuery

La capacité à composer sur n’importe quelle requête LINQ est très utile ; Pour ce faire, il suffit d’appeler une méthode après le IQueryable comme *Skip()* ou *Count()*. Toutefois, il s’agit essentiellement de faire retourne un nouvel objet IQueryable. Il n’existe rien ne vous techniquement à partir de la composition d’un CompiledQuery, cela entraîne la génération d’un nouvel objet IQueryable qui nécessite en passant par le compilateur de plan à nouveau.

Certains composants utilisera IQueryable composé objets pour activer des fonctionnalités avancées. Par exemple, ASP. GridView du NET peut être lié aux données à un objet IQueryable via la propriété SelectMethod. Le contrôle GridView sera ensuite composer sur cet objet IQueryable pour permettre le tri et de pagination sur le modèle de données. Comme vous pouvez le voir, à l’aide d’un CompiledQuery pour le contrôle GridView ne serait pas atteint la requête compilée, mais génère une nouvelle requête autocompiled.

L’équipe de conseil clientèle détaille cela dans son billet de blog « Potentiels performances problèmes avec compilé LINQ requête recompilations » : <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Un seul endroit où vous risquez de rencontrer ce est lorsque vous ajoutez des filtres progressifs à une requête. Par exemple, supposons que vous disposiez d’une page de clients avec plusieurs listes déroulantes de filtres facultatifs (par exemple, pays et OrdersCount). Vous pouvez composer ces filtres sur les résultats IQueryable d’un CompiledQuery, mais cela provoquera dans la nouvelle requête passer par le compilateur plan chaque fois que vous l’exécutez.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Pour éviter cette recompilation, vous pouvez réécrire la CompiledQuery pour tenir compte des filtres possibles :

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Ce qui serait appelé dans l’interface utilisateur tels que :

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Un inconvénient ici est la commande de stockage généré aura toujours des filtres avec les vérifications null, mais il doivent être relativement simples pour le serveur de base de données pour optimiser les :

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 la mise en cache des métadonnées de

Entity Framework prend également en charge la mise en cache des métadonnées. Cela est essentiellement la mise en cache des informations de type et les informations de mappage de type de base de données entre différentes connexions au même modèle. Le cache de métadonnées est unique pour chaque domaine d’application.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 algorithme de mise en cache des métadonnées de

1.  Informations de métadonnées pour un modèle sont stockées dans un ItemCollection pour chaque EntityConnection.
    -   Note à part, il existe des objets ItemCollection différents pour les différentes parties du modèle. Par exemple, StoreItemCollections contient les informations relatives au modèle de base de données ; ObjectItemCollection contient des informations sur le modèle de données ; EdmItemCollection contient des informations relatives au modèle conceptuel.

2.  Si les deux connexions utilisent la même chaîne de connexion, ils partageront la même instance ItemCollection.
3.  Chaînes de connexion fonctionnellement équivalent mais textuellement différents peuvent entraîner des caches de métadonnées différentes. Nous marquer des chaînes de connexion, il vous suffit de modifier l’ordre des jetons doit générer des métadonnées partagées. Mais les deux chaînes de connexion qui semblent fonctionnellement le même ne peuvent pas être évalués comme étant identiques après la création de jetons.
4.  ItemCollection est régulièrement vérifiée pour utilisation. S’il est déterminé qu’un espace de travail n’a accédé récemment, il sera indiqué pour le nettoyage sur le balayage de cache suivant.
5.  Si vous créez un EntityConnection entraîne un cache de métadonnées doit être créé (bien que les collections d’éléments qu’il contient ne seront pas initialisées jusqu'à ce que la connexion est ouverte). Cet espace de travail reste en mémoire jusqu'à ce que l’algorithme de mise en cache détermine qu’il n’est pas « en cours d’utilisation ».

L’équipe de conseil clientèle a écrit un billet de blog décrivant contenant une référence à un ItemCollection afin d’éviter le « obsolescence » lors de l’utilisation de grands modèles : \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 la relation entre la mise en cache des métadonnées et de mise en cache de requête planifier

L’instance de cache de plan de requête se trouve dans ItemCollection du MetadataWorkspace de types de magasin. Cela signifie que les commandes de stockage mis en cache seront être utilisés pour les requêtes sur n’importe quel contexte instancié à l’aide d’une donnée MetadataWorkspace. Cela signifie également que si vous avez deux chaînes de connexion qui sont légèrement différents et ne correspondent pas après la création de jetons, vous devez requête différentes instances de cache de plan.

### <a name="35-results-caching"></a>3.5 la mise en cache des résultats

Avec les résultats de la mise en cache (également appelé « second-level caching »), vous conservez les résultats des requêtes dans un cache local. Lors de l’émission d’une requête, vous voyez tout d’abord si les résultats sont disponibles localement avant de vous interrogez par rapport au magasin. Tandis que les résultats de la mise en cache n’est pas directement pris en charge par Entity Framework, il est possible d’ajouter un cache de deuxième niveau en utilisant un fournisseur d’habillage. Un exemple de fournisseur d’habillage avec un cache de second niveau est de Alachisoft [Entity Framework deuxième niveau Cache basé sur NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Cette implémentation de la mise en cache de second niveau est une fonctionnalité injectée qui a lieu après l’évaluation de l’expression LINQ (et funcletized) et le plan d’exécution de requête est calculé ou extraite du cache de premier niveau. Le cache de second niveau stocke uniquement les résultats de la base de données brutes, puis pour le pipeline de matérialisation s’exécute toujours par la suite.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 des références supplémentaires pour les résultats de la mise en cache avec le fournisseur d’habillage

-   Julie Lerman a écrit un article MSDN de « Second niveau de mise en cache dans Entity Framework et Windows Azure » qui inclut la mise à jour de l’exemple de fournisseur d’habillage pour utiliser la mise en cache de Windows Server AppFabric : [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Si vous travaillez avec Entity Framework 5, le blog de l’équipe a une requête post qui décrit la procédure à effectuer des actions en cours d’exécution avec le fournisseur de mise en cache pour Entity Framework 5 : \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Il inclut également un modèle T4 pour aider à automatiser l’ajout de la mise en cache de la 2e niveau à votre projet.

## <a name="4-autocompiled-queries"></a>Requêtes Autocompiled 4

Quand une requête est exécutée sur une base de données à l’aide d’Entity Framework, elle doit traverser une série d’étapes avant de matérialiser réellement les résultats ; une étape de ce type est la Compilation de la requête. Requêtes Entity SQL connues pour bénéficier de bonnes performances car ils sont automatiquement la mise en cache, l’heure de la deuxième ou troisième vous exécuter la même requête, il peut ignorer le compilateur de plan et utilisez le plan mis en cache à la place.

Entity Framework 5 a introduit la mise en cache automatique pour LINQ aux requêtes d’entités également. Dans les éditions précédentes d’Entity Framework, la création d’un CompiledQuery pour accélérer vos performances a été une pratique courante, comme cela rendrait votre LINQ pour interroger des entités mis en cache. Étant donné que la mise en cache est désormais effectuée automatiquement sans l’utilisation d’un CompiledQuery, nous appelons cette fonctionnalité « requêtes autocompiled ». Pour plus d’informations sur le cache du plan de requête et ses mécanismes, consultez Mise en cache du Plan de requête.

Entity Framework détecte lorsqu’une requête requiert d’être recompilé, et le fait lorsque la requête est appelée même si elle avait déjà été compilé. Les conditions courantes qui provoquent la recompilation de la requête sont :

-   Modification de la MergeOption associée à votre requête. La requête de mise en cache ne sera pas utilisée, à la place le compilateur de plan est ré-exécutées et le plan nouvellement créé est mis en cache.
-   Modification de la valeur de ContextOptions.UseCSharpNullComparisonBehavior. Vous obtenez le même effet que la modification de la MergeOption.

Autres conditions peuvent empêcher votre requête à partir de l’utilisation du cache. Des exemples courants sont :

-   À l’aide de IEnumerable&lt;T&gt;. Contient&lt;&gt;(valeur T).
-   À l’aide de fonctions qui produisent des requêtes avec des constantes.
-   En utilisant les propriétés d’un objet non mappés.
-   Liaison de votre requête à une autre requête qui nécessite d’être recompilé.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 Utilisation IEnumerable&lt;T&gt;. Contient&lt;T&gt;(valeur T)

Entity Framework ne met pas en cache les requêtes qui appellent IEnumerable&lt;T&gt;. Contient&lt;T&gt;(valeur T) par rapport à une collection en mémoire, étant donné que les valeurs de la collection sont considérés comme volatiles. L’exemple de requête suivant ne sera pas être mis en cache, il sera toujours traitée par le compilateur de plan :

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Notez que la taille de l’interface IEnumerable contre les Contains est exécutée détermine la vitesse à laquelle ou comment lentement votre requête est compilée. Lors de l’utilisation de grandes collections, comme illustré dans l’exemple ci-dessus, les performances peuvent diminuer considérablement.

Entity Framework 6 contient des optimisations à la façon dont IEnumerable&lt;T&gt;. Contient&lt;T&gt;(valeur T) fonctionne lorsque les requêtes sont exécutées. Le code SQL généré est beaucoup plus rapide pour produire et plus lisible, et dans la plupart des cas il exécute également plus rapidement sur le serveur.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 à l’aide de fonctions qui produisent des requêtes avec des constantes

Les opérateurs Skip(), utilisez, Contains() et DefautIfEmpty() LINQ ne produisent pas de requêtes SQL incluant des paramètres, mais placent les valeurs qui leur sont transmis en tant que constantes. Pour cette raison, les requêtes qui seraient sinon identiques finissent polluer la requête plan cache, à la fois sur la pile d’EF et sur le serveur de base de données et obtenir reutilized pas, sauf si l’une des constantes mêmes sont utilisés dans une exécution de requête suivants. Exemple :

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

Dans cet exemple, chaque fois que cette requête est exécutée avec une valeur différente pour l’id de la requête sera compilé dans un nouveau plan.

En particulier d’attention à l’utilisation de Skip et Take lors de la pagination. Dans EF6, ces méthodes ont une surcharge de lambda qui rend le plan de requête mis en cache réutilisables, car EF peut capturer les variables passées à ces méthodes et les traduire en objets SqlParameter. Cela vous permet également de maintenir le cache plus propre, car sinon, chaque requête avec une autre constante pour Skip et Take obtiendriez sa propre entrée de cache de plan de requête.

Considérez le code suivant, qui n’est pas optimales, mais vise uniquement à titre de cette classe de requêtes :

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Une version plus rapide de ce même code impliquerait l’appel de Skip avec une expression lambda :

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Le second extrait de code peut-être s’exécuter jusqu'à 11 % plus rapide, car le même plan de requête est utilisé chaque fois que la requête est exécutée, ce qui fait gagner du temps de processeur et évite de polluer le cache de requête. En outre, étant donné que le paramètre Skip est dans une fermeture le code peut également ressembler à ceci maintenant :

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 en utilisant les propriétés d’un objet non mappés

Quand une requête utilise les propriétés d’un type d’objet non mappés comme un paramètre, la requête ne sera pas mise en cache. Exemple :

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

Dans cet exemple, supposons que la classe NonMappedType ne fait pas partie du modèle d’entité. Cette requête peut facilement être modifiée pour ne pas utiliser un type non mappés et à la place utiliser une variable locale comme paramètre à la requête :

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

Dans ce cas, la requête sera en mesure de mise en cache et bénéficier du cache du plan de requête.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 liaison pour les requêtes nécessitant la recompilation

Suivant le même exemple que ci-dessus, si vous disposez d’une deuxième requête qui s’appuie sur une requête qui doit être recompilé, votre seconde requête entière sera également être recompilée. Voici un exemple pour illustrer ce scénario :

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

L’exemple est générique, mais il illustre comment la liaison à firstQuery pose secondQuery impossibilité d’obtenir la mise en cache. Si une requête qui nécessite une recompilation n’avait pas été firstQuery, puis secondQuery serait ont été mis en cache.

## <a name="5-notracking-queries"></a>5 requêtes NoTracking

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 la désactivation de suivi des modifications pour les frais de gestion d’état

Si vous êtes dans un scénario en lecture seule et que vous souhaitez éviter la surcharge de chargement des objets dans ObjectStateManager, vous pouvez émettre des requêtes « Aucun suivi ».  Le suivi des modifications peut être désactivée au niveau de la requête.

Notez cependant qu’en désactivant le suivi des modifications vous permettent effectivement de désactiver le cache d’objets. Lorsque vous interrogez pour une entité, nous ne pouvons pas ignorer la matérialisation en extrayant les résultats de requête précédemment matérialisés à partir de l’ObjectStateManager. Si vous interrogez à plusieurs reprises les mêmes entités sur le même contexte, vous pouvez réellement voir un performance bénéficient de l’activation du suivi des modifications.

Lors de l’interrogation à l’aide d’ObjectContext, instances ObjectQuery et ObjectSet mémorisent une MergeOption une fois qu’elle est définie, et requêtes composées sur ces derniers hérite la MergeOption effective de la requête parente. Lorsque vous utilisez DbContext, suivi peut être désactivé en appelant le modificateur AsNoTracking() sur le DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 la désactivation de suivi des modifications pour une requête lors de l’utilisation de DbContext

Vous pouvez changer de mode d’une requête à NoTracking le chaînage d’un appel à la méthode AsNoTracking() dans la requête. Contrairement à ObjectQuery, les classes DbSet et DbQuery dans l’API DbContext n’ont pas une propriété mutable pour la MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 désactiver le suivi des modifications au niveau de la requête à l’aide d’ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 désactiver le suivi des modifications pour une entité toute entière définie à l’aide d’ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 métriques de test d’illustrant l’amélioration des performances des requêtes de NoTracking

Dans ce test, nous allons au détriment de remplissage ObjectStateManager en comparant le suivi pour les requêtes NoTracking pour le modèle de Navision. Consultez l’annexe pour obtenir une description du modèle Navision et les types de requêtes qui ont été exécutées. Dans ce test, nous effectuer une itération dans la liste des requêtes et exécuter une fois de chacun d’eux. Nous avons exécuté deux variantes du test, une autre fois avec les requêtes NoTracking et une fois avec l’option de fusion par défaut de « AppendOnly ». Nous avons exécuté chaque variation de 3 fois et que vous prenez la valeur moyenne des exécutions. Entre les tests, nous avons effacer le cache de requête sur le serveur SQL Server et réduire la base de données tempdb en exécutant les commandes suivantes :

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Résultats des tests, s’exécute sur 3 médian :

|                        | AUCUN SUIVI – PLAGE DE TRAVAIL | AUCUN SUIVI – HEURE | AJOUTER UNIQUEMENT – PLAGE DE TRAVAIL | AJOUTER UNIQUEMENT : HEURE |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 aura un encombrement mémoire à la fin de l’exécution du rapport à Entity Framework 6. La mémoire consommée par Entity Framework 6 est le résultat de structures de mémoire supplémentaire et de code qui permettent de nouvelles fonctionnalités et meilleures performances.

Il existe également une différence dans l’encombrement mémoire lors de l’utilisation de l’ObjectStateManager. Entity Framework 5 a augmenté son empreinte de 30 % lorsque le suivi de toutes les entités que nous matérialisés à partir de la base de données. Entity Framework 6 a augmenté son empreinte de 28 % lors de cette opération.

En termes de temps, Entity Framework 6 surpasse Entity Framework 5 dans ce test d’une marge de grande taille. Entity Framework 6 terminé le test dans environ 16 % du temps consommé par Entity Framework 5. En outre, Entity Framework 5 prend plus de 9 % de temps lorsque l’ObjectStateManager est utilisé. En comparaison, Entity Framework 6 est à l’aide de plus de temps lors de l’utilisation de l’ObjectStateManager de 3 %.

## <a name="6-query-execution-options"></a>Options de l’exécution de requête 6

Entity Framework propose différentes façons de requête. Nous allons examiner les options suivantes, comparez les avantages et inconvénients de chacun d’eux et examiner leurs caractéristiques de performances :

-   LINQ to Entities.
-   Aucun suivi LINQ to Entities.
-   Entité SQL sur un ObjectQuery.
-   Entité SQL sur un EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1 requêtes LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Professionnels de l'**

-   Approprié pour les opérations CUD.
-   Objets entièrement matérialisés.
-   La plus simple à écrire avec la syntaxe intégrées dans le langage de programmation.
-   Bonnes performances.

**Inconvénients**

-   Certaines restrictions techniques, telles que :
    -   Modèles d’utilisation de DefaultIfEmpty pour les requêtes de jointure externe entraînent des requêtes plus complexes que de simples instructions OUTER JOIN dans Entity SQL.
    -   Vous ne pouvez pas utiliser LIKE et la correspondance de modèle général.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2 aucun suivi LINQ aux requêtes d’entités

Lorsque le contexte est dérivée d’ObjectContext :

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Lorsque le contexte dérive DbContext :

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Professionnels de l'**

-   Amélioration des performances sur les requêtes LINQ régulières.
-   Objets entièrement matérialisés.
-   La plus simple à écrire avec la syntaxe intégrées dans le langage de programmation.

**Inconvénients**

-   Ne convient pas d’opérations CUD.
-   Certaines restrictions techniques, telles que :
    -   Modèles d’utilisation de DefaultIfEmpty pour les requêtes de jointure externe entraînent des requêtes plus complexes que de simples instructions OUTER JOIN dans Entity SQL.
    -   Vous ne pouvez pas utiliser LIKE et la correspondance de modèle général.

Notez que les requêtes qui projettent des propriétés scalaires ne sont pas suivies même si le NoTracking n’est pas spécifié. Exemple :

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Cette requête spécifique ne spécifie pas explicitement en cours NoTracking, mais dans la mesure où il n’est pas matérialiser un type qui a connu le Gestionnaire d’état objet puis le résultat matérialisé n’est pas suivi.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 entity SQL sur un ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Professionnels de l'**

-   Approprié pour les opérations CUD.
-   Objets entièrement matérialisés.
-   Prend en charge la mise en cache du plan de requête.

**Inconvénients**

-   Implique des chaînes de requête textuelle qui sont plus susceptibles d’engendrer des erreurs des utilisateurs à des constructions de requêtes intégrées au langage.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 entity SQL sur une commande de l’entité

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Professionnels de l'**

-   Prend en charge de requête mise en cache de plan dans .NET 4.0 (mise en cache du plan est pris en charge par tous les autres types de requêtes dans .NET 4.5).

**Inconvénients**

-   Implique des chaînes de requête textuelle qui sont plus susceptibles d’engendrer des erreurs des utilisateurs à des constructions de requêtes intégrées au langage.
-   Ne convient pas d’opérations CUD.
-   Résultats ne sont pas matérialisés automatiquement et doivent être lues à partir du lecteur de données.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery et ExecuteStoreQuery

SqlQuery sur la base de données :

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery sur DbSet :

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery :

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Professionnels de l'**

-   En règle générale le plus rapide des performances étant donné que le compilateur de plan est ignorée.
-   Objets entièrement matérialisés.
-   Approprié pour les opérations CUD lorsqu’il est utilisé dans le DbSet.

**Inconvénients**

-   Requête est textuelle et sujet aux erreurs.
-   Requête est liée à un serveur principal spécifique à l’aide de la sémantique du magasin au lieu de la sémantique conceptuelle.
-   Lors de l’héritage est présent, requête XMLHttpRequest doit prendre en compte les conditions de mappage pour le type demandé.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Professionnels de l'**

-   Un maximum d’offre une amélioration des performances de 7 % sur les requêtes LINQ régulières.
-   Objets entièrement matérialisés.
-   Approprié pour les opérations CUD.

**Inconvénients**

-   Augmenter la complexité et surcharge de programmation.
-   L’amélioration des performances est perdue lors de la composition sur une requête compilée.
-   Certaines requêtes LINQ ne peut pas être écrit comme un CompiledQuery - par exemple, les projections de types anonymes.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 Comparaison des performances de des options de requête différents

Microbenchmarks simples où la création de contexte n’était pas minutée ont été mis à l’essai. Nous avons mesuré l’interrogation de 5 000 fois pour un ensemble d’entités de non mise en cache dans un environnement contrôlé. Ces numéros sont effectués avec un avertissement : ils ne reflètent pas les nombres réels générée par une application, mais au lieu de cela, ils sont une mesure très précise de la quantité d’une différence de performances lors de l’interrogation des options différentes sont comparées. pommes comparables, à l’exclusion du coût de la création d’un nouveau contexte.

| EF  | Tester                                 | Temps (ms) | Mémoire   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Requête Linq de ObjectContext             | 2692      | 38277120 |
| EF5 | Aucun suivi de requête Linq de DbContext     | 2818      | 41840640 |
| EF5 | Requête Linq de DbContext                 | 2930      | 41771008 |
| EF5 | Aucun suivi de requête Linq de ObjectContext | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Requête Linq de ObjectContext             | 3074      | 45248512 |
| EF6 | Aucun suivi de requête Linq de DbContext     | 3125      | 47575040 |
| EF6 | Requête Linq de DbContext                 | 3420      | 47652864 |
| EF6 | Aucun suivi de requête Linq de ObjectContext | 3593      | 45260800 |

![Micro-tests EF5, 5000 itérations à chaud](~/ef6/media/ef5micro5000warm.png)

![EF6 micro-tests, 5000 itérations à chaud](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks sont très sensibles aux petites modifications dans le code. Dans ce cas, la différence entre les coûts d’Entity Framework 5 et Entity Framework 6 sont en raison de l’ajout de [l’interception](~/ef6/fundamentals/logging-and-interception.md) et [améliorations transactionnelles](~/ef6/saving/transactions.md). Ces numéros microbenchmarks, cependant, sont une vision amplifiée dans un très petit fragment de ce que fait Entity Framework. Scénarios réels de requêtes à chaud ne devrait pas une régression des performances lors de la mise à niveau à partir d’Entity Framework 5 vers Entity Framework 6.

Pour comparer les performances réelles des options de requête différents, nous avons créé des variations de test distinct 5 où nous utilisons une option de requête différents pour sélectionner tous les produits dont le nom catégorie est « Boissons ». Chaque itération inclut le coût de la création du contexte et le coût de la matérialisation d’entités tous les retours. 10 itérations sont exécutées untimed avant de passer à la somme des itérations de 1000 a expiré. Les résultats affichés sont l’exécution médiane proviennent de 5 exécutions de chaque test. Pour plus d’informations, voir l’annexe B, qui inclut le code pour le test.

| EF  | Tester                                        | Temps (ms) | Mémoire   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Commande de l’entité d’ObjectContext                | 621       | 39350272 |
| EF5 | Requête Sql de DbContext sur la base de données             | 825       | 37519360 |
| EF5 | Requête d’ObjectContext Store                   | 878       | 39460864 |
| EF5 | Aucun suivi de requête Linq de ObjectContext        | 969       | 38293504 |
| EF5 | ObjectContext Entity Sql à l’aide de la requête d’objet | 1089      | 38981632 |
| EF5 | Requête compilée ObjectContext                | 1099      | 38682624 |
| EF5 | Requête Linq de ObjectContext                    | 1152      | 38178816 |
| EF5 | Aucun suivi de requête Linq de DbContext            | 1208      | 41803776 |
| EF5 | Requête Sql de DbContext sur DbSet                | 1414      | 37982208 |
| EF5 | Requête Linq de DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Commande de l’entité d’ObjectContext                | 480       | 47247360 |
| EF6 | Requête d’ObjectContext Store                   | 493       | 46739456 |
| EF6 | Requête Sql de DbContext sur la base de données             | 614       | 41607168 |
| EF6 | Aucun suivi de requête Linq de ObjectContext        | 684       | 46333952 |
| EF6 | ObjectContext Entity Sql à l’aide de la requête d’objet | 767       | 48865280 |
| EF6 | Requête compilée ObjectContext                | 788       | 48467968 |
| EF6 | Aucun suivi de requête Linq de DbContext            | 878       | 47554560 |
| EF6 | Requête Linq de ObjectContext                    | 953       | 47632384 |
| EF6 | Requête Sql de DbContext sur DbSet                | 1023      | 41992192 |
| EF6 | Requête Linq de DbContext                        | 1290      | 47529984 |


![Itérations de requête urgente 1000 EF5](~/ef6/media/ef5warmquery1000.png)

![Itérations de requête urgente 1000 EF6](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Par souci d’exhaustivité, nous avons inclus une variation dans laquelle nous exécutons une requête Entity SQL sur un EntityCommand. Toutefois, étant donné que les résultats ne sont pas matérialisés de telles requêtes, la comparaison n’est pas nécessairement pommes de point à point. Le test inclut une approximation matérialisation pour essayer d’effectuer la comparaison microprocesseur.

Dans ce cas de bout en bout, Entity Framework 6 surpasse Entity Framework 5 en raison des améliorations de performances obtenues sur plusieurs parties de la pile, notamment un beaucoup plus clair DbContext l’initialisation et plus rapide MetadataCollection&lt;T&gt; recherches.

## <a name="7-design-time-performance-considerations"></a>Considérations sur la performance de l’heure de conception 7

### <a name="71-------inheritance-strategies"></a>7.1 stratégies d’héritage de

Une autre considération de performance lors de l’utilisation d’Entity Framework est la stratégie d’héritage que vous utilisez. Entity Framework prend en charge 3 types de base de l’héritage ainsi que leurs combinaisons :

-   Table par hiérarchie (TPH) – où chaque héritage défini maps à une table avec une colonne de discriminateur pour indiquer le type particulier dans la hiérarchie est représenté dans la ligne.
-   Table par Type (TPT) – où chaque type possède sa propre table dans la base de données ; les tables enfants définissent uniquement les colonnes qui ne contient pas la table parente.
-   Table par classe (TPC) – où chaque type possède sa propre table complète dans la base de données ; les tables enfants définissent tous leurs champs, y compris ceux définis dans les types parents.

Si votre modèle utilise l’héritage TPT, les requêtes qui sont générés sera plus complexes que celles qui sont générés avec les autres stratégies l’héritage, ce qui peuvent aboutir sur plus de temps d’exécution sur le magasin.  En règle générale prendra plus de temps pour générer des requêtes sur un modèle TPT et pour matérialiser les objets résultants.

Consultez les « considérations sur les performances lors de l’utilisation de l’héritage TPT (Table par Type) dans le cadre de l’entité » billet de blog MSDN : \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 évitant TPT dans les applications Model First ou Code First

Lorsque vous créez un modèle sur une base de données existante qui dispose d’un schéma TPT, vous n’avez pas beaucoup d’options. Mais lorsque vous créez une application à l’aide de Model First ou Code First, vous devez éviter l’héritage TPT pour les problèmes de performances.

Lorsque vous utilisez le premier modèle dans l’Assistant de concepteur d’entité, vous obtiendrez TPT pour tout l’héritage dans votre modèle. Si vous souhaitez basculer vers une stratégie de l’héritage TPH avec Model First, vous pouvez utiliser le « Entity Designer de base de données Generation Power Pack » disponible à partir de la galerie Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Lorsque vous utilisez Code First pour configurer le mappage d’un modèle avec l’héritage, EF utilise TPH par défaut, par conséquent, toutes les entités dans la hiérarchie d’héritage seront mappées à la même table. Consultez la section « Mappage avec l’API Fluent » de l’article « Code première dans entité Framework4.1 » dans MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) pour plus d’informations.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 la mise à niveau à partir d’EF4 pour améliorer la génération du modèle de temps

Une amélioration spécifiques à SQL Server à l’algorithme qui génère la couche de stockage (SSDL) du modèle est disponible dans Entity Framework 5 et 6 et comme une mise à jour Entity Framework 4 lors de l’installation de Visual Studio 2010 SP1. Les résultats de test suivants illustrent l’amélioration lors de la génération d’un modèle très volumineux, dans ce cas, le modèle Navision. Pour plus d’informations à ce sujet, consultez l’annexe C.

Le modèle contient les jeux d’entités de 1005 et 4227 ensembles d’associations.

| Configuration                              | Répartition du temps consommé                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Génération du langage SSDL : 2 h 27 min. <br/> Génération de mappage : 1 seconde <br/> Génération de CSDL : 1 seconde <br/> Génération de ObjectLayer : 1 seconde <br/> Génération de vues : 2 h 14 min |
| Visual Studio 2010 SP1, Entity Framework 4 | Génération du langage SSDL : 1 seconde <br/> Génération de mappage : 1 seconde <br/> Génération de CSDL : 1 seconde <br/> Génération de ObjectLayer : 1 seconde <br/> Génération de vues : 1 h 53 min.   |
| Visual Studio 2013, Entity Framework 5     | Génération du langage SSDL : 1 seconde <br/> Génération de mappage : 1 seconde <br/> Génération de CSDL : 1 seconde <br/> Génération de ObjectLayer : 1 seconde <br/> Génération de vues : 65 minutes    |
| Visual Studio 2013, Entity Framework 6     | Génération du langage SSDL : 1 seconde <br/> Génération de mappage : 1 seconde <br/> Génération de CSDL : 1 seconde <br/> Génération de ObjectLayer : 1 seconde <br/> Génération de vues : 28 secondes.   |


Il est important de noter que lorsque vous générez le langage SSDL, la charge est presque entièrement consacrée à SQL Server, tandis que l’ordinateur de développement client est en attente inactive pour les résultats de revenir à partir du serveur. DBA doit apprécieront particulièrement cette amélioration. Il est également important de noter qu’essentiellement la totalité des coûts de génération de modèle a lieu dans la génération de vues maintenant.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 fractionnement tout d’abord les modèles volumineux avec la base de données et Model First

Augmente la taille du modèle, l’aire du concepteur devient encombrée et difficile à utiliser. En règle générale, nous considérons un modèle avec des entités plus de 300 trop importante pour utiliser efficacement le concepteur. Le billet de blog suivant décrit plusieurs options pour les modèles volumineux de fractionnement : \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

L’article a été écrit pour la première version d’Entity Framework, mais les étapes s’appliquent toujours.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>Version 7.4 considérations sur les performances de l’avec le contrôle de Source de données Entity

Nous avons vu des cas dans les tests de contrainte et de performances multithread où les performances d’une application web à l’aide du contrôle EntityDataSource se détériorent de manière significative. La cause sous-jacente est que le contrôle EntityDataSource appelle à plusieurs reprises MetadataWorkspace.LoadFromAssembly sur les assemblys référencés par l’application Web pour découvrir les types à utiliser en tant qu’entités.

La solution consiste à définir le ContextTypeName du contrôle EntityDataSource au nom de type de votre classe dérivée d’ObjectContext. Cette option désactive le mécanisme qui analyse tous les assemblys référencés pour les types d’entité.

La définition du champ ContextTypeName empêche également un problème fonctionnel où contrôle EntityDataSource dans .NET 4.0 lève une ReflectionTypeLoadException lorsqu’il ne peut pas charger un type à partir d’un assembly via la réflexion. Ce problème a été résolu dans .NET 4.5.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 des entités POCO et des proxys de suivi des modifications

Entity Framework vous permet d’utiliser des classes de données personnalisées avec votre modèle de données sans apporter de modifications pour les classes de données eux-mêmes. Cela signifie que vous pouvez utiliser des objets CLR « classiques » (ou POCO), tels que les objets de domaine existants, avec votre modèle de données. Ces classes de données POCO (également appelées objets ignorant la persistance), qui sont mappées aux entités qui sont définies dans un modèle de données, prennent en charge la plupart de la même requête, insérant, mettre à jour et supprimer des comportements de types d’entité générés par les outils Entity Data Model.

Entity Framework peut également créer des classes proxy dérivés de vos types POCO, qui sont utilisés lorsque vous souhaitez activer des fonctionnalités telles que le chargement différé et le suivi automatique sur les entités POCO. Vos classes POCO doivent remplir certaines conditions pour permettre d’Entity Framework utiliser des proxys, comme indiqué ici : [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Les proxys de suivi des chance informe le Gestionnaire d’état objet chaque fois que les propriétés de vos entités atteint sa valeur modifiée, donc l’Entity Framework connaît l’état de vos entités tout le temps. Pour cela, en ajoutant des événements de notification pour le corps des méthodes setter de propriétés de votre et avoir le Gestionnaire d’état objet de traitement des événements. Notez que la création d’un proxy d’entité sera généralement être plus coûteux que la création d’une entité POCO de non proxy en raison de l’ajout ensemble d’événements créées par Entity Framework.

Lorsqu’une entité POCO ne dispose pas d’un proxy de suivi, les modifications sont détectées en comparant le contenu de vos entités sur une copie d’un état enregistré précédent. Cette comparaison approfondie deviendra un processus long lorsque vous disposez de nombreuses entités dans votre contexte, ou lorsque vos entités une très grande quantité de propriétés, même si aucune d'entre elles changé depuis la dernière comparaison a eu lieu.

En résumé : vous payez la dégradation des performances lors de la création de proxy de suivi des modifications, mais le suivi des modifications vous aideront à accélérer le processus de détection de modification lors de vos entités ont de nombreuses propriétés ou si vous avez de nombreuses entités dans votre modèle. Pour les entités avec un petit nombre de propriétés où la quantité d’entités n’augmente pas trop, avoir des proxys de suivi des modifications peut-être pas de beaucoup d’avantages.

## <a name="8-loading-related-entities"></a>8 lors du chargement des entités connexes

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 vs de chargement différées. Chargement hâtif

Entity Framework propose différentes façons de charger les entités qui sont associées à votre entité cible. Par exemple, lorsque vous interrogez des produits, il existe différentes façons, que les commandes connexes seront chargés dans le Gestionnaire d’état objet. À partir d’un point de vue performances, la question la plus importante à prendre en compte lors du chargement des entités associées seront s’il faut utiliser le chargement différé ou le chargement hâtif.

Lorsque vous utilisez le chargement hâtif, les entités associées sont chargées en même temps que votre jeu d’entités cible. Vous utilisez une instruction Include dans votre requête pour indiquer quelles entités que vous voulez importer connexes.

Lorsque vous utilisez le chargement différé, votre requête initiale remet uniquement dans le jeu d’entités cible. Mais chaque fois que vous accédez à une propriété de navigation, une autre requête est émise sur le magasin pour charger l’entité associée.

Une fois qu’une entité a été chargée, toutes les requêtes supplémentaires pour l’entité le charger directement depuis le Gestionnaire d’état objet, si vous utilisez le chargement différé ou le chargement hâtif.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 comment choisir entre le chargement différé et le chargement hâtif

Il est important que vous comprenez la différence entre le chargement différé et le chargement hâtif afin que vous pouvez rendre le bon choix pour votre application. Cela vous aidera à évaluer le compromis entre plusieurs demandes par rapport à la base de données par rapport à une requête unique qui peut contenir une charge utile importante. Il peut être approprié d’utiliser le chargement hâtif dans certaines parties de votre application et le chargement différé dans d’autres parties.

Ainsi, ce qui se passe sous le capot, supposons que vous souhaitez interroger pour les clients qui vivent dans le Royaume-Uni et leur nombre de commandes.

**À l’aide d’un chargement hâtif**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**À l’aide de chargement différé**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Lorsque vous utilisez le chargement hâtif, vous allez émettre une requête unique qui retourne tous les clients et toutes les commandes. La commande de stockage ressemble à :

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Lorsque vous utilisez le chargement différé, vous allez émettre initialement la requête suivante :

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

Et chaque fois que vous accédez à la propriété de navigation de commandes d’un client émise sur le magasin une autre requête comme suit :

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Pour plus d’informations, consultez le [le chargement des objets connexes](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 le chargement différé par rapport à l’aide-mémoire pour le chargement hâtif

Il n’existe pas comme universelle à la sélection d’un chargement hâtif et le chargement différé. Essayez tout d’abord de comprendre les différences entre les deux stratégies que vous pouvez effectuer une décision avisée bien ; en outre, prenez en compte si votre code s’intègre à l’un des scénarios suivants :

| Scénario                                                                    | Notre Suggestion                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vous devez accéder à nombreuses propriétés de navigation à partir des entités extraites ? | **Ne** -les deux options fera probablement. Toutefois, si la charge utile de que l’apport de votre requête n’est pas trop volumineuse, que vous pouvez rencontrer des gains de performance à l’aide d’un chargement hâtif comme il allez nécessiter moins des boucles afin de matérialiser les objets de votre réseau. <br/> <br/> **Oui** -si vous avez besoin accéder aux nombreuses propriétés de navigation à partir des entités, vous le feriez qu’en utilisant plusieurs incluent des instructions dans votre requête avec un chargement hâtif. Les entités plus vous incluez, plus la charge utile de votre requête renvoie. Une fois que vous incluez au moins trois entités dans votre requête, songez à passer au mode différé du chargement. |
| Vous savez exactement quelles données seront nécessaires au moment de l’exécution ?                   | **Ne** -chargement différé sera mieux pour vous. Sinon, vous risquez d’interrogation de données que vous n’aurez pas. <br/> <br/> **Oui** - hâtif le chargement est probablement la meilleure solution ; elle vous aide à charger plus rapidement des jeux entière. Si votre requête nécessite l’extraction d’une très grande quantité de données, et cela devient trop lent, essayez de différé le chargement à la place.                                                                                                                                                                                                                                                       |
| Votre code s’exécute loin d’être votre base de données ? (latence accrue du réseau)  | **Ne** : lorsque la latence du réseau n’est pas un problème, à l’aide de chargement différé peut simplifier votre code. N’oubliez pas que la topologie de votre application peut changer, afin de la proximité de la base de données ne prennent pas pour reçoivent. <br/> <br/> **Oui** : quand le réseau est un problème, que vous pouvez décider de ce qui s’adapte mieux pour votre scénario. En général, le chargement hâtif seront améliorée, car elle nécessite moins d’allers-retours.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 problèmes de performances d’avec inclut plusieurs

Lorsque nous entendons les questions de performances qui impliquent des problèmes de temps de réponse de serveur, la source du problème est fréquemment des requêtes avec plusieurs instructions Include. Y compris les entités connexes dans une requête est puissant, il est important de comprendre ce qui se passe en coulisses.

Il prend un certain temps pour une requête avec plusieurs instructions Include qui passent par notre compilateur plan interne pour produire la commande de stockage. L’essentiel de ce temps est consacré à la tentative d’optimisation de la requête résultante. La commande de stockage généré contiendra un Outer Join ou une Union pour chaque Include, en fonction de votre mappage. Ce type de requête s’affiche dans les grands graphiques connectés à partir de votre base de données dans une charge utile unique, qui sera acerbate des problèmes de bande passante, en particulier lorsqu’il y a beaucoup de redondance dans la charge utile (par exemple, lorsque plusieurs niveaux d’inclusion sont utilisés pour parcourir associations dans le sens d’un-à-plusieurs).

Vous pouvez vérifier pour les cas où vos requêtes retournent excessivement grandes charges utiles en accédant à la TSQL sous-jacent pour la requête à l’aide de ToTraceString et l’exécution de la commande de stockage dans SQL Server Management Studio pour afficher la taille de la charge utile. Dans ce cas, que vous pouvez essayer de réduire le nombre d’instructions Include dans votre requête juste mettre dans les données que vous avez besoin. Ou vous pourrez peut-être diviser votre requête en une séquence plus petits de sous-requêtes, par exemple :

**Avant de s’arrêter la requête :**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Après l’arrêt de la requête :**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Cela fonctionne uniquement sur les requêtes suivies, car nous effectuons l’utilisation de la capacité le contexte a pour accomplir automatiquement des correction association et la résolution d’identité.

Comme avec le chargement différé, le compromis sera plus de requêtes de plus petites charges utiles. Vous pouvez également utiliser des projections de propriétés individuelles pour sélectionner explicitement uniquement les données que vous avez besoin à partir de chaque entité, mais vous ne serez pas charger les entités dans ce cas, et mises à jour ne seront pas pris en charge.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 solution de contournement pour obtenir le chargement différé des propriétés

Entity Framework ne prend en charge le chargement différé des propriétés scalaires ou complexes. Toutefois, dans les cas où vous avez une table qui inclut un objet volumineux comme un objet BLOB, vous pouvez utiliser le fractionnement de table pour séparer les propriétés de grande taille dans une entité distincte. Par exemple, supposons que vous avez une table Product qui inclut une colonne de photo varbinary. Si vous n’avez pas souvent besoin pour accéder à cette propriété dans vos requêtes, vous pouvez utiliser la table de fractionnement pour importer uniquement les parties de l’entité que vous devez normalement. L’entité représentant la photo du produit ne sera chargée lorsque vous en avez explicitement besoin.

Une bonne ressource qui montre comment activer le fractionnement de table est le billet de blog « Table fractionnement dans Entity Framework » de Gil Fink : \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 autres considérations

### <a name="91------server-garbage-collection"></a>9.1 Garbage Collection côté serveur de

Certains utilisateurs peuvent observer des conflits de ressources qui limite le parallélisme que s’attendent lorsque le Garbage Collector n’est pas configurée correctement. Chaque fois qu’EF est utilisé dans un scénario multithread, ou dans n’importe quelle application qui ressemble à un système côté serveur, veillez à activer des Garbage Collection côté serveur. Cette opération est effectuée via un simple paramètre dans votre fichier de configuration d’application :

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Cela doit réduire votre conflits de threads et augmenter le débit en jusqu'à 30 % dans les scénarios de processeur ne soit saturé. En règle générale, vous devez toujours vérifier de façon dont votre application comporte à l’aide de la Garbage Collection classique (qui mieux met l’accent sur des scénarios côté client et de l’interface utilisateur), ainsi que le Garbage Collection côté serveur.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges

Comme mentionné précédemment, Entity Framework peut afficher les problèmes de performances lorsque le cache d’objets a de nombreuses entités. Certaines opérations, telles que Add, Remove, recherche, écriture et SaveChanges, déclenchent des appels à DetectChanges qui peut consommer une grande quantité d’UC en fonction de la taille du cache de l’objet est devenu. La raison en est que le cache d’objets, l’objet état manager tentez resteront synchronisées que possible sur chaque opération effectuée à un contexte, afin que les données de produit sont garantie comme étant correct sous un large éventail de scénarios.

Il est généralement une bonne pratique de laisser détection automatique des modifications d’Entity Framework est activé pendant toute la vie de votre application. Si votre scénario est négativement affecté par une utilisation élevée du processeur et vos profils indiquent que la cause du problème est l’appel à DetectChanges, envisagez de désactiver temporairement AutoDetectChanges dans la partie sensible de votre code :

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Avant de désactiver AutoDetectChanges, il est judicieux de comprendre que cela pourrait perdre sa capacité à effectuer le suivi de certaines informations sur les modifications qui se produisent sur les entités d’Entity Framework. Si pas correctement traités, cela peut entraîner une incohérence des données sur votre application. Pour plus d’informations sur la désactivation de AutoDetectChanges lire \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 contexte par demande

Les contextes d’Entity Framework sont destinés à être utilisés comme expérience de courte durée de vie instances afin de fournir des performances optimales. Contextes sont supposés être courts vécu et ignorées et par conséquent ont été implémentées pour être très légers et reutilize métadonnées autant que possible. Dans les scénarios web, il est important de garder cela à l’esprit et un contexte n’est plus longtemps que la durée d’une demande unique. De même, dans les scénarios non web contexte doit être ignoré en fonction de votre compréhension des différents niveaux de mise en cache dans Entity Framework. En règle générale, un doit éviter d’avoir une instance de contexte tout au long de la durée de vie de l’application, ainsi que les contextes par thread et contextes statiques.

### <a name="94------database-null-semantics"></a>9.4 sémantique null de base de données

Entity Framework par défaut génère du code SQL a C\# null sémantique de comparaison. Considérez l’exemple de requête suivant :

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

Dans cet exemple, nous comparons un nombre de variables nullables par rapport à des propriétés nullable sur l’entité, telles que SupplierID et UnitPrice. Le SQL généré pour cette requête vous demande si la valeur du paramètre est le même que la valeur de colonne, ou si le paramètre et les valeurs de colonne sont null. Cela masquera la manière dont le serveur de base de données gère les valeurs NULL et fournira un C cohérente\# null expérience entre les différents fournisseurs de base de données différente. En revanche, le code généré est un peu compliqué et peut s’avérer bien quand le nombre de comparaisons dont la déclaration de la requête a atteint un grand nombre.

Une façon de gérer cette situation est à l’aide de la sémantique null de base de données. Notez que cela peut se comporter potentiellement différemment la c\# null sémantique depuis maintenant Entity Framework génère SQL plus simple qui expose la manière dont le moteur de base de données gère les valeurs null. Sémantique null de base de données peut être activé par contexte avec une seule ligne de configuration unique par rapport à la configuration de contexte :

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Petite à moyenne taille des requêtes n’affichera pas une amélioration des performances perceptible lors de l’utilisation de la sémantique null de base de données, mais la différence devient plus notable sur les requêtes avec un grand nombre de comparaisons de valeurs null potentielles.

Dans l’exemple de requête ci-dessus, la différence de performances était inférieure à 2 % dans un microbenchmark en cours d’exécution dans un environnement contrôlé.

### <a name="95------async"></a>9.5 Async

Prise en charge de Entity Framework 6 a introduit des opérations asynchrones lors de l’exécution sur .NET 4.5 ou version ultérieure. La plupart du temps, les applications qui ont des e/s liés contention bénéficieront le meilleur parti de l’aide d’une requête asynchrone et les opérations de sauvegarde. Si votre application ne soit pas soumise à partir de la contention d’e/s, l’utilisation des opérations asynchrones est, dans le meilleur des cas, exécuter de façon synchrone et retournent le résultat dans la même quantité de temps qu’un appel synchrone, ou dans le pire des cas, simplement différer l’exécution pour une tâche asynchrone et ajoutez tim supplémentaire e à la fin de votre scénario.

Pour plus d’informations sur le travail de programmation asynchrone qui aide à vous de décider si async améliorera les performances de votre application visitez [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Pour plus d’informations sur l’utilisation d’opérations asynchrones sur Entity Framework, consultez [requête asynchrone et enregistrement](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entity Framework 6 n’est pas fourni dans l’installation par défaut du .NET framework. Par conséquent, les assemblys d’Entity Framework ne sont pas que gérés avec NGEN ne par défaut, ce qui signifie que tout le code d’Entity Framework est soumis aux mêmes charges JIT'ing comme tout autre assembly MSIL. Cela peut dégrader l’expérience F5 lors du développement et également sur le démarrage à froid de votre application dans les environnements de production. Afin de réduire les coûts du processeur et mémoire du JIT'ing, il est conseillé à Entity Framework d’images comme il convient de NGEN. Pour plus d’informations sur la façon d’améliorer les performances de démarrage d’Entity Framework 6 avec NGEN, consultez [amélioration des performances de démarrage avec NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>9.7 code First et EDMX

Raisons de Framework d’entité sur le problème de différence d’impédance entre la programmation orientée objet et des bases de données relationnelles en utilisant une représentation en mémoire du modèle conceptuel (objets), le schéma de stockage (base de données) et un mappage entre le deux. Ces métadonnées sont appelée un Entity Data Model ou EDM pour faire plus court. À partir de ce modèle EDM, Entity Framework sera dériver les vues aux données de l’aller-retour à partir des objets en mémoire à la base de données et sauvegarder.

Quand Entity Framework est utilisé avec un fichier EDMX autrement formellement Spécifie le modèle conceptuel, le schéma de stockage et le mappage, puis l’étape de chargement du modèle a uniquement valider que le modèle EDM est correct (par exemple, n’Assurez-vous qu’aucun mappage manquant), puis générer les vues, puis valider les vues et avoir ces métadonnées prêt à être utilisé. Uniquement peut ensuite une requête est exécutée ou nouvelles données enregistrées dans le magasin de données.

L’approche Code First est, à son cœur, un générateur d’Entity Data Model sophistiqué. Entity Framework a produire un modèle EDM à partir du code fourni ; Il fait en analysant les classes impliquées dans le modèle, l’application des conventions et la configuration du modèle via l’API Fluent. Une fois le modèle EDM est généré, Entity Framework essentiellement se comporte de la même façon qu’il aurait eu un fichier EDMX étaient présent dans le projet. Par conséquent, la génération du modèle à partir de Code First ajoute une complexité supplémentaire qui se traduit par un temps de démarrage plus lent pour Entity Framework par rapport à avoir un EDMX. Le coût est complètement dépendant de la taille et la complexité du modèle qui est en cours de génération.

Lorsque vous choisissez d’utiliser EDMX et Code First, il est important de savoir que la flexibilité introduite par Code First augmente le coût de la génération du modèle pour la première fois. Si votre application peut supporter le coût de cette charge de la première fois, en général, Code First sera la meilleure façon d’accéder.

## <a name="10-investigating-performance"></a>Performances de l’examen des 10

### <a name="101-using-the-visual-studio-profiler"></a>10.1 à l’aide du Profiler de Visual Studio

Si vous rencontrez des problèmes de performances avec Entity Framework, vous pouvez utiliser un profileur semblable à celui intégré dans Visual Studio pour voir où votre application passe son temps. Ceci est l’outil que nous avons utilisé pour générer les graphiques à secteurs dans le billet de blog « Exploring the Performance d’ADO.NET Entity Framework - partie 1 » ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) qui indiquent où Entity Framework consacre son temps pendant les requêtes à froid et à chaudes.

Le billet de blog « Profilage Entity Framework à l’aide de la Profiler de 2010 Visual Studio » écrit par les données et la modélisation Customer Advisory Team montre un exemple concret d’utilisation qu’ils le profileur pour étudier un problème de performances.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Cet article a été écrit pour une application windows. Si vous avez besoin pour profiler une application web les outils de l’enregistreur de Performance Windows (WPR) et Windows Performance Analyzer (WPA) peuvent fonctionner mieux que de travailler à partir de Visual Studio. WPR et WPA font partie de la boîte à outils de performances Windows, qui est inclus avec le Kit de déploiement et d’évaluation de Windows ( [http://www.microsoft.com/en-US/download/details.aspx?id=39982](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 application/base de données de profilage

Outils tels que le profileur intégrées à Visual Studio vous indiquent où votre application passe de temps.  Un autre type de profileur est disponible qui réalise une analyse dynamique de votre application en cours d’exécution, soit en production ou de préproduction en fonction des besoins et recherche les pièges courants et anti-schémas d’accès de base de données.

Deux profileurs de code disponibles dans le commerce sont le Profiler de Framework d’entité ( \<http://efprof.com>) et ORMProfiler ( \<http://ormprofiler.com>).

Si votre application est une application MVC à l’aide de Code First, vous pouvez utiliser MiniProfiler de StackExchange. Scott Hanselman décrit cet outil dans son blog à l’adresse : \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Pour plus d’informations sur le profilage d’activité de base de données de votre application, consultez l’article de Julie MSDN Magazine intitulé [activité de base de données de profilage dans Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 enregistreur d’événements de base de données

Si vous utilisez Entity Framework 6 également envisager d’utiliser la fonctionnalité de journalisation intégrées. La propriété de base de données du contexte peut être paramétrée pour se connecter à son activité via une simple configuration d’une seule ligne :

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

Dans cet exemple, l’activité de base de données sera enregistrée dans la console, mais la propriété du journal peut être configurée pour appeler une Action&lt;chaîne&gt; déléguer.

Si vous souhaitez activer la journalisation de base de données sans devoir recompiler et vous utilisez Entity Framework 6.1 ou version ultérieure, vous pouvez le faire en ajoutant un intercepteur dans le fichier web.config ou app.config de votre application.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Pour plus d’informations sur la façon d’ajouter la journalisation sans avoir à recompiler aller à \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Annexe 11

### <a name="111-a-test-environment"></a>11.1 environnement de Test d’a.

Cet environnement utilise un programme d’installation 2-machine avec la base de données sur un ordinateur distinct de l’application cliente. Les machines sont dans le même rack, par conséquent, la latence du réseau est relativement faible, mais plus réaliste qu’un environnement d’ordinateur unique.

#### <a name="1111-------app-server"></a>11.1.1 application serveur

##### <a name="11111------software-environment"></a>11.1.1.1 environnement logiciel

-   Environnement de logiciel Entity Framework 4
    -   Nom du système d’exploitation : Windows Server 2008 R2 SP1 Enterprise.
    -   Visual Studio 2010 Ultimate.
    -   Visual Studio 2010 SP1 (uniquement pour certaines comparaisons).
-   Environnement de logiciel Entity Framework 5 et 6
    -   Nom du système d’exploitation : Windows 8.1 entreprise
    -   Visual Studio 2013 – Édition intégrale.

##### <a name="11112------hardware-environment"></a>11.1.1.2 environnement matériel

-   Biprocesseur : Intel (r) Xeon (r) du processeur L5520 W3530 à 2,27 GHz, 2261 Mhz8 GHz, 4 cœurs, 84 ou des processeurs logiques.
-   Go 2412 RamRAM.
-   136 lecteur Go SCSI250GB SATA 7200 de tr/min / 3GB/s divisé en 4 partitions.

#### <a name="1112-------db-server"></a>11.1.2 serveur de base de données

##### <a name="11121------software-environment"></a>11.1.2.1 environnement logiciel

-   Nom du système d’exploitation : Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 environnement matériel

-   Unique processeur : Intel (r) Xeon (r) du processeur L5520 2,27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 cœurs, 8 processeurs logiques.
-   Go 824 RamRAM.
-   465 lecteur Go ATA500GB SATA 7200 de rpm 6 Go/s en 4 parties.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 tests de comparaison de performances de requête de B.

Le modèle Northwind a été utilisé pour exécuter ces tests. Il a été généré à partir de la base de données à l’aide du Concepteur d’Entity Framework. Ensuite, le code suivant a été utilisé pour comparer les performances des options de l’exécution de requête :

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>Modèle de Navision 11.3 C.

La base de données Navision est une base de données volumineux utilisé pour la démonstration de Microsoft Dynamics : barre de navigation. Le modèle conceptuel généré contient les jeux d’entités de 1005 et 4227 ensembles d’associations. Le modèle utilisé dans le test n’est « plat » – aucun héritage a été ajouté à ce dernier.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 requêtes utilisés pour les tests de Navision

La liste de requêtes utilisée avec le modèle Navision contient 3 catégories de requêtes Entity SQL :

##### <a name="11311-lookup"></a>11.3.1.1 recherche de

Une requête de recherche simple sans agrégations

-   Nombre : 16232
-   Exemple :

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Une requête de BI normale avec plusieurs agrégations, mais aucun sous-total (requête unique)

-   Nombre : 2313
-   Exemple :

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Où MDF\_SessionLogin\_temps\_Max() est défini dans le modèle en tant que :

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Une requête de BI avec des agrégations et les sous-totaux (via l’union de toutes les)

-   Nombre : 178
-   Exemple :

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
