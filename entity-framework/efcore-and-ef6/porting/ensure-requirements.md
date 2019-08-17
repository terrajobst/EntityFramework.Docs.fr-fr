---
title: Portage des exigences de EF6 à EF Core-valider
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565342"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Avant le portage de EF6 vers EF Core: Valider les exigences de votre application

Avant de commencer le processus de Portage, il est important de vérifier que EF Core répond aux exigences d’accès aux données pour votre application.

## <a name="missing-features"></a>Fonctionnalités manquantes

Assurez-vous que EF Core dispose de toutes les fonctionnalités que vous devez utiliser dans votre application. Consultez [comparaison des fonctionnalités](../features.md) pour une comparaison détaillée de la façon dont la fonctionnalité définie dans EF Core est comparée à EF6. Si des fonctionnalités requises sont manquantes, assurez-vous que vous pouvez compenser l’absence de ces fonctionnalités avant de procéder au portage vers EF Core.

## <a name="behavior-changes"></a>Changements de comportement

Il s’agit d’une liste non exhaustive de modifications de comportement entre EF6 et EF Core. Il est important de garder à l’esprit que le port de votre application peut modifier le comportement de votre application, mais qu’elle n’apparaît pas en tant qu’erreurs de compilation après l’échange de EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet. Add/Attach et le comportement du graphique

Dans EF6, l' `DbSet.Add()` appel de sur une entité entraîne une recherche récursive pour toutes les entités référencées dans ses propriétés de navigation. Toutes les entités qui sont détectées et qui ne sont pas déjà suivies par le contexte sont également marquées comme ajoutées. `DbSet.Attach()`se comporte de la même manière, sauf que toutes les entités sont marquées comme inchangées.

**EF Core effectue une recherche récursive similaire, mais avec des règles légèrement différentes.**

*  L’entité racine est toujours dans l’État demandé (ajouté pour `DbSet.Add` et inchangé pour `DbSet.Attach`).

*  **Pour les entités qui sont détectées lors de la recherche récursive des propriétés de navigation:**

    *  **Si la clé primaire de l’entité est générée par le magasin**

        * Si la clé primaire n’est pas définie sur une valeur, l’État est défini sur ajouté. La valeur de clé primaire est considérée comme «non définie» si elle est affectée à la valeur CLR par défaut pour le type de `0` propriété `int`(par `string`exemple, pour, `null` pour, etc.).

        * Si la clé primaire est définie sur une valeur, l’État est défini sur inchangé.

    *  Si la clé primaire n’est pas générée par la base de données, l’entité est placée dans le même État que la racine.

### <a name="code-first-database-initialization"></a>Initialisation de la base de données Code First

**EF6 dispose d’un grand nombre de Magic pour la sélection de la connexion de base de données et l’initialisation de la base de données. Parmi ces règles, citons les suivantes:**

* Si aucune configuration n’est effectuée, EF6 sélectionne une base de données sur SQL Express ou sur une base de données locale.

* Si une chaîne de connexion portant le même nom que le contexte se trouve dans `App/Web.config` le fichier d’application, cette connexion est utilisée.

* Si la base de données n’existe pas, elle est créée.

* Si aucune des tables du modèle n’existe dans la base de données, le schéma du modèle actuel est ajouté à la base de données. Si les migrations sont activées, elles sont utilisées pour créer la base de données.

* Si la base de données existe et que EF6 a déjà créé le schéma, la compatibilité du schéma avec le modèle actuel est vérifiée. Une exception est levée si le modèle a changé depuis la création du schéma.

**EF Core n’effectue pas l’une de ces magie.**

* La connexion de base de données doit être configurée de manière explicite dans le code.

* Aucune initialisation n’est effectuée. Vous devez utiliser `DbContext.Database.Migrate()` pour appliquer des migrations ( `DbContext.Database.EnsureCreated()` ou `EnsureDeleted()` et pour créer/supprimer la base de données sans utiliser les migrations).

### <a name="code-first-table-naming-convention"></a>Convention d’affectation de noms de table Code First

EF6 exécute le nom de la classe d’entité via un service de pluralisation pour calculer le nom de la table par défaut à laquelle l’entité est mappée.

EF Core utilise le nom de la `DbSet` propriété dans laquelle l’entité est exposée dans le contexte dérivé. Si l’entité n’a pas de `DbSet` propriété, le nom de la classe est utilisé.
