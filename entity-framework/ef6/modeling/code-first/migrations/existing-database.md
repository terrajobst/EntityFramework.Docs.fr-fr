---
title: Migrations Code First avec une base de données existante - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
caps.latest.revision: 3
ms.openlocfilehash: 77e139a29bb4708b00fc6198a57780ce75197252
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121475"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrations Code First avec une base de données existante
> [!NOTE]
> **EF4.3 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 4.1. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Cet article aborde l’utilisation de Code First Migrations dans une base de données existante, qui n’a pas été créé par Entity Framework.

> [!NOTE]
> Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base. Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.

## <a name="screencasts"></a>Captures d’écran

Si vous surveille plutôt une capture vidéo à lire cet article, les deux vidéos suivantes couvrent le même contenu que cet article.

### <a name="video-one-migrations---under-the-hood"></a>Une vidéo : « Migrations - sous le capot »

[Cette capture vidéo](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) couvre le mode de suivi des migrations et utilise les informations sur le modèle pour détecter les modifications apportées au modèle.

### <a name="video-two-migrations---existing-databases"></a>Deux vidéo : « Migrations - bases de données »

Création sur les concepts de la vidéo précédente, [cet enregistrement](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) explique comment activer et utiliser des migrations avec une base de données existante.

## <a name="step-1-create-a-model"></a>Étape 1 : Créer un modèle

La première étape sera pour créer un modèle Code First ciblant votre base de données existante. Le [Code First pour une base de données existante](~/ef6/modeling/code-first/workflows/existing-database.md) rubrique fournit des instructions détaillées sur la procédure à suivre.

>[!NOTE]
> Il est important de suivre le reste des étapes de cette rubrique avant d’apporter des modifications à votre modèle qui nécessiterait des modifications du schéma de base de données. Les étapes suivantes requièrent le modèle pour être synchronisé avec le schéma de base de données.

## <a name="step-2-enable-migrations"></a>Étape 2 : Activer les Migrations

L’étape suivante consiste à permettre des migrations. Vous pouvez le faire en exécutant la **Enable-Migrations** commande dans la Console du Gestionnaire de Package.

Cette commande Créer un dossier dans votre solution appelée Migrations et placer une classe unique à l’intérieur de celui-ci appelée Configuration. La classe de Configuration est où vous configurez migrations pour votre application, vous pouvez trouver plus d’informations dans le [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) rubrique.

## <a name="step-3-add-an-initial-migration"></a>Étape 3 : Ajouter une migration initiale

Une fois que les migrations qui ont été créées et appliquées à la base de données locale, que vous pouvez également appliquer ces devient autres bases de données. Par exemple, votre base de données locale peut être une base de données de test et finalement voulez-vous également appliquer les modifications à une base de données de production et/ou bases de données de test des autres développeurs. Il existe deux options pour cette étape et dépend de celui que vous devez sélectionner le schéma d’autres bases de données est vide ou actuellement correspond au schéma de la base de données locale.

-   **Option 1 : Utiliser schéma existant comme point de départ.** Vous devez utiliser cette approche lorsque d’autres bases de données migrations seront appliquées à l’avenir à aura le même schéma que votre base de données locale a actuellement. Par exemple, vous pouvez utiliser cela si votre base de données de test local correspond actuellement à v1 de votre base de données de production et vous devez ensuite appliquer ces migrations pour mettre à jour votre base de données de production vers la version 2.
-   **Option deux : Utiliser la base de données vide en tant que point de départ.** Vous devez utiliser cette approche lorsque les autres bases de données migrations seront appliquées à l’avenir à sont vides (ou n’existent pas encore). Par exemple, vous pouvez utiliser cela si vous en main le développement de votre application à l’aide d’une base de données de test mais sans utiliser de migrations et vous serez ultérieurement créer une base de données de production à partir de zéro.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Option 1 : Utiliser le schéma existant comme point de départ

Migrations Code First utilise un instantané du modèle stocké dans la migration la plus récente pour détecter les modifications apportées au modèle (vous trouverez des informations détaillées à ce sujet dans [Migrations Code First dans les environnements d’équipe](~/ef6/modeling/code-first/migrations/teams.md)). Étant donné que nous allons supposer que les bases de données ont déjà le schéma du modèle actuel, nous allons générer une migration (nulle) vide qui contient le modèle actuel en tant qu’instantané.

1.  Exécutez le **Add-Migration InitialCreate – IgnoreChanges** commande dans la Console du Gestionnaire de Package. Cela crée une migration vide avec le modèle actuel en tant qu’instantané.
2.  Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package. Cela s’applique la migration InitialCreate à la base de données. Étant donné que la migration réelle ne contient pas toutes les modifications, elle sera simplement ajouter une ligne à la \_ \_MigrationsHistory table qui indique que cette migration a déjà été appliquée.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Option deux : Utiliser la base de données vide comme point de départ

Dans ce scénario, nous devons Migrations pour être en mesure de créer la base de données à partir de zéro – y compris les tables qui sont déjà présents dans notre base de données locale. Nous allons générer une migration InitialCreate qui inclut la logique pour créer le schéma existant. Nous allons ensuite rendre notre base de données existante ressemble pas à cette migration a déjà été appliquée.

