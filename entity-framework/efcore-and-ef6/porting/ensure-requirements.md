---
title: Portage de EF6 vers EF Core - valider la configuration requise
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052859"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Avant de porter à partir de EF6 vers EF Core : valider des exigences de votre Application

Avant de commencer le processus de portage, il est important de vérifier qu’EF Core respecte les conditions d’accès de données pour votre application.

## <a name="missing-features"></a>Fonctionnalités manquantes

Assurez-vous que EF Core possède toutes les fonctionnalités que vous devez utiliser dans votre application. Consultez [comparaison des fonctionnalités](../features.md) pour une comparaison détaillée de la différence entre la fonctionnalité définie dans EF Core à EF6. Si toutes les fonctionnalités requises sont manquantes, vérifiez que vous pouvez compenser l’absence de ces fonctionnalités avant de porter vers EF Core.

## <a name="behavior-changes"></a>Changements de comportement

Il s’agit d’une liste non exhaustive de certaines modifications de comportement entre EF6 et EF Core. Il est important de garder ces n’oubliez pas que votre port votre application car elles peuvent changer la façon de votre application se comporte, mais pas apparaîtront en tant qu’erreurs de compilation après le remplacement à EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportement de DbSet.Add/Attach et de graphique

Dans EF6, appelant `DbSet.Add()` sur les résultats dans une recherche récursive pour toutes les entités référencées dans ses propriétés de navigation pour une entité. Toutes les entités qui sont trouvées et ne sont pas suivies par le contexte, sont également être marqué comme ajouté. `DbSet.Attach()`se comporte de la même, mais toutes les entités sont marquées comme non modifié.

**EF Core effectue une recherche récursive similaire, mais avec certaines des règles légèrement différentes.**

*  L’entité racine est toujours dans l’état requis (ajouté pour `DbSet.Add` et inchangée pour `DbSet.Attach`).

*  **Pour les entités qui sont détectés lors de la recherche récursive des propriétés de navigation :**

    *  **Si la clé primaire de l’entité est généré de magasin**

        * Si la clé primaire n’est pas définie à une valeur, l’état est défini à ajouté. La valeur de clé primaire est considéré comme « non définie » si elle est affectée à la valeur par défaut CLR pour le type de propriété (c'est-à-dire `0` pour `int`, `null` pour `string`, etc..).

        * Si la clé primaire est définie sur une valeur, l’état a inchangé.

    *  Si la clé primaire n’est pas généré de base de données, l’entité est placée dans le même état que la racine.

### <a name="code-first-database-initialization"></a>Première initialisation de base de données de code

**EF6 a une quantité importante de magic qu'il effectue autour de la sélection de la connexion de base de données et de l’initialisation de la base de données. Certaines de ces règles sont les suivantes :**

* Si aucune configuration n’est effectuée, EF6 sélectionne une base de données SQL Express ou LocalDb.

* Si une chaîne de connexion avec le même nom que le contexte se trouve dans les applications `App/Web.config` fichier, cette connexion sera utilisée.

* Si la base de données n’existe pas, il est créé.

* Si aucune des tables à partir du modèle existent dans la base de données, le schéma pour le modèle actuel est ajouté à la base de données. Si les migrations sont activées, ils sont utilisés pour créer la base de données.

* Si la base de données existe et EF6 précédemment créée le schéma, le schéma est vérifié pour assurer la compatibilité avec le modèle actuel. Une exception est levée si le modèle a été modifié depuis que le schéma a été créé.

**EF Core n’effectue aucun cet aspect le plus magique.**

* La connexion de base de données doit être configurée de manière explicite dans le code.

* Aucune initialisation n’est effectuée. Vous devez utiliser `DbContext.Database.Migrate()` pour appliquer des migrations (ou `DbContext.Database.EnsureCreated()` et `EnsureDeleted()` pour la création/suppression de la base de données sans l’utilisation de migrations).

### <a name="code-first-table-naming-convention"></a>Code tout d’abord une convention d’affectation de noms de table

EF6 exécute le nom de la classe d’entité via un service de pluralisation pour calculer le nom de table par défaut que l’entité est mappée à.

EF Core utilise le nom de la `DbSet` propriété de l’entité sur laquelle est exposée dans le contexte dérivé. Si l’entité n’a pas un `DbSet` propriété, le nom de classe est utilisé.
