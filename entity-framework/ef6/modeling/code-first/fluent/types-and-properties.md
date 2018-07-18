---
title: API Fluent - configuration et mappage des Types et propriétés - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
caps.latest.revision: 3
ms.openlocfilehash: ec8b484433d13899a88f44e37823dd1a4bed6530
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121368"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="8ef72-102">API Fluent - configuration et de mappage des Types et des propriétés</span><span class="sxs-lookup"><span data-stu-id="8ef72-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="8ef72-103">Lorsque vous travaillez avec Entity Framework Code First le comportement par défaut consiste à mapper vos classes POCO à des tables à l’aide d’un ensemble de conventions intégrées à EF.</span><span class="sxs-lookup"><span data-stu-id="8ef72-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="8ef72-104">Parfois, cependant, vous ne peut pas ou ne souhaitez pas suivre ces conventions et devez mapper des entités sur autre chose que ce que les conventions de dictent.</span><span class="sxs-lookup"><span data-stu-id="8ef72-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="8ef72-105">Il existe deux façons principales, vous pouvez configurer EF pour utiliser une valeur autre que de conventions, à savoir [annotations](~/ef6/modeling/code-first/data-annotations.md) ou l’API fluent EFs.</span><span class="sxs-lookup"><span data-stu-id="8ef72-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="8ef72-106">Les annotations couvrent uniquement un sous-ensemble des fonctionnalités de l’API fluent, donc il existe des scénarios de mappage qui ne peut pas être obtenus à l’aide d’annotations.</span><span class="sxs-lookup"><span data-stu-id="8ef72-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="8ef72-107">Cet article est conçu pour montrer comment utiliser l’API fluent pour configurer les propriétés.</span><span class="sxs-lookup"><span data-stu-id="8ef72-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="8ef72-108">L’API fluent code first est généralement accessible en substituant le [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) méthode sur votre dérivée [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ef72-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="8ef72-109">Les exemples suivants sont conçus pour montrer comment effectuer diverses tâches avec l’api fluent et vous permettent de copier le code et le personnaliser pour l’adapter à votre modèle, si vous souhaitez voir le modèle qui ils peuvent être utilisés avec comme-est alors il est fourni à la fin de cet article.</span><span class="sxs-lookup"><span data-stu-id="8ef72-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="8ef72-110">Paramètres de modèle à l’échelle</span><span class="sxs-lookup"><span data-stu-id="8ef72-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="8ef72-111">Schéma par défaut (Entity Framework 6 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="8ef72-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="8ef72-112">En commençant par EF6, vous pouvez utiliser la méthode HasDefaultSchema sur DbModelBuilder pour spécifier le schéma de base de données à utiliser pour toutes les tables, procédures stockées, etc. Ce paramètre par défaut est remplacé pour tous les objets que vous configurez explicitement un schéma différent pour.</span><span class="sxs-lookup"><span data-stu-id="8ef72-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="8ef72-113">Conventions personnalisées (Entity Framework 6 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="8ef72-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="8ef72-114">Démarrage avec EF6, vous pouvez créer vos propres conventions en supplément de ceux inclus dans le Code First.</span><span class="sxs-lookup"><span data-stu-id="8ef72-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="8ef72-115">Pour plus d’informations, consultez [Conventions Code First personnalisées](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="8ef72-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="8ef72-116">Mappage de propriétés</span><span class="sxs-lookup"><span data-stu-id="8ef72-116">Property Mapping</span></span>  

