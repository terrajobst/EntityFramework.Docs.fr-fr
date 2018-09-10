---
title: Code First Migrations dans les environnements d’équipe - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 31f8476c64d36d4d1cf3d18deb59ebc482dcc975
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251230"
---
# <a name="code-first-migrations-in-team-environments"></a>Code First Migrations dans les environnements d’équipe
> [!NOTE]
> Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base. Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Prenez un café, vous devez lire cet article entier

Les problèmes dans les environnements d’équipe sont principalement autour de la fusion des migrations lorsque deux développeurs ont généré des migrations dans leur base de code local. Alors que les étapes pour résoudre ces problèmes sont relativement simples, ils nécessitent que vous disposiez d’une connaissance approfondie du fonctionnement des migrations. Ne pas simplement passer directement à la fin : prenez le temps de lire l’article complet pour garantir la que réussite de votre.

## <a name="some-general-guidelines"></a>Voici quelques directives générales

Avant d’aller dans la gestion des migrations fusionnées générées par plusieurs développeurs, voici quelques indications générales pour garantir le succès.

### <a name="each-team-member-should-have-a-local-development-database"></a>Chaque membre de l’équipe doit avoir une base de données de développement local

Migrations utilise le  **\_ \_MigrationsHistory** table pour stocker les migrations qui ont été appliquées à la base de données. Si vous avez plusieurs développeurs générant des migrations différentes lors de la tentative de cibler la même base de données (et par conséquent partager une  **\_ \_MigrationsHistory** table) migrations va peu trouble.

Bien sûr, si vous avez des membres de l’équipe qui ne sont pas générer des migrations, il n’est pas un problème que de les partager une base de données centrale de développement.

### <a name="avoid-automatic-migrations"></a>Évitez les migrations automatiques

Le point important est que les migrations automatiques initialement aspect dans les environnements d’équipe, mais en réalité qu’ils ne fonctionnent pas. Si vous souhaitez savoir pourquoi, poursuivez votre lecture – si ce n’est pas, puis vous pouvez passer à la section suivante.

Les migrations automatiques vous permet d’avoir votre schéma de base de données mis à jour pour faire correspondre le modèle actuel sans avoir besoin de générer des fichiers de code (basée sur le code de migrations). Les migrations automatiques fonctionnerait très bien dans un environnement d’équipe si vous uniquement ne les a utilisés et jamais généré les migrations en code. Le problème est que les migrations automatiques sont limitées et ne gèrent pas un nombre d’opérations de propriété/colonne renomme, le déplacement des données vers une autre table, etc. Pour gérer ces scénarios, vous vous retrouvez migrations basées sur le code de génération (et la modification du code généré automatiquement) qui sont mélangées entre les modifications qui sont gérés par les migrations automatiques. Cela rend près sur impossibles à fusionner les modifications lorsque deux développeurs archivent des migrations.

## <a name="screencasts"></a>Captures d’écran

Si vous surveille plutôt une capture vidéo à lire cet article, les deux vidéos suivantes couvrent le même contenu que cet article.

### <a name="video-one-migrations---under-the-hood"></a>Une vidéo : « Migrations - sous le capot »

[Cette capture vidéo](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) couvre le mode de suivi des migrations et utilise les informations sur le modèle pour détecter les modifications apportées au modèle.

### <a name="video-two-migrations---team-environments"></a>Deux vidéo : « Migrations - environnements d’équipe »

