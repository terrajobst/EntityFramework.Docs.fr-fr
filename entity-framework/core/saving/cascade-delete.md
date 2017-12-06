---
title: Cascade Delete - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>Suppression en cascade

Suppression en cascade est couramment utilisée dans la terminologie de base de données pour décrire une caractéristique qui permet la suppression d’une ligne pour déclencher automatiquement la suppression de lignes connexes. Un concept étroitement lié également couvert par les comportements de suppression EF cœur est la suppression automatique d’une entité enfant lorsqu’il est la relation à un parent a été interrompue--cette i communément appelée « suppression des orphelins ».

EF Core implémente plusieurs comportements de suppression différente et permet la configuration des comportements de suppression de relations individuelles. EF Core implémente également des conventions de configurer automatiquement les comportements de suppression par défaut utiles pour chaque relation basée sur le [requiredness de la relation] (.. /Modeling/Relationships.MD#Required-and-Optional-Relationships).

## <a name="delete-behaviors"></a>Supprimer des comportements
Supprimer les comportements sont définis dans le *deleteBehavior()* énumérateur de type et peut être passée à la *OnDelete* API fluent au contrôle si la suppression d’une entité principal/le parent ou l’interruption de la relation à des entités dépendantes/enfant doit avoir un effet secondaire sur les entités dépendantes/enfant.

Il existe trois actions QU'EF peut prendre lorsqu’un principal/le parent est supprimée ou que la relation à l’enfant est interrompue :
* L’enfant/dépendant peut être supprimé.
* Les valeurs de clé étrangère de l’enfant peuvent être définies avec la valeur null
* L’enfant reste inchangé

> [!NOTE]  
> Le comportement de suppression configuré dans le modèle EF principal est uniquement appliqué lorsque l’entité principale est supprimée à l’aide de EF Core et les entités dépendantes sont chargées en mémoire (par exemple, pour les suivis dépendants). Un comportement de cascade correspondant doit se trouver le programme d’installation dans la base de données qui ne sont pas suivies par le contexte a l’action requise est appliquée. Si vous utilisez EF Core pour créer la base de données, ce comportement en cascade sera installé pour vous.

Pour la seconde action ci-dessus, il n’est pas valide si la clé étrangère n’accepte pas d’affectant une valeur de clé étrangère la valeur null. (Une clé étrangère non nullable est équivalente à une relation requise.) Dans ces cas, EF Core suit que la propriété de clé étrangère a été marquée comme null jusqu'à ce que SaveChanges est appelée, auquel cas une exception est levée, car la modification ne peut pas être conservée dans la base de données. Cela est similaire à une violation de contrainte à partir de la base de données.

Il existe quatre supprimer des comportements, répertoriés dans les tableaux ci-dessous. Pour les relations facultatif (clé étrangère nullable) il _est_ possible d’enregistrer une valeur null valeur de clé étrangère, ce qui entraîne les conséquences suivantes :

| Nom du comportement | Effet sur dépendant/enfant dans la mémoire | Effet sur dépendant/enfant dans la base de données
|-|-|-
| **Cascade** | Les entités sont supprimées. | Les entités sont supprimées.
| **ClientSetNull** (par défaut) | Propriétés de clé étrangère sont définies avec la valeur null | None
| **SetNull** | Propriétés de clé étrangère sont définies avec la valeur null | Propriétés de clé étrangère sont définies avec la valeur null
| **Restreindre** | None | None

Pour les relations requises (clé étrangère non nullable) il est _pas_ possible d’enregistrer une valeur null valeur de clé étrangère, ce qui entraîne les conséquences suivantes :

| Nom du comportement | Effet sur dépendant/enfant dans la mémoire | Effet sur dépendant/enfant dans la base de données
|-|-|-
| **Cascade** (par défaut) | Les entités sont supprimées. | Les entités sont supprimées.
| **ClientSetNull** | Lève une exception SaveChanges | None
| **SetNull** | Lève une exception SaveChanges | Lève une exception SaveChanges
| **Restreindre** | None | None

Dans les tableaux ci-dessus, *aucun* peut entraîner une violation de contrainte. Par exemple, si une entité principal/enfant est supprimée, mais aucune action n’est effectuée pour modifier la clé étrangère de dépendant/enfant, puis la base de données probablement lève sur SaveChanges en raison d’une violation de contrainte foreign.

À un niveau élevé :
* Si vous avez des entités qui ne peut pas exister sans un parent, et vous souhaitez EF pour prendre en charge pour la suppression des enfants automatiquement, puis utilisez *Cascade*.
  * Entités qui ne peut pas exister sans un parent Assurez-vous généralement utiliser les relations requises, pour lequel *Cascade* est la valeur par défaut.
* Si vous avez des entités qui peuvent ou ne peuvent pas avoir un parent, et vous souhaitez EF pour prendre en charge de la mise à la clé étrangère pour vous, puis utilisez *ClientSetNull*
  * Entités qui peuvent exister sans un parent Assurez-vous généralement utiliser des relations facultatif, pour lequel *ClientSetNull* est la valeur par défaut.
  * Si vous souhaitez que la base de données également essayer de propager les valeurs null pour les clés étrangères enfants même lorsque l’entité enfant n’est pas chargée, puis utilisez *SetNull*. Toutefois, notez que la base de données doit prendre en charge cette et configuration de la base de données, comme cela peut entraîner dans d’autres restrictions, qui dans la pratique rend souvent cette option inapproprié. C’est pourquoi *SetNull* n’est pas la valeur par défaut.
* Si vous ne souhaitez pas Core EF jamais supprimer une entité automatiquement ou annuler automatiquement la clé étrangère, puis utiliser *Restrict*. Notez que cela nécessite que votre code de synchroniser les entités enfants et leurs valeurs de clé étrangère manuellement dans le cas contraire les exceptions de contrainte seront levées.

> [!NOTE]
> Dans Core EF, contrairement à EF6, effets en cascade ne produisent pas immédiatement, mais à la place uniquement lorsque SaveChanges est appelée.

> [!NOTE]  
> **Modifications dans EF Core 2.0 :** dans les versions précédentes, *Restrict* provoquerait des propriétés de clé étrangère facultatif dans des entités dépendantes suivies soit définie sur null et a été le comportement pour les relations de l’optionnels de suppression de la valeur par défaut. Dans EF Core 2.0, le *ClientSetNull* a été introduit pour représenter ce comportement et est devenue la valeur par défaut pour les relations facultatif. Le comportement de *Restrict* a été ajusté pour ne jamais avoir d’effets sur les entités dépendantes.

## <a name="entity-deletion-examples"></a>Exemples de suppression d’entité

Le code suivant fait partie d’un [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) une exécution qui peut être téléchargé. L’exemple montre que se passe-t-il pour chaque comportement de suppression pour les relations facultatives et requises lorsqu’une entité parente est supprimée.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Nous allons étudier chaque variation de comprendre ce qui se passe.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade avec une relation obligatoire ou facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Blog est marqué comme supprimé
* Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges
* SaveChanges envoie des suppressions pour les dépendances/enfants (publie), puis le principal/parent (blog)
* Après l’enregistrement, toutes les entités sont détachées, car ils ont été supprimées à partir de la base de données

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec relation requise

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog est marqué comme supprimé
* Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges
* SaveChanges tente de définir la publication FK null, mais cette opération échoue car la clé étrangère ne peut pas être null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog est marqué comme supprimé
* Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges
* Tentatives de SaveChanges définit la clé étrangère à la fois (publie) / enfants dépendants null avant de supprimer le principal/parent (blog)
* Après l’enregistrement, le principal/parent (blog) est supprimé, mais les objets dépendants/enfants (publie) sont toujours suivies
* Les objets dépendants suivies/enfants (publie) ont maintenant des valeurs FK null et leur référence vers le principal/parents (blog) a été supprimé.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict avec une relation obligatoire ou facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog est marqué comme supprimé
* Publications restent initialement Unchanged étant donné que les cascades ne se produisent pas tant que SaveChanges
* Étant donné que *Restrict* indique EF pas automatiquement de définir la clé étrangère sur null, elle reste non null et SaveChanges lève sans enregistrer

## <a name="delete-orphans-examples"></a>Supprimer les exemples orphelins

Le code suivant fait partie d’un [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) une exécution qui peut être téléchargé. L’exemple montre que se passe-t-il pour chaque comportement de suppression pour les relations facultatives et requises lors de la relation entre un parent/principal et ses éléments enfants/dépendants est interrompue. Dans cet exemple, la relation est rompue en supprimant les objets dépendants/enfants (publie) à partir de la propriété de navigation de collection sur le principal/parent (blog). Toutefois, le comportement est le même si la référence de dépendant/enfant au principal/le parent est annulée à la place.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Nous allons étudier chaque variation de comprendre ce qui se passe.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade avec une relation obligatoire ou facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null
  * Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null
* SaveChanges envoie les suppression pour les objets dépendants/enfants (publications)
* Après l’enregistrement, les objets dépendants/enfants (publie) sont détachés, car ils ont été supprimées à partir de la base de données

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec relation requise

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null
  * Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null
* SaveChanges tente de définir la publication FK null, mais cette opération échoue car la clé étrangère ne peut pas être null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull avec une relation facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null
  * Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null
* SaveChanges définit la clé étrangère à la fois (publie) / enfants dépendants null
* Une fois que l’enregistrement, les objets dépendants/enfants (publie) ont maintenant des valeurs FK null et leur référence vers le principal/parents (blog) a été supprimé.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict avec une relation obligatoire ou facultative

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Publications sont marquées comme modifiés, car l’interruption de la relation a provoqué la clé étrangère à marquer comme null
  * Si la clé étrangère ne peut pas être null, puis la valeur réelle changera pas même si elle est marquée en tant que valeur null
* Étant donné que *Restrict* indique EF pas automatiquement de définir la clé étrangère sur null, elle reste non null et SaveChanges lève sans enregistrer

## <a name="cascading-to-untracked-entities"></a>En cascade pour les entités non chaînées

Lorsque vous appelez *SaveChanges*, les règles de suppression en cascade s’appliquera à toutes les entités qui sont suivies par le contexte. C’est le cas dans tous les exemples ci-dessus, c’est pourquoi le SQL généré pour supprimer le principal/parent (blog) et tous les objets dépendants/enfants (publie) :

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Si seul le principal est chargé--par exemple, lorsqu’une requête est faite pour un blog sans un `Include(b => b.Posts)` d’inclure également les publications--puis SaveChanges génère uniquement SQL pour supprimer le principal/parent :

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Les objets dépendants/enfants (publie) être supprimés uniquement si la base de données a un comportement de cascade correspondant configuré. Si vous utilisez EF pour créer la base de données, ce comportement en cascade sera installé pour vous.
