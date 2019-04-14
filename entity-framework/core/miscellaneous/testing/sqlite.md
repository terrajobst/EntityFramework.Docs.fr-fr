---
title: Test avec SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562531"
---
# <a name="testing-with-sqlite"></a>Test avec SQLite

SQLite a un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests par rapport à une base de données relationnelle, sans la surcharge des opérations de base de données réelle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées aux blogs. Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server. Il serait utile pour le remplacement de ce contexte pour se connecter à une base de données en mémoire SQLite afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code, ou vous pouvez faire beaucoup de travail pour créer un test double du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparer votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer deux fournisseurs de base de données

Dans vos tests, vous vous apprêtez à configurer en externe le contexte pour utiliser le fournisseur en mémoire. Si vous configurez un fournisseur de base de données en substituant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si elle n’a pas déjà été configurée.

> [!TIP]  
> Si vous utilisez ASP.NET Core, vous devez inutile ce code dans la mesure où votre fournisseur de base de données est configuré en dehors du contexte (dans Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Ajoutez un constructeur pour le test

Pour activer le test par rapport à une autre base de données le plus simple consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Indique le contexte à tous ses paramètres, tels que la base de données pour se connecter à. Il s’agit de l’objet qui est généré en exécutant la méthode OnConfiguring dans votre contexte.

## <a name="writing-tests"></a>Écriture de tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte à utiliser SQLite et contrôler la portée de la base de données en mémoire. L’étendue de la base de données est contrôlé par l’ouverture et fermeture de la connexion. La base de données est limitée à la durée pendant laquelle la connexion est ouverte. En général, vous souhaitez une nouvelle base de données pour chaque méthode de test.

>[!TIP]
> Pour utiliser `SqliteConnection()` et `.UseSqlite()` référence le package NuGet, méthode d’extension [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
