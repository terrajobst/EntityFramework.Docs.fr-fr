---
title: Relations, les propriétés de navigation et les clés étrangères - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 28dab25c7d19a117e594b1761f7bc745b684f7b3
ms.sourcegitcommit: 5c2634c546720902fe01935f4fc031d73aa3ebde
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393761"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relations, les propriétés de navigation et les clés étrangères
Cette rubrique donne une vue d’ensemble de la façon dont Entity Framework gère les relations entre entités. Il fournit également des conseils sur la façon de mapper et de manipuler des relations.

## <a name="relationships-in-ef"></a>Relations dans EF

Dans les bases de données relationnelles, les relations (également appelées associations) entre les tables sont définies via les clés étrangères. Une clé étrangère (FK) est une colonne ou une combinaison de colonnes est utilisé pour établir et conserver un lien entre les données de deux tables. Il existe généralement trois types de relations :-à-un, un-à-plusieurs et plusieurs-à-plusieurs. Dans une relation un-à-plusieurs, la clé étrangère est définie sur la table qui représente la fin de nombreux de la relation. La relation plusieurs-à-plusieurs implique la définition d’une troisième table (appelée une table de jonction ou de jointure), dont la clé primaire est composée des clés étrangères des deux tables connexes. Dans une relation un à un, la clé primaire se comporte en outre, comme une clé étrangère, et il n’existe aucune colonne de clé étrangère distinct pour des tables.

L’illustration suivante montre les deux tables impliquées dans une relation un-à-plusieurs. Le **cours** table est la table dépendante, car elle contient le **DepartmentID** colonne qui lie à la **département** table.

![Database2](~/ef6/media/database2.png)

Dans Entity Framework, une entité peut être associée à d’autres entités via une association ou de la relation. Chaque relation comporte deux terminaisons qui décrivent le type d’entité et la multiplicité du type (un, zéro-ou-un ou plusieurs) pour les deux entités dans cette relation. La relation peut-être être régie par une contrainte référentielle, qui décrit quelle fin dans la relation est un rôle principal et qui est un rôle dépendant.

Propriétés de navigation permettent de parcourir une association entre deux types d’entité. Chaque objet peut avoir une propriété de navigation pour chaque relation à laquelle il participe. Propriétés de navigation permettent de parcourir et de gérer les relations dans les deux directions, en retournant un objet de la référence (si la multiplicité est soit un ou zéro-ou-un) ou une collection (si la multiplicité est nombreuses). Vous pouvez également choisir d’avoir une navigation unidirectionnelle, auquel cas vous définissez la propriété de navigation sur un seul des types qui participe à la relation et non sur les deux.

Il est recommandé d’inclure des propriétés dans le modèle correspondant aux clés étrangères dans la base de données. Lorsque les propriétés de clé étrangère sont incluses, vous pouvez créer ou modifier une relation en changeant la valeur de clé étrangère d'un objet dépendant. Ce type d'association s'appelle une association de clé étrangère. À l’aide de clés étrangères est encore plus indispensable lorsque vous travaillez avec des entités déconnectées. Notez que quand utilisation 1-à-1 ou 1-0... relations 1, il n’existe aucune colonne de clé étrangère distinct, agit comme la clé étrangère de la propriété de clé primaire et est toujours incluse dans le modèle.

Lorsque les colonnes clés étrangères ne sont pas inclus dans le modèle, les informations d’association sont gérées comme un objet indépendant. Les relations sont suivies par le biais des références d’objet au lieu de propriétés de clé étrangère. Ce type d’association est appelé un *association indépendante*. La méthode la plus courante pour modifier un *association indépendante* consiste à modifier les propriétés de navigation sont générées pour chaque entité qui participe à l’association.

Vous pouvez choisir d'utiliser un type d'association dans votre modèle ou les deux à la fois. Toutefois, si vous avez une relation plusieurs-à-plusieurs pure qui est connectée par une table de jointure qui contient uniquement des clés étrangères, EF utilisera une association indépendante pour gérer cette relation plusieurs-à-plusieurs.   

