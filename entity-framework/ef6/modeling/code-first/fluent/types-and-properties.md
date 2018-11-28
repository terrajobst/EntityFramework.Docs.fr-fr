---
title: API Fluent - configuration et mappage des Types et propriétés - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: 75f8a179ac9a70ad390fc7ab2a6c5e714e701b8b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339801"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API Fluent - configuration et de mappage des Types et des propriétés
Lorsque vous travaillez avec Entity Framework Code First le comportement par défaut consiste à mapper vos classes POCO à des tables à l’aide d’un ensemble de conventions intégrées à EF. Parfois, cependant, vous ne peut pas ou ne souhaitez pas suivre ces conventions et devez mapper des entités sur autre chose que ce que les conventions de dictent.  

Il existe deux façons principales, vous pouvez configurer EF pour utiliser une valeur autre que de conventions, à savoir [annotations](~/ef6/modeling/code-first/data-annotations.md) ou l’API fluent EFs. Les annotations couvrent uniquement un sous-ensemble des fonctionnalités de l’API fluent, donc il existe des scénarios de mappage qui ne peut pas être obtenus à l’aide d’annotations. Cet article est conçu pour montrer comment utiliser l’API fluent pour configurer les propriétés.  

L’API fluent code first est généralement accessible en substituant le [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) méthode sur votre dérivée [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Les exemples suivants sont conçus pour montrer comment effectuer diverses tâches avec l’api fluent et vous permettent de copier le code et le personnaliser pour l’adapter à votre modèle, si vous souhaitez voir le modèle qui ils peuvent être utilisés avec comme-est alors il est fourni à la fin de cet article.  

## <a name="model-wide-settings"></a>Paramètres de modèle à l’échelle  

### <a name="default-schema-ef6-onwards"></a>Schéma par défaut (Entity Framework 6 et versions ultérieures)  

En commençant par EF6, vous pouvez utiliser la méthode HasDefaultSchema sur DbModelBuilder pour spécifier le schéma de base de données à utiliser pour toutes les tables, procédures stockées, etc. Ce paramètre par défaut est remplacé pour tous les objets que vous configurez explicitement un schéma différent pour.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Conventions personnalisées (Entity Framework 6 et versions ultérieures)  

Démarrage avec EF6, vous pouvez créer vos propres conventions en supplément de ceux inclus dans le Code First. Pour plus d’informations, consultez [Conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mappage de propriétés  

Le [propriété](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) méthode est utilisée pour configurer des attributs pour chaque propriété appartenant à une entité ou un type complexe. La méthode de propriété est utilisée pour obtenir un objet de configuration pour une propriété donnée. Les options de l’objet de configuration sont spécifiques au type en cours de configuration ; IsUnicode est disponible uniquement sur les propriétés de chaîne par exemple.  

### <a name="configuring-a-primary-key"></a>Configuration d’une clé primaire  

La convention d’Entity Framework pour les clés primaires est :  

1. Votre classe définit une propriété dont le nom est « ID » ou « Id »  
2. ou un nom de classe suivie de « ID » ou « Id »  

Pour définir explicitement une propriété soit une clé primaire, vous pouvez utiliser la méthode HasKey. Dans l’exemple suivant, la méthode HasKey permet de configurer la clé primaire InstructorID sur le type OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Configuration d’une clé primaire Composite  

L’exemple suivant configure les propriétés DepartmentID et le nom à la clé primaire composite du type de service.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>En désactivant les identités pour les clés primaires numérique  

L’exemple suivant définit la propriété DepartmentID à System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None pour indiquer que la valeur ne sera pas générée par la base de données.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Spécifiant la longueur maximale sur une propriété  

Dans l’exemple suivant, la propriété Name doit être non plue de 50 caractères. Si vous apportez à la valeur de plus de 50 caractères, vous obtiendrez un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception. Si le Code First crée une base de données à partir de ce modèle il définit également la longueur maximale de la colonne de nom à 50 caractères.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Configuration de la propriété comme étant obligatoire  

Dans l’exemple suivant, la propriété Name est requise. Si vous ne spécifiez pas le nom, vous obtiendrez une exception DbEntityValidationException. Si le Code First crée une base de données à partir de ce modèle puis la colonne utilisée pour stocker cette propriété seront généralement non nullable.  

> [!NOTE]
> Dans certains cas il ne peut pas être possible pour la colonne dans la base de données soit non nullable même si la propriété est requise. Par exemple, quand à l’aide de données de stratégie de l’héritage TPH pour plusieurs types est stockée dans une table unique. Si un type dérivé inclut une propriété obligatoire, que la colonne ne peut pas être rendue non nullable comme pas tous les types dans la hiérarchie ont cette propriété.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Configuration d’un Index sur une ou plusieurs propriétés  

> [!NOTE]
> **EF6.1 et versions ultérieures uniquement** -attribut de l’Index a été introduite dans Entity Framework 6.1. Si vous utilisez une version antérieure les informations contenues dans cette section ne s’applique pas.  

Création d’index n’est pas pris en charge en mode natif par l’API Fluent, mais vous pouvez faire utiliser la prise en charge pour **IndexAttribute** via l’API Fluent. Attributs d’index sont traités en incluant une annotation de modèle sur le modèle qui est ensuite activée dans un Index dans la base de données plus loin dans le pipeline. Vous pouvez ajouter manuellement ces mêmes annotations à l’aide de l’API Fluent.  

Le moyen le plus simple pour ce faire consiste à créer une instance de **IndexAttribute** qui contient tous les paramètres pour le nouvel index. Vous pouvez ensuite créer une instance de **IndexAnnotation** qui est un type spécifique d’EF qui convertira le **IndexAttribute** paramètres dans une annotation de modèle qui peut être stocké sur le modèle EF. Il peuvent ensuite être transmis à la **HasColumnAnnotation** méthode sur l’API Fluent, en spécifiant le nom **Index** pour l’annotation.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Pour obtenir la liste complète des paramètres disponibles dans **IndexAttribute**, consultez le *Index* section de [Annotations de données Code First](~/ef6/modeling/code-first/data-annotations.md). Cela inclut la personnalisation du nom de l’index, la création d’index uniques et la création des index multi-colonnes.  

Vous pouvez spécifier plusieurs annotations d’index sur une propriété unique en passant un tableau de **IndexAttribute** au constructeur de **IndexAnnotation**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Spécification afin de ne pas mapper une propriété CLR à une colonne dans la base de données  

L’exemple suivant montre comment spécifier qu’une propriété sur un type CLR n’est pas mappée à une colonne dans la base de données.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mappage d’une propriété CLR à une colonne spécifique dans la base de données  

L’exemple suivant mappe la propriété nom CLR à la colonne de base de données DepartmentName.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Modification du nom d’une clé étrangère qui n’est pas définie dans le modèle  

Si vous choisissez de ne pas définir une clé étrangère sur un type CLR, mais souhaitez spécifier quel nom, il doit avoir dans la base de données, procédez comme suit :  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Configuration si une propriété de chaîne prend en charge le contenu Unicode  

Par défaut, les chaînes sont Unicode (nvarchar dans SQL Server). Vous pouvez utiliser la méthode IsUnicode pour spécifier qu’une chaîne doit être de type varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Configuration du Type de données d’une colonne de base de données  

Le [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) méthode active le mappage aux différentes représentations du même type de base. À l’aide de cette méthode ne vous autorise pas à effectuer une conversion des données en cours d’exécution. Notez que IsUnicode est la meilleure façon de définition de colonnes varchar, car il est indépendant de la base de données.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Configurer les propriétés d’un Type complexe  

Il existe deux façons de configurer des propriétés scalaires sur un type complexe.  

Vous pouvez appeler la propriété sur ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Vous pouvez également utiliser la notation par points pour accéder à une propriété d’un type complexe.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Configuration d’une propriété à utiliser comme un jeton d’accès concurrentiel optimiste  

Pour spécifier qu’une propriété dans une entité représente un jeton d’accès concurrentiel, vous pouvez utiliser l’attribut ConcurrencyCheck ou la méthode IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Vous pouvez également utiliser la méthode IsRowVersion pour configurer la propriété à une version de ligne dans la base de données. Définition de la propriété soit qu'une version de ligne configure automatiquement pour qu’il soit un jeton d’accès concurrentiel optimiste.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mappage de type  

### <a name="specifying-that-a-class-is-a-complex-type"></a>En spécifiant qu’une classe est un Type complexe  

Par convention, un type qui ne possède aucune clé primaire spécifié est traité comme un type complexe. Il existe certains scénarios où Code First ne détecte pas un type complexe (par exemple, si vous n’avez pas une propriété appelée ID, mais vous ne signifient pas qu’elle soit une clé primaire). Dans ce cas, vous utiliseriez l’API fluent pour spécifier explicitement qu’un type est un type complexe.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Spécification afin de ne pas mapper un Type d’entité CLR à une Table dans la base de données  

L’exemple suivant montre comment exclure un type CLR de qui est mappé à une table dans la base de données.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mappage d’un Type d’entité à une Table spécifique dans la base de données  

Toutes les propriétés du service seront mappées aux colonnes dans une table appelée m_ département.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Vous pouvez également spécifier le nom de schéma comme suit :  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mappage de l’héritage Table par hiérarchie (TPH)  

Dans le scénario de mappage TPH, tous les types dans une hiérarchie d’héritage sont mappés à une seule table. Une colonne de discriminateur est utilisée pour identifier le type de chaque ligne. Lorsque vous créez votre modèle avec Code First, TPH est la stratégie par défaut pour les types qui participent à la hiérarchie d’héritage. Par défaut, la colonne de discriminateur est ajoutée à la table avec le nom « Discriminator » et le nom de type CLR de chaque type dans la hiérarchie est utilisé pour les valeurs de discriminateur. Vous pouvez modifier le comportement par défaut à l’aide de l’API fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mappage de l’héritage Table par Type (TPT)  

Dans le scénario de mappage TPT, tous les types sont mappés à des tables individuelles. Les propriétés qui appartiennent uniquement à un type de base ou à un type dérivé sont stockées dans une table qui mappe à ce type. Les tables qui mappent aux types dérivés stockent également une clé étrangère qui joint la table dérivée avec la table de base.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mappage de l’héritage Table-par classe concrète (TPC)  

Dans le scénario de mappage TPC, tous les types non abstraits dans la hiérarchie sont mappés à des tables individuelles. Les tables qui mappent aux classes dérivées n’ont aucune relation à la table qui mappe à la classe de base dans la base de données. Toutes les propriétés d’une classe, y compris les propriétés héritées, sont mappées aux colonnes de la table correspondante.  

Appelez la méthode MapInheritedProperties pour configurer chaque type dérivé. MapInheritedProperties remappe toutes les propriétés héritées de la classe de base à de nouvelles colonnes dans la table pour la classe dérivée.  

> [!NOTE]
> Notez que, étant donné que les tables impliquées dans la hiérarchie d’héritage TPC ne partagent pas une clé primaire il sera clés d’entité en double, lors de l’insertion dans des tables qui sont mappés aux sous-classes si vous avez des valeurs de base de données générée avec la même valeur de départ d’identité. Pour résoudre ce problème, vous pouvez spécifier une valeur de départ initiale différente pour chaque table ou désactive l’identité sur la propriété de clé primaire. L’identité est la valeur par défaut pour les propriétés de clé entière lorsque vous travaillez avec Code First.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mappage des propriétés d’un Type d’entité à plusieurs Tables dans la base de données (fractionnement d’entité)  

Fractionnement d’entité permet aux propriétés d’un type d’entité d’être réparties entre plusieurs tables. Dans l’exemple suivant, l’entité Department est divisée en deux tables : département et DepartmentDetails. Fractionnement d’entité utilise plusieurs appels à la méthode de mappage pour mapper un sous-ensemble de propriétés à une table spécifique.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mappage de plusieurs Types d’entités à une Table dans la base de données (fractionnement de Table)  

L’exemple suivant mappe les deux types d’entités qui partagent une clé primaire pour une table.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mappage d’un Type d’entité aux procédures stockées Insert/Update/Delete (Entity Framework 6 et versions ultérieures)  

En commençant par EF6, vous pouvez mapper une entité pour utiliser des procédures stockées pour insérer mettre à jour et supprimer. Pour plus d’informations, consultez [Code première Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
