---
title: Test avec SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417301"
---
# <a name="testing-with-sqlite"></a>Test avec SQLite

SQLite dispose d’un mode en mémoire qui vous permet d’utiliser SQLite pour écrire des tests sur une base de données relationnelle, sans la surcharge des opérations de base de données réelles.

> [!TIP]  
> Vous pouvez consulter l' [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de cet article sur GitHub

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Examinez le service suivant qui permet au code de l’application d’effectuer des opérations liées aux blogs. En interne, elle utilise un `DbContext` qui se connecte à une base de données SQL Server. Il serait utile de permuter ce contexte pour se connecter à une base de données SQLite en mémoire afin que nous puissions écrire des tests efficaces pour ce service sans avoir à modifier le code, ou faire beaucoup de travail pour créer un double de test du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparez votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer deux fournisseurs de base de données

Dans vos tests, vous allez configurer en externe le contexte pour utiliser le fournisseur d’InMemory. Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ajouter du code conditionnel pour vous assurer que vous ne configurez le fournisseur de base de données que s’il n’a pas déjà été configuré.

> [!TIP]  
> Si vous utilisez ASP.NET Core, vous n’avez pas besoin de ce code, car votre fournisseur de base de données est configuré en dehors du contexte (dans Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Ajouter un constructeur pour le test

La façon la plus simple d’activer les tests sur une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indique au contexte tous ses paramètres, tels que la base de données à laquelle se connecter. Il s’agit du même objet que celui généré par l’exécution de la méthode OnConfiguring dans votre contexte.

## <a name="writing-tests"></a>Écrire des tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer au contexte d’utiliser SQLite et de contrôler l’étendue de la base de données en mémoire. L’étendue de la base de données est contrôlée par l’ouverture et la fermeture de la connexion. La portée de la base de données est limitée à la durée pendant laquelle la connexion est ouverte. En général, vous souhaitez une base de données propre pour chaque méthode de test.

>[!TIP]
> Pour utiliser `SqliteConnection()` et la méthode d’extension `.UseSqlite()`, référencez le package NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
