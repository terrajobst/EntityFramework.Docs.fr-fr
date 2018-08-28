---
title: Conventions Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995524"
---
# <a name="code-first-conventions"></a>Conventions Code First
Code First vous permet de décrire un modèle à l’aide de classes c# ou Visual Basic .NET. La forme de base du modèle est détectée à l’aide des conventions. Conventions des ensembles de règles qui sont utilisés pour configurer automatiquement un modèle conceptuel basé sur les définitions de classe lorsque vous travaillez avec Code First. Les conventions sont définies dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions.  

Vous pouvez configurer davantage de votre modèle à l’aide des annotations de données ou l’API fluent. La priorité est donnée à la configuration via l’API fluent, suivi de conventions et des annotations de données. Pour plus d’informations, consultez [Annotations de données](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relations](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - Types de & propriétés](~/ef6/modeling/code-first/fluent/types-and-properties.md) et [API Fluent avec VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Une liste détaillée des conventions Code First est disponible dans le [Documentation de l’API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Cette rubrique fournit une vue d’ensemble des conventions utilisées par Code First.  

## <a name="type-discovery"></a>Découverte de type  

Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine). En plus de définir les classes, vous devez également communiquer à **DbContext** connaître les types que vous souhaitez inclure dans le modèle. Pour ce faire, vous définissez une classe de contexte qui dérive de **DbContext** et expose **DbSet** propriétés pour les types que vous souhaitez faire partie du modèle. Code inclut tout d’abord ces types et également permet d’extraire tous les types référencés, même si les types référencés sont définis dans un autre assembly.  

Si vos types de participent à une hiérarchie d’héritage, il suffit de définir un **DbSet** propriété pour la classe de base et les types dérivés qui sera incluse automatiquement, si elles se trouvent dans le même assembly que la classe de base.  

Dans l’exemple suivant, il existe un seul **DbSet** propriété définie sur le **SchoolEntities** classe (**départements**). Code utilise cette propriété pour découvrir et d’extraire tous les types référencés.  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

Si vous souhaitez exclure un type à partir du modèle, utilisez le **NotMapped** attribut ou le **DbModelBuilder.Ignore** API fluent.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Convention de clé primaire  

Code déduit tout d’abord qu’une propriété est une clé primaire si une propriété sur une classe est nommée « ID » (non sensible à la casse), ou le nom de classe suivie de « ID ». Si le type de la propriété de clé primaire est numérique ou le GUID sera configuré comme une colonne d’identité.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Convention de relation  

Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités. Chaque objet peut avoir une propriété de navigation pour chaque relation à laquelle il participe. Propriétés de navigation permettent de parcourir et de gérer les relations dans les deux directions, en retournant un objet de la référence (si la multiplicité est soit un ou zéro-ou-un) ou une collection (si la multiplicité est nombreuses). Code déduit tout d’abord les relations basées sur les propriétés de navigation définies sur vos types.  

En plus des propriétés de navigation, nous vous recommandons d’inclure des propriétés de clé étrangère sur les types qui représentent des objets dépendants. N’importe quelle propriété avec le même type de données en tant que propriété de clé primaire principal et avec un nom qui suit l’un des formats suivants représente une clé étrangère pour la relation : «\<nom de propriété de navigation\>\<principal nom de propriété de clé primaire\>','\<nom de la classe principal\>\<nom de propriété de clé primaire\>», ou «\<nom de propriété de clé primaire principal\>». Si plusieurs correspondances sont trouvées priorité est donnée dans l’ordre indiqué ci-dessus. La détection de clé étrangère n’est pas sensible à la casse. Lorsqu’une propriété de clé étrangère est détectée, Code First déduit la multiplicité de la relation en fonction de la possibilité de valeur de la clé étrangère. Si la propriété est nullable, la relation est enregistrée comme étant facultatifs ; Sinon, la relation est inscrit en fonction des besoins.  

Si une clé étrangère sur l’entité dépendante n’est pas nullable, Code First définit ensuite suppression en cascade sur la relation. Si une clé étrangère sur l’entité dépendante est nullable, Code First ne définit pas suppression en cascade sur la relation, et lorsque le principal est supprimé la clé étrangère sera définie sur null. La multiplicité et cascade delete comportement détecté par convention peut être substituée à l’aide de l’API fluent.  

Dans l’exemple suivant, les propriétés de navigation et une clé étrangère sont utilisés pour définir la relation entre les classes de service et un cours.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> Si vous avez plusieurs relations entre les mêmes types (par exemple, supposons que vous définissiez le **personne** et **livre** classes, où le **personne** classe contient le  **ReviewedBooks** et **AuthoredBooks** propriétés de navigation et la **livre** classe contient le **auteur** et  **Réviseur** propriétés de navigation) vous devez configurer manuellement les relations à l’aide des Annotations de données ou l’API fluent. Pour plus d’informations, consultez [Annotations de données - relations](~/ef6/modeling/code-first/data-annotations.md) et [API Fluent - relations](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Convention de Types complexes  

Lorsque Code First détecte une définition de classe où une clé primaire ne peut pas être déduite, et aucune clé primaire n’est inscrit par le biais des annotations de données ou l’API fluent, puis le type est automatiquement enregistré comme un type complexe. Détection de type complexe requiert également que le type n’a pas de propriétés qui référencent des types d’entités et ne sont pas référencées à partir d’une propriété de collection sur un autre type. Compte tenu des définitions de classe suivant Code First serait déduire que **détails** est un type complexe, car il n’a aucune clé primaire.  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a>Convention de chaîne de connexion  

Pour en savoir plus sur les conventions que DbContext utilise pour détecter la connexion à utiliser voir [connexions et des modèles](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Suppression des Conventions  

Vous pouvez supprimer une des conventions définies dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions. L’exemple suivant supprime **PluralizingTableNameConvention**.  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a>Conventions personnalisées  

Conventions personnalisées sont prises en charge dans EF6 et versions ultérieures. Pour plus d’informations, consultez [Conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md).