<span data-ttu-id="8ef72-117">Le [propriété](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) méthode est utilisée pour configurer des attributs pour chaque propriété appartenant à une entité ou un type complexe.</span><span class="sxs-lookup"><span data-stu-id="8ef72-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="8ef72-118">La méthode de propriété est utilisée pour obtenir un objet de configuration pour une propriété donnée.</span><span class="sxs-lookup"><span data-stu-id="8ef72-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="8ef72-119">Les options de l’objet de configuration sont spécifiques au type en cours de configuration ; IsUnicode est disponible uniquement sur les propriétés de chaîne par exemple.</span><span class="sxs-lookup"><span data-stu-id="8ef72-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="8ef72-120">Configuration d’une clé primaire</span><span class="sxs-lookup"><span data-stu-id="8ef72-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="8ef72-121">La convention d’Entity Framework pour les clés primaires est :</span><span class="sxs-lookup"><span data-stu-id="8ef72-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="8ef72-122">Votre classe définit une propriété dont le nom est « ID » ou « Id »</span><span class="sxs-lookup"><span data-stu-id="8ef72-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="8ef72-123">ou un nom de classe suivie de « ID » ou « Id »</span><span class="sxs-lookup"><span data-stu-id="8ef72-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="8ef72-124">Pour définir explicitement une propriété soit une clé primaire, vous pouvez utiliser la méthode HasKey.</span><span class="sxs-lookup"><span data-stu-id="8ef72-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="8ef72-125">Dans l’exemple suivant, la méthode HasKey permet de configurer la clé primaire InstructorID sur le type OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="8ef72-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="8ef72-126">Configuration d’une clé primaire Composite</span><span class="sxs-lookup"><span data-stu-id="8ef72-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="8ef72-127">L’exemple suivant configure les propriétés DepartmentID et le nom à la clé primaire composite du type de service.</span><span class="sxs-lookup"><span data-stu-id="8ef72-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="8ef72-128">En désactivant les identités pour les clés primaires numérique</span><span class="sxs-lookup"><span data-stu-id="8ef72-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="8ef72-129">L’exemple suivant définit la propriété DepartmentID à System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None pour indiquer que la valeur ne sera pas générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="8ef72-130">Spécifiant la longueur maximale sur une propriété</span><span class="sxs-lookup"><span data-stu-id="8ef72-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="8ef72-131">Dans l’exemple suivant, la propriété Name doit être non plue de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="8ef72-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="8ef72-132">Si vous apportez à la valeur de plus de 50 caractères, vous obtiendrez un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span><span class="sxs-lookup"><span data-stu-id="8ef72-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="8ef72-133">Si le Code First crée une base de données à partir de ce modèle il définit également la longueur maximale de la colonne de nom à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="8ef72-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="8ef72-134">Configuration de la propriété comme étant obligatoire</span><span class="sxs-lookup"><span data-stu-id="8ef72-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="8ef72-135">Dans l’exemple suivant, la propriété Name est requise.</span><span class="sxs-lookup"><span data-stu-id="8ef72-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="8ef72-136">Si vous ne spécifiez pas le nom, vous obtiendrez une exception DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="8ef72-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="8ef72-137">Si le Code First crée une base de données à partir de ce modèle puis la colonne utilisée pour stocker cette propriété seront généralement non nullable.</span><span class="sxs-lookup"><span data-stu-id="8ef72-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ef72-138">Dans certains cas il ne peut pas être possible pour la colonne dans la base de données soit non nullable même si la propriété est requise.</span><span class="sxs-lookup"><span data-stu-id="8ef72-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="8ef72-139">Par exemple, quand à l’aide de données de stratégie de l’héritage TPH pour plusieurs types est stockée dans une table unique.</span><span class="sxs-lookup"><span data-stu-id="8ef72-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="8ef72-140">Si un type dérivé inclut une propriété obligatoire, que la colonne ne peut pas être rendue non nullable comme pas tous les types dans la hiérarchie ont cette propriété.</span><span class="sxs-lookup"><span data-stu-id="8ef72-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="8ef72-141">Configuration d’un Index sur une ou plusieurs propriétés</span><span class="sxs-lookup"><span data-stu-id="8ef72-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="8ef72-142">**EF6.1 et versions ultérieures uniquement** -attribut de l’Index a été introduite dans Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="8ef72-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="8ef72-143">Si vous utilisez une version antérieure les informations contenues dans cette section ne s’applique pas.</span><span class="sxs-lookup"><span data-stu-id="8ef72-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="8ef72-144">Création d’index n’est pas pris en charge en mode natif par l’API Fluent, mais vous pouvez faire utiliser la prise en charge pour **IndexAttribute** via l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8ef72-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="8ef72-145">Attributs d’index sont traités en incluant une annotation de modèle sur le modèle qui est ensuite activée dans un Index dans la base de données plus loin dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="8ef72-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="8ef72-146">Vous pouvez ajouter manuellement ces mêmes annotations à l’aide de l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8ef72-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="8ef72-147">Le moyen le plus simple pour ce faire consiste à créer une instance de **IndexAttribute** qui contient tous les paramètres pour le nouvel index.</span><span class="sxs-lookup"><span data-stu-id="8ef72-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="8ef72-148">Vous pouvez ensuite créer une instance de **IndexAnnotation** qui est un type spécifique d’EF qui convertira le **IndexAttribute** paramètres dans une annotation de modèle qui peut être stocké sur le modèle EF.</span><span class="sxs-lookup"><span data-stu-id="8ef72-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="8ef72-149">Il peuvent ensuite être transmis à la **HasColumnAnnotation** méthode sur l’API Fluent, en spécifiant le nom **Index** pour l’annotation.</span><span class="sxs-lookup"><span data-stu-id="8ef72-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="8ef72-150">Pour obtenir la liste complète des paramètres disponibles dans **IndexAttribute**, consultez le *Index* section de [Annotations de données Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="8ef72-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="8ef72-151">Cela inclut la personnalisation du nom de l’index, la création d’index uniques et la création des index multi-colonnes.</span><span class="sxs-lookup"><span data-stu-id="8ef72-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="8ef72-152">Vous pouvez spécifier plusieurs annotations d’index sur une propriété unique en passant un tableau de **IndexAttribute** au constructeur de **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="8ef72-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="8ef72-153">Spécification afin de ne pas mapper une propriété CLR à une colonne dans la base de données</span><span class="sxs-lookup"><span data-stu-id="8ef72-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="8ef72-154">L’exemple suivant montre comment spécifier qu’une propriété sur un type CLR n’est pas mappée à une colonne dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="8ef72-155">Mappage d’une propriété CLR à une colonne spécifique dans la base de données</span><span class="sxs-lookup"><span data-stu-id="8ef72-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="8ef72-156">L’exemple suivant mappe la propriété nom CLR à la colonne de base de données DepartmentName.</span><span class="sxs-lookup"><span data-stu-id="8ef72-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="8ef72-157">Modification du nom d’une clé étrangère qui n’est pas définie dans le modèle</span><span class="sxs-lookup"><span data-stu-id="8ef72-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="8ef72-158">Si vous choisissez de ne pas définir une clé étrangère sur un type CLR, mais souhaitez spécifier quel nom, il doit avoir dans la base de données, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="8ef72-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="8ef72-159">Configuration si une propriété de chaîne prend en charge le contenu Unicode</span><span class="sxs-lookup"><span data-stu-id="8ef72-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="8ef72-160">Par défaut, les chaînes sont Unicode (nvarchar dans SQL Server).</span><span class="sxs-lookup"><span data-stu-id="8ef72-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="8ef72-161">Vous pouvez utiliser la méthode IsUnicode pour spécifier qu’une chaîne doit être de type varchar.</span><span class="sxs-lookup"><span data-stu-id="8ef72-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="8ef72-162">Configuration du Type de données d’une colonne de base de données</span><span class="sxs-lookup"><span data-stu-id="8ef72-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="8ef72-163">Le [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) méthode active le mappage aux différentes représentations du même type de base.</span><span class="sxs-lookup"><span data-stu-id="8ef72-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="8ef72-164">À l’aide de cette méthode ne vous autorise pas à effectuer une conversion des données en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="8ef72-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="8ef72-165">Notez que IsUnicode est la meilleure façon de définition de colonnes varchar, car il est indépendant de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="8ef72-166">Configurer les propriétés d’un Type complexe</span><span class="sxs-lookup"><span data-stu-id="8ef72-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="8ef72-167">Il existe deux façons de configurer des propriétés scalaires sur un type complexe.</span><span class="sxs-lookup"><span data-stu-id="8ef72-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="8ef72-168">Vous pouvez appeler la propriété sur ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="8ef72-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="8ef72-169">Vous pouvez également utiliser la notation par points pour accéder à une propriété d’un type complexe.</span><span class="sxs-lookup"><span data-stu-id="8ef72-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="8ef72-170">Configuration d’une propriété à utiliser comme un jeton d’accès concurrentiel optimiste</span><span class="sxs-lookup"><span data-stu-id="8ef72-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="8ef72-171">Pour spécifier qu’une propriété dans une entité représente un jeton d’accès concurrentiel, vous pouvez utiliser l’attribut ConcurrencyCheck ou la méthode IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="8ef72-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="8ef72-172">Vous pouvez également utiliser la méthode IsRowVersion pour configurer la propriété à une version de ligne dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="8ef72-173">Définition de la propriété soit qu'une version de ligne configure automatiquement pour qu’il soit un jeton d’accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="8ef72-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="8ef72-174">Mappage de type</span><span class="sxs-lookup"><span data-stu-id="8ef72-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="8ef72-175">En spécifiant qu’une classe est un Type complexe</span><span class="sxs-lookup"><span data-stu-id="8ef72-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="8ef72-176">Par convention, un type qui ne possède aucune clé primaire spécifié est traité comme un type complexe.</span><span class="sxs-lookup"><span data-stu-id="8ef72-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="8ef72-177">Il existe certains scénarios où Code First ne détecte pas un type complexe (par exemple, si vous n’avez pas une propriété appelée ID, mais vous ne signifient pas qu’elle soit une clé primaire).</span><span class="sxs-lookup"><span data-stu-id="8ef72-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="8ef72-178">Dans ce cas, vous utiliseriez l’API fluent pour spécifier explicitement qu’un type est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="8ef72-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="8ef72-179">Spécification afin de ne pas mapper un Type d’entité CLR à une Table dans la base de données</span><span class="sxs-lookup"><span data-stu-id="8ef72-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="8ef72-180">L’exemple suivant montre comment exclure un type CLR de qui est mappé à une table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="8ef72-181">Mappage d’un Type d’entité à une Table spécifique dans la base de données</span><span class="sxs-lookup"><span data-stu-id="8ef72-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="8ef72-182">Toutes les propriétés du service seront mappées aux colonnes dans une table appelée m_ département.</span><span class="sxs-lookup"><span data-stu-id="8ef72-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="8ef72-183">Vous pouvez également spécifier le nom de schéma comme suit :</span><span class="sxs-lookup"><span data-stu-id="8ef72-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="8ef72-184">Mappage de l’héritage Table par hiérarchie (TPH)</span><span class="sxs-lookup"><span data-stu-id="8ef72-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="8ef72-185">Dans le scénario de mappage TPH, tous les types dans une hiérarchie d’héritage sont mappés à une seule table.</span><span class="sxs-lookup"><span data-stu-id="8ef72-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="8ef72-186">Une colonne de discriminateur est utilisée pour identifier le type de chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="8ef72-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="8ef72-187">Lorsque vous créez votre modèle avec Code First, TPH est la stratégie par défaut pour les types qui participent à la hiérarchie d’héritage.</span><span class="sxs-lookup"><span data-stu-id="8ef72-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="8ef72-188">Par défaut, la colonne de discriminateur est ajoutée à la table avec le nom « Discriminator » et le nom de type CLR de chaque type dans la hiérarchie est utilisé pour les valeurs de discriminateur.</span><span class="sxs-lookup"><span data-stu-id="8ef72-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="8ef72-189">Vous pouvez modifier le comportement par défaut à l’aide de l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="8ef72-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="8ef72-190">Mappage de l’héritage Table par Type (TPT)</span><span class="sxs-lookup"><span data-stu-id="8ef72-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="8ef72-191">Dans le scénario de mappage TPT, tous les types sont mappés à des tables individuelles.</span><span class="sxs-lookup"><span data-stu-id="8ef72-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="8ef72-192">Les propriétés qui appartiennent uniquement à un type de base ou à un type dérivé sont stockées dans une table qui mappe à ce type.</span><span class="sxs-lookup"><span data-stu-id="8ef72-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="8ef72-193">Les tables qui mappent aux types dérivés stockent également une clé étrangère qui joint la table dérivée avec la table de base.</span><span class="sxs-lookup"><span data-stu-id="8ef72-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="8ef72-194">Mappage de l’héritage Table-par classe concrète (TPC)</span><span class="sxs-lookup"><span data-stu-id="8ef72-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="8ef72-195">Dans le scénario de mappage TPC, tous les types non abstraits dans la hiérarchie sont mappés à des tables individuelles.</span><span class="sxs-lookup"><span data-stu-id="8ef72-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="8ef72-196">Les tables qui mappent aux classes dérivées n’ont aucune relation à la table qui mappe à la classe de base dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8ef72-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="8ef72-197">Toutes les propriétés d’une classe, y compris les propriétés héritées, sont mappées aux colonnes de la table correspondante.</span><span class="sxs-lookup"><span data-stu-id="8ef72-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="8ef72-198">Appelez la méthode MapInheritedProperties pour configurer chaque type dérivé.</span><span class="sxs-lookup"><span data-stu-id="8ef72-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="8ef72-199">MapInheritedProperties remappe toutes les propriétés héritées de la classe de base à de nouvelles colonnes dans la table pour la classe dérivée.</span><span class="sxs-lookup"><span data-stu-id="8ef72-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="8ef72-200">Notez que, étant donné que les tables impliquées dans la hiérarchie d’héritage TPC ne partagent pas une clé primaire il sera clés d’entité en double, lors de l’insertion dans des tables qui sont mappés aux sous-classes si vous avez des valeurs de base de données générée avec la même valeur de départ d’identité.</span><span class="sxs-lookup"><span data-stu-id="8ef72-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="8ef72-201">Pour résoudre ce problème, vous pouvez spécifier une valeur de départ initiale différente pour chaque table ou désactive l’identité sur la propriété de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="8ef72-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="8ef72-202">L’identité est la valeur par défaut pour les propriétés de clé entière lorsque vous travaillez avec Code First.</span><span class="sxs-lookup"><span data-stu-id="8ef72-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="8ef72-203">Mappage des propriétés d’un Type d’entité à plusieurs Tables dans la base de données (fractionnement d’entité)</span><span class="sxs-lookup"><span data-stu-id="8ef72-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="8ef72-204">Fractionnement d’entité permet aux propriétés d’un type d’entité d’être réparties entre plusieurs tables.</span><span class="sxs-lookup"><span data-stu-id="8ef72-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="8ef72-205">Dans l’exemple suivant, l’entité Department est divisée en deux tables : département et DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="8ef72-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="8ef72-206">Fractionnement d’entité utilise plusieurs appels à la méthode de mappage pour mapper un sous-ensemble de propriétés à une table spécifique.</span><span class="sxs-lookup"><span data-stu-id="8ef72-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="8ef72-207">Mappage de plusieurs Types d’entités à une Table dans la base de données (fractionnement de Table)</span><span class="sxs-lookup"><span data-stu-id="8ef72-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="8ef72-208">L’exemple suivant mappe les deux types d’entités qui partagent une clé primaire pour une table.</span><span class="sxs-lookup"><span data-stu-id="8ef72-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="8ef72-209">Mappage d’un Type d’entité aux procédures stockées Insert/Update/Delete (Entity Framework 6 et versions ultérieures)</span><span class="sxs-lookup"><span data-stu-id="8ef72-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="8ef72-210">En commençant par EF6, vous pouvez mapper une entité pour utiliser des procédures stockées pour insérer mettre à jour et supprimer.</span><span class="sxs-lookup"><span data-stu-id="8ef72-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="8ef72-211">Pour plus d’informations, consultez [Code première Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="8ef72-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="8ef72-212">Modèle utilisé dans les exemples</span><span class="sxs-lookup"><span data-stu-id="8ef72-212">Model Used in Samples</span></span>  

<span data-ttu-id="8ef72-213">Le modèle Code First suivant est utilisé pour les exemples de cette page.</span><span class="sxs-lookup"><span data-stu-id="8ef72-213">The following Code First model is used for the samples on this page.</span></span>  

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
