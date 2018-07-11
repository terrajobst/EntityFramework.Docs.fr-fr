---
title: Portage depuis EF6 vers EF Core - valider les exigences
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949112"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Avant le portage depuis EF6 vers EF Core : valider les exigences de votre Application

Avant de commencer le processus de portage, il est important de valider qu’EF Core répond à l’accès aux données pour votre application.

## <a name="missing-features"></a>Fonctionnalités manquantes

Assurez-vous que EF Core possède toutes les fonctionnalités que vous devez utiliser dans votre application. Consultez [comparaison des fonctionnalités](../features.md) pour une comparaison détaillée de la fonctionnalité définie dans EF Core comparatif entre EF6. Si toutes les fonctionnalités requises sont manquantes, vérifiez que vous pouvez pallier l’absence de ces fonctionnalités avant de porter vers EF Core.

## <a name="behavior-changes"></a>Changements de comportement

Il s’agit d’une liste non exhaustive de certaines modifications de comportement entre EF6 et EF Core. Il est important de garder ces pratiques comme votre port de votre application car elles peuvent changer la façon de votre application se comporte, mais n’apparaît pas comme des erreurs de compilation après le remplacement à EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportement DbSet.Add/Attach et graph

Dans EF6, l’appel `DbSet.Add()` sur les résultats de l’entité dans une recherche récursive pour toutes les entités référencées dans ses propriétés de navigation. Toutes les entités qui sont trouvent et ne sont pas suivies par le contexte, sont également être marqué comme ajouté. `DbSet.Attach()` se comporte de la même, mais toutes les entités sont marquées comme inchangé.

**EF Core effectue une recherche récursive similaire, mais avec certaines des règles légèrement différentes.**

*  L’entité racine est toujours dans l’état requis (ajoutée pour `DbSet.Add` et inchangée pour `DbSet.Attach`).

*  **Pour les entités qui se trouvent au cours de la recherche récursive des propriétés de navigation :**

    *  **Si la clé primaire de l’entité est généré de magasin**

        * Si la clé primaire n’est pas définie sur une valeur, l’état est défini à ajouté. La valeur de clé primaire est considérée comme « non définie » si elle est affectée à la valeur par défaut CLR pour le type de propriété (par exemple, `0` pour `int`, `null` pour `string`, etc..).

        * Si la clé primaire est définie sur une valeur, l’état a inchangé.

    *  Si la clé primaire n’est pas généré de base de données, l’entité est placée dans le même état que la racine.

### <a name="code-first-database-initialization"></a>Première initialisation de base de données de code

**EF6 a beaucoup de magie qu’il effectue autour de la sélection de la connexion de base de données et de l’initialisation de la base de données. Certaines de ces règles sont les suivantes :**

* Si aucune configuration n’est effectuée, EF6 sélectionne une base de données sur SQL Express ou LocalDb.

* Si une chaîne de connexion avec le même nom que le contexte est dans les applications `App/Web.config` fichier, cette connexion est utilisée.

* Si la base de données n’existe pas, il est créé.

* Si aucune des tables à partir du modèle existent dans la base de données, le schéma pour le modèle actuel est ajouté à la base de données. Si les migrations sont activées, ils sont utilisés pour créer la base de données.

* Si la base de données existe et EF6 avait précédemment créé le schéma, le schéma est vérifié pour assurer la compatibilité avec le modèle actuel. Une exception est levée si le modèle a changé depuis que le schéma a été créé.

**EF Core n’effectue aucun de cette commande magique.**

* La connexion de base de données doit être configurée de manière explicite dans le code.

* Aucune initialisation n’est effectuée. Vous devez utiliser `DbContext.Database.Migrate()` pour appliquer des migrations (ou `DbContext.Database.EnsureCreated()` et `EnsureDeleted()` pour créer ou supprimer de la base de données sans utiliser de migrations).

### <a name="code-first-table-naming-convention"></a>Code tout d’abord une convention d’affectation de noms de table

EF6 s’exécute le nom de classe d’entité via un service de pluralisation pour calculer le nom de table par défaut l’entité est mappée à.

EF Core utilise le nom de la `DbSet` propriété l’entité sur laquelle est exposée dans le contexte dérivé. Si l’entité n’a pas un `DbSet` propriété, le nom de classe est utilisé.
