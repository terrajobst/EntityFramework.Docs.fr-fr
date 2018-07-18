---
title: Utilisation de valeurs de propriété - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120846"
---
# <a name="working-with-property-values"></a>Utilisation de valeurs de propriété
La plupart du temps Entity Framework se chargera de suivi de l’état, les valeurs d’origine et les valeurs actuelles des propriétés de vos instances d’entité. Toutefois, il peut être certains cas, par exemple les scénarios déconnectés - où vous souhaitez afficher ou manipuler les informations QU'EF a les propriétés. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

Entity Framework effectue le suivi de deux valeurs pour chaque propriété d’une entité suivie. La valeur actuelle est, comme son nom l’indique, la valeur actuelle de la propriété dans l’entité. La valeur d’origine est la valeur de la propriété avait lorsque l’entité a été interrogée à partir de la base de données ou attachée au contexte.  

Il existe deux mécanismes généraux pour l’utilisation des valeurs de propriété :  

- La valeur d’une propriété unique peut être obtenue de manière fortement typée à l’aide de la méthode de propriété.  
- Valeurs pour toutes les propriétés d’une entité peuvent être lues dans un objet DbPropertyValues. DbPropertyValues agit alors comme un objet de type dictionnaire permettant d’autoriser les valeurs de propriété à lire et à définir. Les valeurs dans un objet DbPropertyValues peuvent être définies à partir des valeurs dans un autre objet DbPropertyValues ou à partir des valeurs dans un autre objet, par exemple une autre copie de l’entité ou un objet de transfert de données simple (DTO).  

Les sections ci-dessous montrent des exemples d’utilisation à la fois des mécanismes ci-dessus.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Obtention et définition de la valeur actuelle ou d’origine d’une propriété individuelle  

L’exemple ci-dessous montre comment la valeur actuelle d’une propriété peut être lu et puis définissez une nouvelle valeur :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Utilisez la propriété OriginalValue au lieu de la propriété CurrentValue pour lire ou définir la valeur d’origine.  

Notez que la valeur retournée est typée en tant que « objet » lorsqu’une chaîne est utilisée pour spécifier le nom de propriété. En revanche, la valeur retournée est fortement typée si une expression lambda est utilisée.  

Définition de la valeur de propriété comme ceci uniquement marque la propriété comme modifiée si la nouvelle valeur est différente de l’ancienne valeur.  

Lorsqu’une valeur de propriété est définie de cette façon la modification est détectée automatiquement même si AutoDetectChanges est désactivée.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Obtention et définition de la valeur actuelle d’une propriété non mappée  

La valeur actuelle d’une propriété qui n’est pas mappée à la base de données peut également être lu. Un exemple d’une propriété non mappée peut être une propriété RssLink sur le Blog. Cette valeur peut être calculée en fonction de la BlogId et par conséquent n’a pas besoin être stockées dans la base de données. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

La valeur actuelle peut également indiquer si la propriété expose une méthode setter.  

Il est utile de lire les valeurs des propriétés non mappées lors de la validation d’Entity Framework de propriétés non mappées. Pour la même raison les valeurs actuelles peuvent lire et définir des propriétés d’entités qui ne sont pas actuellement suivies par le contexte. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Notez que les valeurs d’origine ne sont pas disponibles pour les propriétés non mappées ou pour les propriétés d’entités qui ne sont pas suivies par le contexte.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Vérifier si une propriété est marquée comme modifiée  

L’exemple ci-dessous montre comment vérifier si une propriété individuelle est marquée comme modifiée :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Les valeurs des propriétés modifiées sont envoyées en tant que mises à jour à la base de données lorsque SaveChanges est appelée.  

##  <a name="marking-a-property-as-modified"></a>Marquage d’une propriété comme modifiée  

L’exemple ci-dessous montre comment forcer une propriété individuelle à être marquée comme modifiée :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Marquage d’une propriété comme modifiée force une mise à jour à envoyer à la base de données pour la propriété lorsque SaveChanges est appelée même si la valeur actuelle de la propriété est identique à sa valeur d’origine.  

Il n’est pas encore possible de réinitialiser une propriété individuelle ne pas modifiables après que qu’il a été marquée comme modifiée. C’est quelque chose que nous prévoyons de prendre en charge dans une version ultérieure.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>En cours de lecture, d’origine et les valeurs de base de données pour toutes les propriétés d’une entité  

L’exemple ci-dessous montre comment lire les valeurs actuelles, les valeurs d’origine et les valeurs réellement dans la base de données pour toutes les propriétés mappées d’une entité.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

