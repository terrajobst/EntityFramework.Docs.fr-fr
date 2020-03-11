---
title: Test avec InMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416508"
---
# <a name="testing-with-inmemory"></a>Test avec InMemory

Le fournisseur d’InMemory est utile lorsque vous souhaitez tester des composants à l’aide d’un composant qui se rapproche de la connexion à la base de données réelle, sans la surcharge des opérations de base de données réelles.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>La mémoire n’est pas une base de données relationnelle

EF Core les fournisseurs de base de données n’ont pas besoin d’être des bases de données relationnelles. La mémoire est conçue comme une base de données à usage général à des fins de test et n’est pas conçue pour imiter une base de données relationnelle.

Voici quelques exemples :

* InMemory vous permet d’enregistrer des données qui enfreignent les contraintes d’intégrité référentielle dans une base de données relationnelle.
* Si vous utilisez DefaultValueSql (String) pour une propriété dans votre modèle, il s’agit d’une API de base de données relationnelle qui n’aura aucun effet lors de l’exécution sur InMemory.
* La [concurrence via l’horodateur/la version de ligne](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` ou `IsRowVersion`) n’est pas prise en charge. Aucune [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) n’est levée si une mise à jour est effectuée à l’aide d’un ancien jeton d’accès concurrentiel.

> [!TIP]  
> Dans de nombreux cas de test, ces différences n’ont pas d’importance. Toutefois, si vous souhaitez tester des éléments qui se comportent davantage comme une véritable base de données relationnelle, envisagez d’utiliser [le mode en mémoire SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Examinez le service suivant qui permet au code de l’application d’effectuer des opérations liées aux blogs. En interne, elle utilise un `DbContext` qui se connecte à une base de données SQL Server. Il serait utile de permuter ce contexte pour se connecter à une base de données InMemory afin que nous puissions écrire des tests efficaces pour ce service sans avoir à modifier le code, ou faire beaucoup de travail pour créer un double de test du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparez votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer deux fournisseurs de base de données

Dans vos tests, vous allez configurer en externe le contexte pour utiliser le fournisseur d’InMemory. Si vous configurez un fournisseur de base de données en remplaçant `OnConfiguring` dans votre contexte, vous devez ajouter du code conditionnel pour vous assurer que vous ne configurez le fournisseur de base de données que s’il n’a pas déjà été configuré.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si vous utilisez ASP.NET Core, vous n’avez pas besoin de ce code, car votre fournisseur de base de données est déjà configuré en dehors du contexte (dans Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Ajouter un constructeur pour le test

La façon la plus simple d’activer les tests sur une autre base de données consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indique au contexte tous ses paramètres, tels que la base de données à laquelle se connecter. Il s’agit du même objet que celui généré par l’exécution de la méthode OnConfiguring dans votre contexte.

## <a name="writing-tests"></a>Écrire des tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer au contexte d’utiliser le fournisseur d’InMemory et de contrôler l’étendue de la base de données en mémoire. En général, vous souhaitez une base de données propre pour chaque méthode de test.

Voici un exemple de classe de test qui utilise la base de données InMemory. Chaque méthode de test spécifie un nom de base de données unique, ce qui signifie que chaque méthode possède sa propre base de données InMemory.

>[!TIP]
> Pour utiliser la méthode d’extension `.UseInMemoryDatabase()`, référencez le package NuGet [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
