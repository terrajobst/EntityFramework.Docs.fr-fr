---
title: Test de SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052699"
---
# <a name="testing-with-sqlite"></a>Test de SQLite

SQLite a un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests par rapport à une base de données relationnelle, sans la surcharge des opérations de base de données.

> [!TIP]  
> Vous pouvez afficher cet article [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées à des blogs. Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server. Il peut être utile d’échange de ce contexte pour se connecter à une base de données en mémoire SQLite afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code ou effectuer beaucoup de travail pour créer un test double du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparer votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer les deux fournisseurs de base de données

Dans vos tests, vous vous apprêtez à l’extérieur de configurer le contexte pour utiliser le fournisseur en mémoire. Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si une n’a pas déjà été configurée.

> [!TIP]  
> Si vous utilisez ASP.NET Core, puis vous devez pas ce code depuis votre fournisseur de base de données est configurée en dehors du contexte (dans Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Ajoutez un constructeur pour le test

La méthode la plus simple pour activer le test par rapport à une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Indique le contexte de tous ses paramètres, tels que la base de données pour se connecter à. Il s’agit de l’objet qui est généré par la méthode OnConfiguring en cours d’exécution dans votre contexte.

## <a name="writing-tests"></a>Écriture de tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte à utiliser SQLite, contrôler l’étendue de la base de données en mémoire. L’étendue de la base de données est contrôlé par l’ouverture et de fermeture de la connexion. La portée de la base de données est limitée à la durée pendant laquelle la connexion est ouverte. En règle générale, vous souhaitez une nouvelle base de données pour chaque méthode de test.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
