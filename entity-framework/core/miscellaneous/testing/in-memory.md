---
title: Test avec InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997890"
---
# <a name="testing-with-inmemory"></a>Test avec InMemory

Le fournisseur en mémoire est utile lorsque vous souhaitez tester les composants à l’aide d’un élément qui est une approximation de la connexion à la base de données réelle, sans la surcharge des opérations de base de données réelle.

> [!TIP]  
> Vous pouvez afficher cet [exemple](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) sur GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>En mémoire n’est pas une base de données relationnelle

Fournisseurs de base de données EF Core n’ont pas à être des bases de données relationnelles. InMemory est conçu pour être une base de données à usage général pour le test et n’est pas conçu pour imiter une base de données relationnelle.

Voici quelques exemples de ce sont :

* InMemory vous permettra enregistrer les données qui risque de violer les contraintes d’intégrité référentielle dans une base de données relationnelle.
* Si vous utilisez DefaultValueSql(string) pour une propriété dans votre modèle, cela est une API de base de données relationnelle et n’a aucun effet lors de l’exécution par rapport à InMemory.
* [Accès concurrentiel via la version de l’horodatage/ligne](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` ou `IsRowVersion`) n’est pas pris en charge. Ne [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) sera levée si une mise à jour est effectuée à l’aide d’un ancien jeton d’accès concurrentiel.

> [!TIP]  
> Pour autant à des fins de test ces différences ne seront pas important. Toutefois, si vous souhaitez tester par rapport à quelque chose qui se comporte davantage comme une véritable base de données relationnelle, envisagez d’utiliser [mode in-memory de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Exemple de scénario de test

Envisagez le service suivant qui permet au code d’application effectuer certaines opérations liées aux blogs. Il utilise en interne un `DbContext` qui se connecte à une base de données SQL Server. Il serait utile pour le remplacement de ce contexte pour se connecter à une base de données en mémoire afin que nous pouvons écrire des tests efficaces pour ce service sans avoir à modifier le code, ou vous pouvez faire beaucoup de travail pour créer un test double du contexte.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Préparer votre contexte

### <a name="avoid-configuring-two-database-providers"></a>Évitez de configurer deux fournisseurs de base de données

Dans vos tests, vous vous apprêtez à configurer en externe le contexte pour utiliser le fournisseur en mémoire. Si vous configurez un fournisseur de base de données en substituant `OnConfiguring` dans votre contexte, vous devez ensuite ajouter du code conditionnel afin de configurer le fournisseur de base de données uniquement si elle n’a pas déjà été configurée.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si vous utilisez ASP.NET Core, vous devez inutile ce code dans la mesure où votre fournisseur de base de données est déjà configuré en dehors du contexte (dans Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Ajoutez un constructeur pour le test

Pour activer le test par rapport à une autre base de données le plus simple consiste à modifier votre contexte pour exposer un constructeur qui accepte un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Indique le contexte à tous ses paramètres, tels que la base de données pour se connecter à. Il s’agit de l’objet qui est généré en exécutant la méthode OnConfiguring dans votre contexte.

## <a name="writing-tests"></a>Écriture de tests

La clé à tester avec ce fournisseur est la possibilité d’indiquer le contexte d’utiliser le fournisseur en mémoire et de contrôler l’étendue de la base de données en mémoire. En général, vous souhaitez une nouvelle base de données pour chaque méthode de test.

Voici un exemple d’une classe de test qui utilise la base de données en mémoire. Chaque méthode de test spécifie un nom de base de données unique, ce qui signifie que chaque méthode possède sa propre base de données en mémoire.

>[!TIP]
> Pour utiliser le `.UseInMemoryDatabase()` référence le package NuGet, méthode d’extension `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