1.  Exécutez le **Add-Migration InitialCreate** commande dans la Console du Gestionnaire de Package. Cette opération crée une migration pour créer le schéma existant.
2.  Commentez tout le code dans la méthode du haut de la migration nouvellement créée. Cela nous permettra à « appliquer » la migration vers la base de données local sans essayer de recréer toutes les tables etc. qui existent déjà.
3.  Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package. Cela s’applique la migration InitialCreate à la base de données. Étant donné que la migration réelle ne contient pas des modifications (étant donné que nous commenté temporairement les out), il sera simplement ajouter une ligne à la \_ \_MigrationsHistory table qui indique que cette migration a déjà été appliquée.
4.  Supprimer le commentaire le code dans la méthode à distance. Cela signifie que lorsque cette migration est appliquée pour les futures bases de données, le schéma qui existaient déjà dans la base de données locale est créé par des migrations.

## <a name="things-to-be-aware-of"></a>Choses à connaître

Il existe plusieurs choses à connaître lors de l’utilisation de Migrations par rapport à une base de données existante.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Noms par défaut/calculé peut ne pas correspondent schéma existant

Migrations spécifie explicitement les noms de colonnes et les tables quand il structure un migrations. Toutefois, il existe d’autres objets de base de données qui Migrations calcule un nom par défaut lors de l’application les migrations. Cela inclut les index et contraintes de clé étrangère. Lorsque vous ciblez un schéma existant, ces noms calculés peut ne pas correspondent ce qui existe réellement dans votre base de données.

Voici quelques exemples de lorsque vous devez être conscient de cela :

**Si vous avez utilisé « une Option : utiliser un schéma existant comme point de départ ' de l’étape 3 :**

-   Si les futures modifications dans votre modèle requièrent la modification ou suppression d’un des objets de base de données qui porte un nom différent, vous devez modifier la migration généré automatiquement pour spécifier le nom correct. Les API de Migrations ont un paramètre de nom facultatif qui vous permet de procéder.
    Par exemple, votre schéma existant peut avoir une table Post avec une colonne de clé étrangère BlogId qui a un index nommé IndexFk\_BlogId. Toutefois, par défaut Migrations attendez cet index pour être nommé IX\_BlogId. Si vous apportez une modification à votre modèle qui provoque la suppression de cet index, vous devez modifier l’appel de DropIndex généré automatiquement pour spécifier le IndexFk\_BlogId nom.

**Si vous avez utilisé « Option deux : base de données vide à utiliser comme point de départ ' de l’étape 3 :**

-   Tentative d’exécution de la méthode vers le bas de la migration initiale (qui est, retour à la base de données vide) par rapport à votre base de données locale peut échouer, car les Migrations tente de supprimer les index et contraintes de clé étrangère en utilisant les noms incorrects. Cela n’affecte que votre base de données locale dans la mesure où les autres bases de données seront créées à partir de zéro à l’aide de la méthode d’accès de la migration initiale.
    Si vous souhaitez rétrograder votre base de données local existant à un état vide, il est plus facile d’effectuer cette opération manuellement, soit en supprimant la base de données ou en supprimant toutes les tables. Après cette opération initiale tous les objets de base de données seront recréées avec les noms par défaut, par conséquent, ce problème ne se pas présentera à nouveau.
-   Si les futures modifications dans votre modèle requièrent la modification ou suppression d’un des objets de base de données qui porte un nom différent, cela ne fonctionne pas sur votre base de données local existant – étant donné que les noms ne correspondent pas les valeurs par défaut. Toutefois, il fonctionnera sur les bases de données qui ont été créés « à partir de zéro » dans la mesure où ils ont utilisera les noms par défaut choisis par des Migrations.
    Vous pourrez apporter ces modifications manuellement sur votre base de données local existant, ou faites en sorte que les Migrations recréer votre base de données à partir de zéro – comme il le sera sur d’autres ordinateurs.
-   Bases de données créées à l’aide de la méthode du haut de la migration initiale peuvent différer légèrement de la base de données locale depuis les noms par défaut calculés pour les index et contraintes de clé étrangère seront utilisés. Vous pourrez également vous retrouver avec des index supplémentaires comme des Migrations créera des index sur les colonnes de clé étrangère par défaut : il a peut-être pas le cas dans votre base de données locale d’origine.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Pas tous les objets de base de données sont représentées dans le modèle

Les objets de base de données qui ne font pas partie de votre modèle ne seront pas gérées par des Migrations. Cela peut inclure des vues, les procédures stockées, les autorisations, les tables qui ne font pas partie de votre modèle, des index supplémentaires, etc.

Voici quelques exemples de lorsque vous devez être conscient de cela :

-   Quelle que soit l’option choisie dans « Étape 3 », si les futures modifications dans votre modèle requièrent la modification ou la suppression de ces objets supplémentaires que migrations ne saura pas pour effectuer ces modifications. Par exemple, si vous supprimez une colonne qui a un index supplémentaire sur ce dernier, les Migrations ne saura pas à supprimer l’index. Vous devrez l’ajouter manuellement à la Migration du modèle généré automatiquement.
-   Si vous avez utilisé « Option deux : base de données vide à utiliser comme point de départ », ces objets supplémentaires ne seront pas créés par la méthode du haut de la migration initiale.
    Vous pouvez modifier le haut et vers le bas des méthodes pour prendre en charge de ces objets supplémentaires si vous le souhaitez. Pour les objets qui ne sont pas en mode natif pris en charge dans l’API de Migrations – tels que les vues – vous pouvez utiliser la [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) méthode à exécuter des requêtes SQL brutes pour créer/supprimer les.
