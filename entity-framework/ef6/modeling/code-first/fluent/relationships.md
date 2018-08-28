---
title: API Fluent - relations - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: e13a1370f46362e0f2ca3743ec5b063ce6f87cbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994760"
---
# <a name="fluent-api---relationships"></a>API Fluent - relations
> [!NOTE]
> Cette page fournit des informations sur la définition des relations dans votre modèle Code First à l’aide de l’API fluent. Pour obtenir des informations générales sur les relations dans Entity Framework et comment accéder à et manipuler des données à l’aide de relations, consultez [relations & Propriétés de Navigation](~/ef6/fundamentals/relationships.md).  

Lorsque vous travaillez avec Code First, vous définissez votre modèle en définissant vos classes CLR de domaine. Par défaut, Entity Framework utilise les conventions Code First pour mapper vos classes pour le schéma de base de données. Si vous utilisez les conventions d’affectation de noms de Code First, dans la plupart des cas, vous pouvez compter sur Code First pour définir des relations entre vos tables basées sur les clés étrangères et les propriétés de navigation que vous définissez sur les classes. Si vous ne suivez pas les conventions lors de la définition de vos classes, ou si vous souhaitez modifier la façon dont les conventions de fonctionnent, vous pouvez utiliser l’API fluent ou des annotations de données pour configurer vos classes de Code First peut mapper les relations entre vos tables.  

## <a name="introduction"></a>Introduction  

Lorsque vous configurez une relation avec l’API fluent, vous commencez par l’instance EntityTypeConfiguration, puis utilisez la méthode HasRequired, HasOptional ou HasMany pour spécifier le type de relation de que cette entité participe. Les méthodes HasRequired et HasOptional prennent une expression lambda qui représente une propriété de navigation de référence. La méthode HasMany prend une expression lambda qui représente une propriété de navigation de collection. Vous pouvez ensuite configurer une propriété de navigation inverse en utilisant les méthodes WithRequired, WithOptional et WithMany. Ces méthodes ont des surcharges qui ne prennent pas d’arguments et peuvent être utilisées pour spécifier cardinalité avec navigation unidirectionnelle.  

Vous pouvez ensuite configurer les propriétés de clé étrangère à l’aide de la méthode HasForeignKey. Cette méthode prend une expression lambda qui représente la propriété à utiliser comme clé étrangère.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Configuration d’une relation requise-à-facultatif (un-à-zéro-ou-un)  

L’exemple suivant configure une relation un-à-zéro-ou-un. Le OfficeAssignment a la propriété InstructorID qui est une clé primaire et une clé étrangère, étant donné que le nom de la propriété ne respecte pas la convention, que la méthode HasKey permet de configurer la clé primaire.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Configuration d’une relation où les deux extrémités sont nécessaires (-à-un)  

Dans la plupart des cas Entity Framework peut déduire le type dépend et qui est le principal dans une relation. Toutefois, lorsque les deux extrémités de la relation sont requises ou les deux côtés sont facultatifs Entity Framework ne peut pas identifier le principal et dépendants. Lorsque les deux extrémités de la relation sont requises, utilisez WithRequiredPrincipal ou WithRequiredDependent après la méthode HasRequired. Lorsque les deux extrémités de la relation sont facultatifs, utilisez WithOptionalPrincipal ou WithOptionalDependent après la méthode HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Configuration d’une relation plusieurs-à-plusieurs  

Le code suivant configure une relation plusieurs-à-plusieurs entre les types de Course et Instructor. Dans l’exemple suivant, les conventions Code First par défaut sont utilisées pour créer une table de jointure. Par conséquent, la table CourseInstructor est créée avec des colonnes Course_CourseID et Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Si vous souhaitez spécifier le nom de table de jointure et les noms des colonnes de la table, vous devez effectuer une configuration supplémentaire à l’aide de la méthode de mappage. Le code suivant génère la table CourseInstructor avec des colonnes CourseID et InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Configuration d’une relation avec une propriété de Navigation  

Un unidirectionnelle (également appelé unidirectionnel) relation est lorsqu’une propriété de navigation est définie sur un seul des extrémités de relation et non sur les deux. Par convention, Code First interprète toujours une relation unidirectionnelle comme un-à-plusieurs. Par exemple, si vous souhaitez une relation un à un entre formateur et OfficeAssignment, où vous avez une propriété de navigation sur uniquement le type Instructor, vous devez utiliser l’API fluent pour configurer cette relation.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>L’activation de suppression en Cascade  

Vous pouvez configurer la suppression en cascade sur une relation à l’aide de la méthode WillCascadeOnDelete. Si une clé étrangère sur l’entité dépendante n’est pas nullable, Code First définit ensuite suppression en cascade sur la relation. Si une clé étrangère sur l’entité dépendante est nullable, Code First ne définit pas suppression en cascade sur la relation, et lorsque le principal est supprimé la clé étrangère sera définie sur null.  

Vous pouvez supprimer ces conventions de suppression en cascade à l’aide de :  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()  

Le code suivant configure la relation comme étant obligatoire et puis désactive la suppression en cascade.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Configuration d’une clé étrangère Composite  

Si la clé primaire sur le type de service est constitué de propriétés DepartmentID et Name, vous devez configurez la clé primaire pour le service et la clé étrangère sur les types de cours comme suit :  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modification du nom d’une clé étrangère qui n’est pas définie dans le modèle  

Si vous choisissez de ne pas définir une clé étrangère sur le type CLR, mais souhaitez spécifier quel nom, il doit avoir dans la base de données, procédez comme suit :  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Configuration d’un nom de clé étrangère qui ne respecte pas la Convention de premier Code  

Si la propriété de clé étrangère sur la classe de cours était appelée SomeDepartmentID au lieu de DepartmentID, vous devez procédez comme suit pour spécifier que vous souhaitez SomeDepartmentID qui sera la clé étrangère :  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Modèle utilisé dans les exemples  

Le modèle Code First suivant est utilisé pour les exemples de cette page.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