L’illustration suivante montre un modèle conceptuel qui a été créé avec l’Entity Framework Designer. Le modèle contient deux entités qui participent de relation un-à-plusieurs. Les deux entités ont des propriétés de navigation. **Cours** est l’entité depend et a la **DepartmentID** propriété de clé étrangère définie.

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

L’extrait de code suivant montre le même modèle que celui qui a été créé avec Code First.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Configuration ou le mappage des relations

Le reste de cette page décrit comment accéder à et manipuler des données à l’aide de relations. Pour plus d’informations sur la configuration des relations dans votre modèle, consultez les pages suivantes.

-   Pour configurer des relations dans le Code First, voir [Annotations de données](~/ef6/modeling/code-first/data-annotations.md) et [API Fluent – relations](~/ef6/modeling/code-first/fluent/relationships.md).
-   Pour configurer des relations à l’aide d’Entity Framework Designer, consultez [relations avec le Concepteur EF](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Création et modification de relations

Dans un *association de clé étrangère*, lorsque vous modifiez la relation, l’état d’un objet dépendant avec une EntityState.Unchanged l’état passe à EntityState.Modified. Dans une relation indépendante, modification de la relation ne pas mettre à jour l’état de l’objet dépendant.

Les exemples suivants montrent comment utiliser les propriétés de clé étrangère et les propriétés de navigation pour associer les objets connexes. Avec les associations de clé étrangère, vous pouvez utiliser soit la méthode à modifier, créer ou modifier des relations. Avec les associations indépendantes, vous ne pouvez pas utiliser la propriété de clé étrangère.

-   En assignant une nouvelle valeur à une propriété de clé étrangère, comme dans l’exemple suivant.  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   Le code suivant supprime une relation en définissant la clé étrangère sur **null**. Notez que la propriété de clé étrangère doit être nullable.  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > Si la référence est dans l’état ajouté (dans cet exemple, l’objet de cours), la propriété de navigation de référence ne sera pas synchronisée avec les valeurs de clé d’un nouvel objet jusqu'à ce que SaveChanges est appelée. La synchronisation n'a pas lieu, car le contexte de l'objet ne contient pas de clés permanentes pour les objets ajoutés tant qu'ils ne sont pas enregistrés. Si vous devez disposer des nouveaux objets entièrement synchronisés dès que vous définissez la relation, utilisez une du methods.* suivant

-   En affectant un nouvel objet à une propriété de navigation. Le code suivant crée une relation entre un cours et un `department`. Si les objets sont attachés au contexte, le `course` est également ajouté à la `department.Courses` collection et la clé étrangère propriété sur le `course` objet est défini sur la valeur de propriété de clé du service.  
    ``` csharp
    course.Department = department;
    ```

 -   Pour supprimer la relation, définissez la propriété de navigation sur `null`. Si vous travaillez avec Entity Framework qui repose sur .NET 4.0, la terminaison connexe doit être chargé avant que vous le définissez sur null. Exemple :  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    En commençant par Entity Framework 5.0, qui est basé sur .NET 4.5, vous pouvez définir la relation NULL sans charger de la terminaison connexe. Vous pouvez également définir la valeur actuelle sur null à l’aide de la méthode suivante.  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   En supprimant ou en ajoutant un objet dans une collection d'entités. Par exemple, vous pouvez ajouter un objet de type `Course` à la `department.Courses` collection. Cette opération crée une relation entre un particulier **cours** et un particulier `department`. Si les objets sont attachés pour le contexte, la référence de service et la propriété de clé étrangère sur la **cours** objet définira appropriés `department`.  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- À l’aide de la `ChangeRelationshipState` méthode pour modifier l’état de la relation entre deux objets d’entité spécifiée. Cette méthode est couramment utilisée lorsque vous travaillez avec des applications multicouches et un *association indépendante* (il ne peut pas être utilisé avec une association de clé étrangère). En outre, pour utiliser cette méthode, vous devez supprimer jusqu'à `ObjectContext`, comme illustré dans l’exemple ci-dessous.  
Dans l’exemple suivant, il existe une relation plusieurs-à-plusieurs entre formateurs et cours. Appelant le `ChangeRelationshipState` (méthode) et en passant le `EntityState.Added` vous permet de paramètre, le `SchoolContext` savoir qu’une relation a été ajoutée entre les deux objets :

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Synchronisation des modifications entre les clés étrangères et les propriétés de navigation

Lorsque vous modifiez la relation des objets attachés au contexte en utilisant l’une des méthodes décrites ci-dessus, Entity Framework a besoin synchroniser les clés étrangères, des références et des collections. Entity Framework gère automatiquement cette synchronisation (également appelé relation correctives) pour les entités POCO avec proxys. Pour plus d’informations, consultez [utilisation de proxys](~/ef6/fundamentals/proxies.md).

Si vous utilisez des entités POCO sans proxys, vous devez vous assurer que le **DetectChanges** méthode est appelée pour synchroniser les objets connexes dans le contexte. Notez que les API suivantes déclenchent automatiquement un **DetectChanges** appeler.

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   Exécution d’un LINQ de la requête par rapport à un `DbSet`

## <a name="loading-related-objects"></a>Charger des objets connexes

Dans Entity Framework, que vous utilisez le plus souvent utiliser les propriétés de navigation pour charger des entités qui sont liées à l’entité retournée par l’association définie. Pour plus d’informations, consultez [le chargement des objets connexes](~/ef6/querying/related-data.md).

> [!NOTE]
> Dans une association de clé étrangère, lorsque vous chargez une terminaison connexe d'un objet dépendant, l'objet connexe est chargé en fonction de la valeur de clé étrangère du dépendant actuellement en mémoire :

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

Dans une association indépendante, la terminaison connexe d'un objet dépendant est interrogée en fonction de la valeur de clé étrangère actuellement présente dans la base de données. Toutefois, si la relation a été modifiée, et la propriété de référence sur l’objet dépendant pointe vers un autre objet principal qui est chargé dans le contexte de l’objet, Entity Framework tente de créer une relation en tant qu’il est défini sur le client.

## <a name="managing-concurrency"></a>Gestion de l'accès concurrentiel

Dans la clé étrangère et associations indépendantes, les contrôles d’accès concurrentiel sont basées sur les clés d’entité et d’autres propriétés d’entité qui sont définies dans le modèle. Lorsque vous utilisez le Concepteur EF pour créer un modèle, définissez le `ConcurrencyMode` attribut **fixe** pour spécifier que la propriété doit être vérifiée pour l’accès concurrentiel. Lorsque vous utilisez Code First pour définir un modèle, utilisez le `ConcurrencyCheck` annotation sur les propriétés que vous souhaitez être vérifiée pour l’accès concurrentiel. Lorsque vous travaillez avec Code First, vous pouvez également utiliser le `TimeStamp` annotation pour spécifier que la propriété doit être vérifiée pour l’accès concurrentiel. Vous pouvez avoir qu’une seule propriété d’horodatage dans une classe donnée. Tout d’abord les cartes de code cette propriété à un champ non nullable dans la base de données.

Nous recommandons de toujours utiliser l’association de clé étrangère lorsque vous travaillez avec des entités qui participent à la résolution et de contrôle d’accès concurrentiel.

Pour plus d’informations, consultez [gère les conflits d’accès concurrentiel](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Utilisation de clés qui se chevauchent

Des clés qui se chevauchent sont des clés composites qui ont certaines propriétés en commun dans l'entité qu'elles constituent. Une association indépendante ne peut pas contenir de clés qui se chevauchent. Pour modifier une association de clé étrangère qui comporte des clés qui se chevauchent, nous vous conseillons de modifier les valeurs de clé étrangère plutôt que d'utiliser les références d'objet.
