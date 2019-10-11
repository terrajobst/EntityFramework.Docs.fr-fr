---
title: 'Fonctionnement des requêtes : EF Core'
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: bc085755f39b1288f092a8b2df892c1bf82a89f1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186266"
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
   3. Une ou plusieurs requêtes sont envoyées à la base de données et un jeu de résultats est retourné (les résultats sont des valeurs de la base de données, et non des instances d’entité)
3. Pour chaque élément du jeu de résultats
   1. S’il s’agit d’une requête de suivi, EF vérifie si les données représentent une entité déjà présente dans le traceur de modifications pour l’instance de contexte
      * Dans ce cas, l’entité existante est retournée
      * Si ce n’est pas le cas, une nouvelle entité est créée, le suivi des modifications est configuré, et la nouvelle entité est retournée
   2. S’il s’agit d’une requête sans suivi, EF vérifie si les données représentent une entité déjà présente dans le jeu de résultats pour cette requête
      * Dans ce cas, l’entité existante est retournée <sup>(1)</sup>
      * Dans le cas contraire, une nouvelle entité est créée et retournée

<sup>(1)</sup> Les requêtes sans suivi utilisent les références faibles pour effectuer le suivi des entités qui ont déjà été retournées. Si un résultat antérieur avec la même identité est hors de portée, et que le nettoyage de la mémoire s’exécute, vous risquez d’obtenir une nouvelle instance de l’entité.

## <a name="when-queries-are-executed"></a>Lorsque des requêtes sont exécutées

Lorsque vous appelez des opérateurs LINQ, vous créez simplement une représentation en mémoire de la requête. La requête est envoyée à la base de données uniquement lorsque les résultats sont consommés.

Les opérations qui génèrent les requêtes envoyées à la base de données les plus courantes sont :
* Itération des résultats dans une boucle `for`
* Utilisation d’un opérateur tel que `ToList`, `ToArray`, `Single`, `Count`
* Liaison des données des résultats d’une requête sur une interface utilisateur

> [!WARNING]  
> **Toujours valider l’entrée utilisateur :** Bien que EF Core protège contre les attaques par injection SQL en utilisant des paramètres et des littéraux d’échappement dans les requêtes, il ne valide pas les entrées. Vous devez effectuer une validation adéquate, conformément aux exigences de l’application, avant que les valeurs provenant d’une source non fiable soient utilisées dans des requêtes LINQ, affectées à des propriétés d’entité ou transmises à d’autres API EF Core. Cela inclut toute entrée utilisateur utilisée pour construire des requêtes de façon dynamique. Même si vous utilisez LINQ, si vous acceptez les entrées d’utilisateur pour générer des expressions, vous devez vous assurer que seules les expressions prévues peuvent être construites.