Les valeurs actuelles sont les valeurs contenant les propriétés de l’entité actuellement. Les valeurs d’origine sont les valeurs qui ont été lus à partir de la base de données lors de l’entité a été interrogée. Les valeurs de la base de données sont les valeurs qu’ils sont actuellement stockés dans la base de données. Obtenir les valeurs de la base de données est utile lorsque les valeurs dans la base de données a peut-être modifié depuis l’entité a été interrogée telles que lorsqu’une simultanées modifier à la base de données a été effectuée par un autre utilisateur.  

## <a name="setting-current-or-original-values-from-another-object"></a>Définition des valeurs actuelles ou d’origine à partir d’un autre objet  

Les valeurs d’origine ou en cours d’une entité de suivi peuvent être mis à jour en copiant les valeurs d’un autre objet. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

Exécution du code ci-dessus imprimera :  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Cette technique est parfois utilisée lors de la mise à jour une entité avec les valeurs obtenues à partir d’un appel de service ou un client dans une application multicouche. Notez que l’objet utilisé ne devra pas être du même type que l’entité afin qu’il dispose des propriétés dont les noms correspondent à ceux de l’entité. Dans l’exemple ci-dessus, une instance de BlogDTO est utilisée pour mettre à jour les valeurs d’origine.  

Notez que seules les propriétés qui sont définies sur des valeurs différentes lorsque copiés à partir de l’autre objet seront marquées comme modifié.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Définition des valeurs d’origine ou en cours d’un dictionnaire  

Les valeurs d’origine ou en cours d’une entité de suivi peuvent être mis à jour en copiant les valeurs d’un dictionnaire ou une autre structure de données. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

Utilisez la propriété OriginalValues au lieu de la propriété CurrentValues pour définir les valeurs d’origine.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Définition des valeurs actuelles ou d’origine à partir d’un dictionnaire à l’aide de la propriété  

Une alternative à l’aide de CurrentValues ou OriginalValues comme indiqué ci-dessus est d’utiliser la méthode de propriété pour définir la valeur de chaque propriété. Cela peut être préférable lorsque vous avez besoin définir les valeurs de propriétés complexes. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

Dans l’exemple ci-dessus des propriétés complexes sont accessibles à l’aide de noms en pointillés. Pour d’autres façons d’accéder aux propriétés complexes, consultez les deux sections plus loin dans cette rubrique en particulier sur les propriétés complexes.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Création d’un objet cloné contenant actuel, d’origine ou les valeurs de base de données  

L’objet DbPropertyValues retournée à partir de CurrentValues, OriginalValues, ou GetDatabaseValues peut être utilisé pour créer un clone de l’entité. Ce clone doit contenir les valeurs de propriété à partir de l’objet DbPropertyValues utilisé pour le créer. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Notez que l’objet retourné n’est pas l’entité et qu’il n’est pas suivie par le contexte. L’objet retourné n’a pas plus de toutes les relations définies à d’autres objets.  

L’objet cloné peut être utile pour la résolution des problèmes liés aux mises à jour simultanées à la base de données, en particulier dans lequel une interface utilisateur qui implique la liaison de données aux objets d’un certain type est utilisée.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Obtention et définition des valeurs actuelles ou d’origine de propriétés complexes  

La valeur d’un objet complexe entier peut être lu et défini à l’aide de la méthode de propriété comme il peut être pour une propriété primitive. En outre vous pouvez examiner l’objet complexe et lecture ou définies les propriétés de cet objet, ou même un objet imbriqué. Voici quelques exemples :  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

Utilisez la propriété OriginalValue au lieu de la propriété CurrentValue à obtenir ou définir une valeur d’origine.  

Notez que la propriété ou la méthode ComplexProperty peut être utilisée pour accéder à une propriété complexe. Toutefois, la méthode ComplexProperty doit être utilisée si vous souhaitez approfondir l’objet complexe avec une propriété supplémentaire ou ComplexProperty appelle.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>À l’aide de DbPropertyValues pour accéder aux propriétés complexes  

Lorsque vous utilisez CurrentValues, OriginalValues ou GetDatabaseValues pour obtenir tous les cours, d’origine, ou les valeurs de base de données pour une entité, les valeurs de toutes les propriétés complexes sont retournées en tant qu’objets DbPropertyValues imbriqués. Ces imbriqué des objets peut alors être utilisées pour obtenir les valeurs de l’objet complexe. Par exemple, la méthode suivante affiche les valeurs de toutes les propriétés, y compris les valeurs de toutes les propriétés complexes et des propriétés complexes imbriquées.  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

Pour imprimer toutes les valeurs de propriété actuelles la méthode est appelée comme suit :  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
