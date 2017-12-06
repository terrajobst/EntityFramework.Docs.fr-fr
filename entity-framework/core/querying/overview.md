---
title: "Fonctionnement des requêtes - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Fonctionnement des requêtes

Entity Framework Core utilise intégrer requête LINQ (Language) pour interroger des données à partir de la base de données. LINQ vous permet d’utiliser c# (ou .NET langue de votre choix) pour écrire des requêtes fortement typées en fonction de vos classes dérivées de contexte et d’entité.

## <a name="the-life-of-a-query"></a>La durée de vie d’une requête

Voici une vue d’ensemble de haut niveau du processus de que chaque requête parcourt.

1. La requête LINQ est traitée par Entity Framework Core pour générer une représentation est prête à être traité par le fournisseur de base de données
   1. Le résultat est mis en cache afin que ce traitement n’a pas besoin d’être effectuée chaque fois que la requête est exécutée.
2. Le résultat est transmis au fournisseur de base de données
   1. Le fournisseur de base de données identifie les parties de la requête peuvent être évaluées dans la base de données
   2. Ces parties de la requête sont traduites en langage de requête spécifique de base de données (par exemple, SQL pour une base de données relationnelle)
   3. Une ou plusieurs requêtes sont envoyées à la base de données et le jeu de résultats retourné (résultats sont des valeurs de la base de données, et non des instances d’entité)
3. Pour chaque élément du jeu de résultats
   1. S’il s’agit d’une requête de suivi, EF vérifie si les données représentent une entité déjà dans le traceur de modifications pour l’instance de contexte
      * Dans ce cas, l’entité existante est retournée.
      * Si ce n’est pas le cas, une nouvelle entité est créée, le suivi des modifications est le programme d’installation, et la nouvelle entité est retournée.
   2. S’il s’agit d’une requête de suivi de non, EF vérifie si les données représentent une entité déjà dans le jeu de résultats pour cette requête
      * Si, par conséquent, l’entité existante est retournée <sup>(1)</sup>
      * Dans le cas contraire, une nouvelle entité est créée et retournée

<sup>(1) </sup> Des requêtes de suivi sans utilisent des références faibles pour effectuer le suivi des entités qui ont déjà été retournées. Si un résultat antérieur avec la même identité est hors de portée, et le garbage collection s’exécute, vous risquez d’obtenir une nouvelle instance de l’entité.

## <a name="when-queries-are-executed"></a>Lorsque des requêtes sont exécutées.

Lorsque vous appelez des opérateurs LINQ, vous créez simplement une représentation en mémoire de la requête. La requête est envoyée uniquement à la base de données lorsque les résultats sont consommés.

Opérations qui génèrent les requêtes envoyées à la base de données les plus courantes sont :
* Itérer les résultats dans un `for` boucle
* À l’aide d’un opérateur tel que `ToList`, `ToArray`, `Single`,`Count`
* Liaison de données les résultats d’une requête à une interface utilisateur

> [!WARNING]  
> **Validez toujours l’entrée d’utilisateur :** tandis que EF assure une protection contre les attaques par injection SQL, il n’effectue aucune validation générale d’entrée. Par conséquent, si les valeurs transmises aux API, utilisé dans les requêtes LINQ, affectées aux propriétés de l’entité, etc., proviennent d’une source non fiable, puis validation appropriées, selon vos besoins de l’application, doit être effectuée. Cela inclut toute entrée d’utilisateur utilisée pour construire des requêtes de façon dynamique. Même si vous utilisez LINQ, si vous acceptez d’entrée d’utilisateur pour générer des expressions, que vous devez vous assurer que seules les expressions prévues peut être construit.