Création sur les concepts de la vidéo précédente, [cet enregistrement](http://channel9.msdn.com/blogs/ef/migrations-team-environments) couvre les problèmes qui surviennent dans un environnement d’équipe et comment les résoudre.

## <a name="understanding-how-migrations-works"></a>Comprendre le fonctionnement des migrations

La clé pour utiliser correctement les migrations dans un environnement d’équipe est un présentation Comment suit les migrations de base et utilise les informations sur le modèle pour détecter les modifications apportées au modèle.

### <a name="the-first-migration"></a>La première migration

Lorsque vous ajoutez la première migration à votre projet, vous exécutez quelque chose comme **Add-Migration première** dans la Console du Gestionnaire de Package. Les étapes de haut niveau qui exécute cette commande sont décrits ci-dessous.

![Première Migration](~/ef6/media/firstmigration.png)

Le modèle actuel est calculé à partir de votre code (1). Les objets de base de données requis sont alors calculées par la différence de modèle (2) : dans la mesure où cela est la première migration du modèle diffèrent utilise simplement un modèle vide pour la comparaison. Les modifications requises sont passées au Générateur de code pour générer le code de migration requises (3) qui est ensuite ajouté à votre solution Visual Studio (4).

Outre le code de migration réelle qui est stocké dans le fichier de code principal, les migrations génère également certains fichiers code-behind supplémentaires. Ces fichiers sont des métadonnées qui sont utilisée par des migrations et ne sont pas quelque chose que vous devez modifier. Un de ces fichiers est un fichier de ressources (.resx) qui contient un instantané du modèle au moment de que la génération de la migration. Vous verrez comment il est utilisé dans l’étape suivante.

À ce stade, vous exécutez probablement **Update-Database** pour appliquer vos modifications à la base de données, puis passez sur l’implémentation des autres zones de votre application.

### <a name="subsequent-migrations"></a>Des migrations

Plus tard, vous revenez et apportez des modifications à votre modèle : dans notre exemple, nous allons ajouter un **Url** propriété **Blog**. Puis émettez une commande comme **Add-Migration AddUrl** pour générer automatiquement une migration pour appliquer la base de données change. Les étapes de haut niveau qui exécute cette commande sont décrits ci-dessous.

![Nouvelle Migration](~/ef6/media/secondmigration.png)

Tout comme la dernière fois, le modèle actuel est calculé à partir du code (1). Toutefois, cette fois, il existe des migrations existantes pour le modèle précédent est récupéré à partir de la dernière migration (2). Ces deux modèles sont comparés pour rechercher les modifications de base de données requis (3) et le processus effectue ensuite comme avant.

Ce même processus est utilisé pour les migrations supplémentaires que vous ajoutez au projet.

### <a name="why-bother-with-the-model-snapshot"></a>Pourquoi s’embêter avec la capture instantanée du modèle ?

Vous vous demandez peut-être pourquoi EF dérange avec la capture instantanée du modèle – pourquoi pas simplement examiner la base de données. Dans ce cas, lisez la suite. Si vous ne souhaitez pas vous pouvez ignorer cette section.

Il existe de nombreuses raisons QU'EF conserve la capture instantanée autour du modèle :

-   Il permet à votre base de données à passer à partir du modèle Entity Framework. Ces modifications peuvent être apportées directement dans la base de données, ou vous pouvez modifier le code généré automatiquement dans vos migrations pour apporter les modifications. Voici quelques exemples de cela en pratique :
    -   Vous souhaitez ajouter une Inserted et la mise à jour pour la colonne à un ou plusieurs de vos tables, mais vous ne souhaitez pas inclure ces colonnes dans le modèle EF. Si les migrations examiné la base de données que s’il s’agissait en permanence essayez de supprimer ces colonnes chaque fois que vous généré automatiquement une migration. À l’aide de la capture instantanée du modèle, EF uniquement jamais détecte des modifications légitimes portées au modèle.
    -   Vous souhaitez modifier le corps d’une procédure stockée utilisée pour les mises à jour pour inclure une journalisation. Si les migrations étudié cette procédure stockée à partir de la base de données en permanence tentait de rétablir la définition qui attend d’EF. À l’aide de la capture instantanée du modèle, EF sera structurer uniquement jamais de code pour modifier la procédure stockée lorsque vous modifiez la forme de la procédure décrite dans le modèle EF.
    -   Ces mêmes principes s’appliquent à l’ajout d’index supplémentaires, y compris les tables supplémentaires dans votre base de données, mappage EF à une vue de base de données qui se trouve sur une table, etc.
-   Le modèle EF contient plus que la forme de la base de données. L’ensemble du modèle permet de migrations d’examiner des informations sur les propriétés et les classes dans votre modèle et comment elles correspondent aux colonnes et des tables. Ces informations permettent à être plus intelligentes dans le code il structure les migrations. Par exemple, si vous modifiez le nom de la colonne qui a une propriété est mappée aux migrations peut détecter le changement de nom en fait de constater qu’il est la même propriété – quelque chose qui ne peut pas être effectuée si vous avez uniquement le schéma de base de données. 

## <a name="what-causes-issues-in-team-environments"></a>Ce qui provoque des problèmes dans les environnements d’équipe

Le flux de travail traités dans la précédente fonctionne de section parfaitement lorsque vous êtes un développeur unique qui travaille sur une application. Elle fonctionne également bien dans un environnement d’équipe si vous êtes la seule personne à apporter des modifications au modèle. Dans ce scénario, vous pouvez apporter des modifications de modèle, générer des migrations et les envoyer à votre contrôle de code source. D’autres développeurs peuvent synchroniser vos modifications et exécutez **Update-Database** pour que les modifications de schéma appliquées.

Problèmes commencent à se produire lorsque vous avez plusieurs développeurs apporter des modifications au modèle EF et envoi au contrôle de code source en même temps. Qu’EF ne dispose pas d’est un moyen de première classe pour fusionner vos migrations locales avec les migrations un autre développeur a soumis au contrôle de code source depuis la dernière synchronisation.

## <a name="an-example-of-a-merge-conflict"></a>Un exemple d’un conflit de fusion

Première Examinons un exemple concret de conflit de fusion. Nous allons continuer sur avec l’exemple que nous avons vu précédemment. Nous allons commencer à point supposent les modifications à partir de la section précédente ont été archivées par le développeur d’origine. Nous allons suivre deux développeurs quand ils apportent des modifications au code base.

Nous allons suivre le modèle EF et les migrations à un nombre de modifications. Pour un point de départ, les développeurs ont synchronisé dans le référentiel de contrôle source, comme illustré dans le graphique suivant.

![Point de départ](~/ef6/media/startingpoint.png)

Développeur \#1 et développeur \#2 désormais apporte des modifications au modèle EF dans leur code local base. Développeur \#1 ajoute un **évaluation** propriété **Blog** – et génère une **AddRating** migration pour appliquer les modifications à la base de données. Développeur \#2 ajoute un **lecteurs** propriété **Blog** – et génère le correspondantes **AddReaders** migration. Les développeurs exécutaient **Update-Database**, pour appliquer les modifications à leurs bases de données locales, puis continuer le développement de l’application.

> [!NOTE]
> Migrations sont précédées avec un horodatage, de sorte que notre graphique représente que la migration AddReaders développeur \#2 vient après la migration AddRating développeur \#1. Si développeur \#1 ou \#2 ne généré aucune différence pour les problèmes de l’utilisation dans une équipe, ou le processus de fusion que nous allons examiner dans la section suivante d’effectuer de la première migration.

![Modifications locales](~/ef6/media/localchanges.png)

C’est un jour chance pour développeur \#1 qu’elles se produisent à soumettre leurs modifications tout d’abord. Car aucune autre personne a archivé dans la mesure où ils synchronisés son référentiel, ils peuvent simplement les soumettre leurs modifications sans effectuer toute fusion.

![Envoyer](~/ef6/media/submit.png)

Maintenant il est temps pour développeur \#2 à envoyer. Ils ne sont pas donc chance. Étant donné que quelqu'un d’autre a soumis des modifications dans la mesure où ils synchronisés, ils doivent extraire les modifications et la fusion. Le système de contrôle de code source sera probablement être en mesure de fusionner automatiquement les modifications au niveau du code dans la mesure où ils sont très simples. L’état de développeur \#local de 2 référentiel après la synchronisation est représentée dans le graphique suivant. 

![Extraction](~/ef6/media/pull.png)

À ce stade développeur \#2 peuvent exécuter **Update-Database** qui détectera la nouvelle **AddRating** migration (qui n’a pas été appliqué au développeur \#2 base de données) et l’appliquer. Maintenant le **évaluation** colonne est ajoutée à la **Blogs** table et la base de données est synchronisée avec le modèle.

Il existe cependant deux problèmes :

1.  Bien que **Update-Database** appliquera la **AddRating** migration elle déclenche également un avertissement : *Impossible de mettre à jour de la base de données pour faire correspondre le modèle actuel, car les modifications en attente et migration automatique est désactivée...*
    Le problème est que la capture instantanée du modèle est stocké dans la dernière migration (**AddReader**) n’a pas la **évaluation** propriété sur **Blog** (dans la mesure où il ne fasse pas partie du modèle lors de la migration a été générée). Tout d’abord, le code détecte que le modèle dans la dernière migration ne correspond pas au modèle actuel et déclenche l’avertissement.
2.  Exécution de l’application entraînera une exception InvalidOperationException indiquant que «*le modèle soutient le contexte 'BloggingContext' a changé depuis la création de la base de données. Envisagez d’utiliser des Migrations Code First pour mettre à jour de la base de données... »*
    Là encore, le problème est la capture instantanée du modèle stockée dans la dernière migration ne correspond pas au modèle actuel.
3.  Enfin, nous pouvons prévoir en cours d’exécution **Add-Migration** maintenant générerait une migration vide (dans la mesure où il n’y a aucune modification à appliquer à la base de données). Mais étant donné que les migrations compare le modèle actuel à celui de la dernière migration (qui manque le **évaluation** propriété) il sera réellement structurer un autre **AddColumn** appel visant à ajouter dans le **Évaluation** colonne. Bien sûr, cette migration échouerait pendant **Update-Database** , car le **évaluation** colonne existe déjà.

## <a name="resolving-the-merge-conflict"></a>Résolution du conflit de fusion

La bonne nouvelle est qu’il n’est pas trop difficile à gérer avec la fusion manuelle : fournies vous avez une compréhension du fonctionnement des migrations. Par conséquent, si vous avez ignoré à l’avance à cette section... Nous sommes désolés, vous devez revenir en arrière et lire le reste de l’article tout d’abord !

Il existe deux options, la plus simple consiste à générer une migration vide qui contient le modèle correct actuel en tant qu’instantané. La deuxième option consiste à mettre à jour de la capture instantanée dans la dernière migration pour avoir le bon capture instantanée du modèle. La deuxième option est un peu plus difficile et ne peut pas être utilisée dans tous les scénarios, mais il est également plus propre, car il n’implique l’ajout d’une migration supplémentaire.

### <a name="option-1-add-a-blank-merge-migration"></a>Option 1 : Ajouter une migration vide « fusionner »

Dans cette option, nous générons une migration vide uniquement afin de s’assurer de la dernière migration a l’instantané de modèle approprié stockées qu’il contient.

Cette option peut être utilisée indépendamment de ce qui a généré la dernière migration. Dans l’exemple que nous avons effectué les développeurs \#2 est prise en charge de la fusion et ils se produisent pour générer la dernière migration. Mais ces mêmes étapes peuvent être utilisées si développeur \#1 généré la dernière migration. Les étapes s’appliquent également si plusieurs migrations sont impliqués : nous avons été en train d’utiliser deux pour faire simple.

Le processus suivant peut être utilisé pour cette approche, à partir du moment que vous réalisez que vous avez des modifications qui doivent être synchronisés à partir du contrôle de code source.

1.  Vérifiez que toutes les modifications en attente de modèle dans votre base de code local ont été écrits dans une migration. Cette étape garantit que vous ne manquez pas les modifications légitimes lorsqu’il s’agit de la génération de la migration vide.
2.  Synchroniser avec le contrôle de code source.
3.  Exécutez **Update-Database** pour appliquer les migrations de nouveau les autres développeurs ont archivé.
    **
    *Remarque : *** Si vous n’obtenez pas les avertissements à partir de la commande Update-Database ne comportait aucune nouvelle migration à partir d’autres développeurs et il est inutile d’effectuer toute fusion.*
4.  Exécutez **Add-Migration &lt;choisir\_un\_nom&gt; – IgnoreChanges** (par exemple, **Add-Migration de fusion – IgnoreChanges**). Cela génère une migration avec toutes les métadonnées (y compris un instantané du modèle actif), mais ignore les modifications qu’il détecte lors de la comparaison du modèle actif pour la capture instantanée dans les migrations dernière (ce qui signifie que vous obtenez une valeur vide **des** et **Vers le bas** méthode).
5.  Continuer à développer ou les envoyer au contrôle de code source (après que exécution votre de tests unitaires bien entendu).

Voici l’état de développeur \#local de 2 code base après l’utilisation de cette approche.

![Migration de fusion](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Option 2 : Mettre à jour la capture instantanée du modèle dans la dernière migration

Cette option est très similaire à l’option 1, mais supprime la migration vierge supplémentaires – car Admettons-le, qui souhaite les fichiers de code supplémentaire dans leur solution.

**Cette approche est uniquement possible si la migration la plus récente existe uniquement dans votre base de code local et le n'a pas encore été soumise au contrôle de code source (par exemple, si la dernière migration a été générée par l’utilisateur qui effectue la fusion)**. Modifier les métadonnées des migrations autres développeurs déjà appliqués à leur base de données de développement – ou encore pire appliqué à une base de données de production – peut entraîner des effets secondaires inattendus. Au cours du processus, nous allons restaurer la dernière migration dans notre base de données locale et réappliquez avec les métadonnées mises à jour.

Bien que la dernière migration doit juste être dans le code local base, il n’existe aucune restriction pour le nombre ou l’ordre des migrations qui il continuer. Il peut y avoir plusieurs migrations à partir de plusieurs développeurs différents et les mêmes étapes s’appliquent : nous avons été en train d’utiliser deux pour faire simple.

Le processus suivant peut être utilisé pour cette approche, à partir du moment que vous réalisez que vous avez des modifications qui doivent être synchronisés à partir du contrôle de code source.

1.  Vérifiez que toutes les modifications en attente de modèle dans votre base de code local ont été écrits dans une migration. Cette étape garantit que vous ne manquez pas les modifications légitimes lorsqu’il s’agit de la génération de la migration vide.
2.  Synchroniser avec le contrôle de code source.
3.  Exécutez **Update-Database** pour appliquer les migrations de nouveau les autres développeurs ont archivé.
    **
    *Remarque : *** Si vous n’obtenez pas les avertissements à partir de la commande Update-Database ne comportait aucune nouvelle migration à partir d’autres développeurs et il est inutile d’effectuer toute fusion.*
4.  Exécutez **mise à jour la base de données – TargetMigration &lt;deuxième\_dernière\_migration&gt;**  (dans l’exemple que nous avons été suivant ce serait **Update-Database : TargetMigration AddRating**). Cette sauvegarde de la base de données à l’état de la deuxième des rôles dernière migration – efficacement « non application » la dernière migration à partir de la base de données.
    **
    *Remarque : *** cette étape est nécessaire pour sécuriser de modifier les métadonnées de la migration dans la mesure où les métadonnées sont également stockées dans le \_ \_MigrationsHistoryTable de la base de données. C’est pourquoi vous devez uniquement utiliser cette option si la dernière migration est uniquement dans votre base de code local. Si d’autres bases de données était la dernière migration appliquée vous devrez également les annuler et ré-appliquer la dernière migration pour mettre à jour les métadonnées.* 
5.  Exécutez **Add-Migration &lt;complète\_nom\_notamment\_timestamp\_de\_dernière\_migration** &gt; (dans l’exemple Nous avons été suivant ce serait quelque chose comme **Add-Migration 201311062215252\_AddReaders**).
    **
    *Remarque : *** vous devez inclure l’horodatage afin que les migrations sait que vous souhaitez modifier la migration existante au lieu d’une génération de modèles automatique.*
    Cela met à jour les métadonnées pour la dernière migration correspondre au modèle actuel. Vous obtiendrez l’avertissement suivant lors de la commande terminée, mais c’est exactement ce que vous voulez. «*Uniquement le Code de concepteur pour la migration « 201311062215252\_AddReaders' a été ré-généré automatiquement. Pour ré-structurer la migration entière, utilisez le paramètre - Force. »*
6.  Exécutez **Update-Database** demander à nouveau la dernière migration avec les métadonnées de mises à jour.
7.  Continuer à développer ou les envoyer au contrôle de code source (après que exécution votre de tests unitaires bien entendu).

Voici l’état de développeur \#local de 2 code base après l’utilisation de cette approche.

![Métadonnées mises à jour](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Résumé

Il existe quelques problèmes lors de l’utilisation de Code First Migrations dans un environnement d’équipe. Toutefois, une compréhension élémentaire du fonctionnement de la migration et de certaines approches simples pour la résolution des conflits de fusion facilitent surmonter ces défis.

Le problème fondamental est incorrecte métadonnées stockées dans la dernière migration. Par conséquent, Code First détecte de façon incorrecte que le modèle actuel et le schéma de base de données ne correspondent pas et structurer le code incorrect dans la prochaine migration. Cette situation peut être surmontée en générant une migration vide avec le modèle correct, ou la mise à jour les métadonnées de la dernière migration.
