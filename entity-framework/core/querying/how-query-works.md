---
title: 'Fonctionnement des requêtes : EF Core'
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136242"
---
# <a name="how-queries-work"></a>Fonctionnement des requêtes

Entity Framework Core utilise LINQ (Language Integrated Query) pour interroger les données de la base de données. LINQ vous permet d’utiliser C# (ou le langage .NET de votre choix) pour écrire des requêtes fortement typées en fonction de votre contexte dérivé et de vos classes d’entité.

## <a name="the-life-of-a-query"></a>La durée de vie d’une requête

Voici une vue d’ensemble de haut niveau du processus que chaque requête traverse.

1. La requête LINQ est traitée par Entity Framework Core pour générer une représentation qui est prête à être traitée par le fournisseur de base de données
   1. Le résultat est mis en cache afin que ce traitement n’ait pas besoin d’être effectué chaque fois que la requête est exécutée
2. Le résultat est transmis au fournisseur de base de données
   1. Le fournisseur de base de données identifie les parties de la requête qui peuvent être évaluées dans la base de données
   2. Ces parties de la requête sont traduites en langage de requête spécifique de base de données (par exemple, SQL pour une base de données relationnelle)
   3. Une requête est envoyée à la base de données et l’ensemble de résultats retourné (les résultats sont des valeurs de la base de données, et non des instances de l’entité)
3. Pour chaque élément du jeu de résultats
   1. S’il s’agit d’une requête de suivi, EF vérifie si les données représentent une entité déjà présente dans le traceur de modifications pour l’instance de contexte
      * Dans ce cas, l’entité existante est retournée
      * Si ce n’est pas le cas, une nouvelle entité est créée, le suivi des modifications est configuré, et la nouvelle entité est retournée
   2. S’il s’agit d’une requête sans suivi, alors une nouvelle entité est toujours créée et retournée

## <a name="when-queries-are-executed"></a>Lorsque des requêtes sont exécutées

Lorsque vous appelez des opérateurs LINQ, vous créez simplement une représentation en mémoire de la requête. La requête est envoyée à la base de données uniquement lorsque les résultats sont consommés.

Les opérations qui génèrent les requêtes envoyées à la base de données les plus courantes sont :

* Itération des résultats dans une boucle `for`
* Utilisation d’un `ToList` `ToArray`opérateur `Single` `Count` tel que , , ou l’équivalent des surcharges async

> [!WARNING]  
> **Toujours valider l’entrée d’utilisateur :** bien qu’EF Core protège contre les attaques par injection SQL au moyen de paramètres et de l’échappement des littéraux dans les requêtes, il ne valide pas les entrées. La validation appropriée, selon les exigences de l’application, doit être effectuée avant que les valeurs provenant de sources non fiables ne soient utilisées dans les requêtes LINQ, attribuées aux propriétés de l’entité ou transmises à d’autres API de base de l’EF. Cela inclut toute entrée utilisateur utilisée pour construire des requêtes de façon dynamique. Même si vous utilisez LINQ, si vous acceptez les entrées d’utilisateur pour générer des expressions, vous devez vous assurer que seules les expressions prévues peuvent être construites.
