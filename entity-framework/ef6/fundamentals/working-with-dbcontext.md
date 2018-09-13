---
title: Utilisation de DbContext - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489061"
---
# <a name="working-with-dbcontext"></a>Utilisation de DbContext

Pour utiliser Entity Framework pour interroger, insérer, mettre à jour et supprimer des données à l’aide d’objets .NET, vous devez d’abord [créer un modèle](~/ef6/modeling/index.md) qui mappe les entités et les relations qui sont définies dans votre modèle à des tables dans une base de données.

Une fois que vous avez un modèle, la classe principale, votre application interagit avec est `System.Data.Entity.DbContext` (souvent appelé la classe de contexte). Vous pouvez utiliser un DbContext associé à un modèle pour :
- Écrire et exécuter des requêtes   
- Matérialiser les résultats de la requête en tant qu’objets d’entité
- Le suivi des modifications apportées à ces objets.
- Conserver les modifications de l’objet dans la base de données
- Lier des objets en mémoire pour les contrôles d’interface utilisateur

Cette page fournit des conseils sur la gestion de la classe de contexte.  

## <a name="defining-a-dbcontext-derived-class"></a>Définition d’une classe DbContext dérivée  

La méthode recommandée pour travailler avec contexte consiste à définir une classe qui dérive de DbContext et expose les propriétés DbSet qui représentent des collections d’entités dans le contexte spécifiées. Si vous travaillez avec le Concepteur EF, le contexte sera généré pour vous. Si vous utilisez Code First, vous allez généralement écrire le contexte vous-même.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Une fois que vous disposez d’un contexte, vous interroger, ajouter (à l’aide de `Add` ou `Attach` méthodes) ou supprimer (à l’aide de `Remove`) les entités dans le contexte via ces propriétés. L’accès à un `DbSet` propriété sur un objet de contexte représentent une requête initiale qui retourne toutes les entités du type spécifié. Notez que l’accès à une propriété pas exécute la requête. Une requête est exécutée lorsque :  

- elle est énumérée par une instruction `foreach` (C#) ou `For Each` (Visual Basic) ;  
- Elle est énumérée par une opération de collection comme `ToArray`, `ToDictionary`, ou `ToList`.  
- Les opérateurs LINQ, tels que `First` ou `Any` sont spécifiés dans la partie la plus extérieure de la requête.  
- Une des méthodes suivantes sont appelées : le `Load` méthode d’extension, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, et `DbSet<T>.Find`, si une entité avec la clé spécifiée se trouve pas déjà chargée dans le contexte.  

## <a name="lifetime"></a>Durée de vie  

La durée de vie du contexte commence lorsque l’instance est créée et se termine lorsque l’instance est supprimée ou opération de garbage collection. Utilisez **à l’aide de** si vous voulez que toutes les ressources contrôlées par le contexte soient supprimées à la fin du bloc. Lorsque vous utilisez **à l’aide de**, le compilateur crée automatiquement un bloc try/finally et appelle la méthode dispose dans le **enfin** bloc.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Voici quelques instructions générales lorsque vous décidez de la durée de vie du contexte :  

- Lorsque vous travaillez avec les applications Web, utilisez une instance de contexte par demande.  
- Lorsque vous travaillez avec Windows Presentation Foundation (WPF) ou Windows Forms, utilisez une instance de contexte par formulaire. Cela vous permet d’utiliser des fonctionnalités de suivi des modifications fournit ce contexte.  
- Si l’instance de contexte est créée par un conteneur d’injection de dépendance, il est généralement la responsabilité du conteneur à supprimer le contexte.
- Si le contexte est créé dans le code d’application, pensez à supprimer le contexte lorsqu’il n’est plus nécessaire.  
- Lorsque vous travaillez avec le contexte d’exécution longue, considérez les points suivants :  
    - Comme vous chargez des objets et leurs références dans la mémoire, du contexte de la consommation de mémoire peut augmenter rapidement. Cela peut provoquer des problèmes de performances.  
    - Le contexte n’est pas thread-safe, par conséquent, il ne doit pas être partagé entre plusieurs threads travailler simultanément sur celui-ci.
    - Si une exception provoque le contexte dans un état irrécupérable, l’application entière peut mettre fin.  
    - Les risques liés à l'exécution dans un état d'accès concurrentiel augmentent à mesure que l'intervalle entre le moment où les données sont interrogées et mises à jour s'allonge.  

## <a name="connections"></a>Connexions  

Par défaut, le contexte gère les connexions à la base de données. Le contexte s’ouvre et ferme les connexions en fonction des besoins. Par exemple, le contexte ouvre une connexion pour exécuter une requête, puis ferme la connexion lorsque tous les jeux de résultats ont été traitées.  

Dans certains cas, vous souhaiterez avoir davantage de contrôle sur l'ouverture et la fermeture des connexions. Par exemple, lorsque vous travaillez avec SQL Server Compact, il est souvent recommandé de maintenir une connexion à la base de données ouverte distincte pour la durée de vie de l’application pour améliorer les performances. Vous pouvez gérer ce processus manuellement à l'aide de la propriété `Connection`.  
