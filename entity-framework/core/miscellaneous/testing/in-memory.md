---
title: Test avec InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a>Test avec InMemory

Le fournisseur en mémoire est utile lorsque vous souhaitez tester des composants à l’aide d’un élément qui est une approximation de la connexion à la base de données réel, sans la surcharge des opérations de base de données.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>En mémoire n’est pas une base de données relationnelle

Fournisseurs de base de données de base EF n’ont pas à être des bases de données relationnelles. En mémoire est conçu pour être une base de données usage général pour le test et n’est pas conçu pour simuler une base de données relationnelle.

Voici quelques exemples de ce sont :
* InMemory vous permettra d’enregistrer les données qui violent les contraintes d’intégrité référentielle dans une base de données relationnelle.

* Si vous utilisez DefaultValueSql(string) pour une propriété dans votre modèle, il est une API de base de données relationnelle et n’a aucun effet lors de l’exécution par rapport à en mémoire.

> [!TIP]  
> Ces différences sont peu pour autant à des fins de test. Toutefois, si vous souhaitez tester par rapport à un élément qui se comporte davantage comme une base de données relationnelle true, envisagez d’utiliser [mode in-memory de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées à des blogs. Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server. Il peut être utile d’échange de ce contexte pour se connecter à une base de données en mémoire afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code ou effectuer beaucoup de travail pour créer un test double du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparer votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer les deux fournisseurs de base de données

Dans vos tests, vous vous apprêtez à l’extérieur de configurer le contexte pour utiliser le fournisseur en mémoire. Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si une n’a pas déjà été configurée.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si vous utilisez ASP.NET Core, puis vous devez pas ce code depuis votre fournisseur de base de données est déjà configuré en dehors du contexte (dans Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Ajoutez un constructeur pour le test

La méthode la plus simple pour activer le test par rapport à une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Indique le contexte de tous ses paramètres, tels que la base de données pour se connecter à. Il s’agit de l’objet qui est généré par la méthode OnConfiguring en cours d’exécution dans votre contexte.

## <a name="writing-tests"></a>Écriture de tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte à utiliser le fournisseur en mémoire et contrôler l’étendue de la base de données en mémoire. En règle générale, vous souhaitez une nouvelle base de données pour chaque méthode de test.

Voici un exemple d’une classe de test qui utilise la base de données en mémoire. Chaque méthode de test spécifie un nom de base de données unique, ce qui signifie que chaque méthode a sa propre base de données en mémoire.

>[!TIP]
> Pour utiliser le `.UseInMemoryDatabase()` référence le package Nuget, méthode d’extension `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
