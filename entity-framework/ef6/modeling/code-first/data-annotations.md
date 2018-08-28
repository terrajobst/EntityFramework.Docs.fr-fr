---
title: Annotations de données First - EF6 de code
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 0ab66afa3babafe657b3ddb32c02c3fba0ae310e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994584"
---
# <a name="code-first-data-annotations"></a>Annotations de données Code First
> [!NOTE]
> **EF4.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 4.1. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Le contenu de cette page est une adaptation d’et que l’article ont été écrit par Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First vous permet d’utiliser vos propres classes de domaine pour représenter le modèle EF reposant sur pour exécuter des requêtes sur, modifier le suivi et la mise à jour des fonctions. Code exploite tout d’abord un modèle de programmation appelé convention sur configuration. Cela signifie que le code tout d’abord supposera que vos classes suivent les conventions utilisées par Entity Framework. Dans ce cas, EF pourrez travailler sur les détails qu’il doit faire son travail. Toutefois, si vos classes ne suivent pas ces conventions, vous avez la possibilité d’ajouter des configurations à vos classes afin de fournir d’EF avec les informations que nécessaires.

Tout d’abord les code vous offre deux façons d’ajouter ces configurations à vos classes. Une utilise des attributs simples appelés DataAnnotations et l’autre est tout d’abord à l’aide de code est l’API Fluent, qui vous offre un moyen de décrire des configurations de manière impérative dans le code.

Cet article se concentrera sur l’utilisation de DataAnnotations (dans l’espace de noms System.ComponentModel.DataAnnotations) pour configurer vos classes – les configurations plus fréquemment requises de mise en surbrillance. DataAnnotations sont également comprises par un nombre d’applications .NET, telles qu’ASP.NET MVC qui autorise ces applications d’exploiter les mêmes annotations de validations côté client.


## <a name="the-model"></a>Le modèle

Je vais vous montrer DataAnnotations premier avec une simple paire de classes de code : Blog et Post.

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

Lorsqu’ils sont, les classes de Blog et Post facilement suivent la convention de premier code et requis sans ajustements pour aider à EF de travailler avec eux. Mais vous pouvez également utiliser les annotations pour fournir plus d’informations à EF sur les classes et de la base de données qu’ils associent aux.

 

## <a name="key"></a>Touche

Entity Framework s’appuie sur chaque entité ayant une valeur de clé qu’il utilise pour les entités de suivi. Les conventions de code en premier dépend est comment il implique la propriété qui est la clé dans chacune des classes de premier code. Cette convention consiste à rechercher pour une propriété nommée « Id » ou qui combine le nom de classe et un « Id », tels que « BlogId ». La propriété doit être mappée à une colonne de clé primaire dans la base de données.

Les classes Blog et Post suivent cette convention. Mais que se passe-t-il si ce n’était pas ? Que se passe-t-il si Blog utilisé le nom *PrimaryTrackingKey* à la place ou même *foo*? Si le code tout d’abord ne trouve pas d’une propriété qui correspond à cette convention lève une exception en raison de l’exigence d’Entity Framework que vous devez disposer d’une propriété de clé. Vous pouvez utiliser l’annotation de clé pour spécifier quelle propriété doit être utilisé comme valeur EntityKey.

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

Si vous êtes tout d’abord à l’aide de code est fonction de génération de base de données, la table de Blog aura une colonne clé primaire nommée PrimaryTrackingKey qui est également défini en tant qu’identité par défaut.

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Clés composites

Entity Framework prend en charge les clés composites - clés primaires qui se composent de plusieurs propriétés. Par exemple, votre peut avoir une classe de Passport dont la clé primaire est une combinaison de PassportNumber et IssuingCountry.

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Si vous deviez essayer et utiliser la classe ci-dessus dans votre modèle EF vous obtiendriez un indiquant d’exceptions InvalidOperationException ;

*Impossible de déterminer le composite primaire tri par clé pour le type « Passport ». Utilisez la ColumnAttribute ou la méthode HasKey pour spécifier l’ordre des clés primaires composites.*

Lorsque vous avez des clés composites, Entity Framework vous oblige à définir un ordre des propriétés de clé. Pour cela, à l’aide de l’annotation de colonne pour spécifier un ordre.

>[!NOTE]
> La valeur d’ordre est relative (plutôt qu’un index est basé) pour toutes les valeurs puissent être utilisées. Par exemple, 100 et 200 serait acceptable à la place de 1 et 2.

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Si vous avez des entités avec des clés étrangères composites vous devez spécifier la même colonne de classement que vous avez utilisé pour les propriétés de clé primaire correspondantes.

Uniquement l’ordre relatif dans les propriétés de clé étrangères doit être le même, les valeurs exactes affectés à **ordre** n’avez pas besoin de correspondre. Par exemple, dans la classe suivante, 3 et 4 peut servir à la place de 1 et 2.

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a>Obligatoire

L’annotation requise indique à EF qu’une propriété particulière est requise.

Ajout nécessaire pour la propriété Title forcera EF (et MVC) pour vous assurer que la propriété comporte des données.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sans aucun supplémentaire aucune modification de code ou de balisage dans l’application, une application MVC effectue la validation côté client, création même dynamique d’un message en utilisant les noms de propriété et d’annotation.

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

L’attribut Required affecte également la base de données généré en effectuant la propriété mappée non nullable. Vérifiez que le champ titre est passé à « not null ».

>[!NOTE]
> Dans certains cas il ne peut pas être possible pour la colonne dans la base de données soit non nullable même si la propriété est requise. Par exemple, quand à l’aide de données de stratégie de l’héritage TPH pour plusieurs types est stockée dans une table unique. Si un type dérivé inclut une propriété obligatoire, que la colonne ne peut pas être rendue non nullable comme pas tous les types dans la hiérarchie ont cette propriété.

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength et MinLength

Les attributs MaxLength et MinLength autoriser vous permettent de spécifier des validations de propriété supplémentaires, comme vous le faisiez avec requis.

Voici le BloggerName avec les exigences de longueur. L’exemple montre également comment combiner des attributs.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

L’annotation MaxLength aura un impact sur la base de données en définissant la longueur de la propriété à 10.

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

Annotation de côté client MVC et annotation de côté serveur EF 4.1 les deux respecteront cette validation, création à nouveau dynamique d’un message d’erreur : « le champ BloggerName doit être un type de chaîne ou tableau avec une longueur maximale de « 10 ». » Ce message est un peu long. Nombre illimité d’annotations vous permettre de spécifier un message d’erreur avec l’attribut de message d’erreur.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Vous pouvez également spécifier le message d’erreur dans l’annotation requise.

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Convention de premier code impose que chaque propriété d’un type de données pris en charge est représentée dans la base de données. Mais ce n’est pas toujours le cas dans vos applications. Par exemple, vous pouvez avoir une propriété dans la classe de Blog qui crée un code basé sur les champs titre et BloggerName. Cette propriété peut être créée dynamiquement et ne doit pas être stocké. Vous pouvez marquer toutes les propriétés qui ne correspondent pas à la base de données avec l’annotation NotMapped telle que cette propriété BlogCode.

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a>ComplexType

Il n’est pas rare de décrire vos entités de domaine sur un ensemble de classes et de couche ensuite ces classes pour décrire une entité complète. Par exemple, vous pouvez ajouter une classe appelée BlogDetails à votre modèle.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Notez que BlogDetails n’a pas de n’importe quel type de propriété de clé. Dans la conception conduite par domaine, BlogDetails est appelé pour un objet de valeur. Entity Framework fait référence aux objets de valeur comme des types complexes.  Les types complexes ne peuvent pas être suivies sur leurs propres.

Toutefois en tant que propriété dans la classe de Blog, BlogDetails il sera suivie dans le cadre d’un objet de Blog. Dans l’ordre pour code first pour reconnaître de cela, vous devez marquer la classe BlogDetails comme un ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Vous pouvez maintenant ajouter une propriété dans la classe de Blog pour représenter le BlogDetails pour ce blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

Dans la base de données, la table de Blog contiendra toutes les propriétés du blog, y compris les propriétés contenues dans sa propriété BlogDetail. Par défaut, chacun d’eux est précédé par le nom du type complexe, BlogDetail.

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

Autre Remarque intéressant est que bien que la propriété DateCreated a été définie comme une valeur non nullable DateTime dans la classe, le champ de base de données correspondante est nullable. Vous devez utiliser l’annotation requise si vous souhaitez affecter le schéma de base de données.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

L’annotation ConcurrencyCheck vous permet de marquer une ou plusieurs propriétés à utiliser pour l’accès concurrentiel dans la base de données lorsqu’un utilisateur modifie ou supprime une entité. Si vous avez travaillé avec le Concepteur EF, celui-ci s’aligne définition ConcurrencyMode d’une propriété sur Fixed.

Nous allons voir comment ConcurrencyCheck fonctionne en l’ajoutant à la propriété BloggerName.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Lorsque SaveChanges est appelée, en raison de l’annotation ConcurrencyCheck sur le champ BloggerName, la valeur d’origine de cette propriété sera utilisée dans la mise à jour. La commande tente de localiser la ligne correcte par filtrage non seulement sur la valeur de clé, mais également sur la valeur d’origine de BloggerName.  Voici les parties critiques de la commande de mise à jour envoyées à la base de données, où vous pouvez voir la commande met à jour la ligne qui a un PrimaryTrackingKey est 1 et un BloggerName de « Julie » qui était la valeur d’origine quand ce blog a été récupéré à partir de la base de données.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Si quelqu'un a modifié le nom de blogueur pour ce blog en attendant, cette mise à jour échoue et vous obtiendrez une DbUpdateConcurrencyException dont vous aurez besoin pour gérer.

 

## <a name="timestamp"></a>Horodatage

Il est plus courant d’utiliser des champs rowversion ou timestamp pour le contrôle d’accès concurrentiel. Mais au lieu d’utiliser l’annotation ConcurrencyCheck, vous pouvez utiliser l’annotation d’horodateur plus spécifique, que le type de la propriété est le tableau d’octets. Code tout d’abord traite les propriétés horodatage exactement en tant que propriétés de ConcurrencyCheck, mais il garantit également que le champ de base de données qui génère d’abord code est non nullable. Vous ne pouvez avoir qu’une seule propriété d’horodatage dans une classe donnée.

Ajout de la propriété suivante à la classe de Blog :

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

résultats dans le code créant d’abord une colonne timestamp non null dans la table de base de données.

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Table et colonne

Si vous permettez à Code First créer la base de données, vous souhaiterez modifier le nom des tables et des colonnes en cours de création. Vous pouvez également utiliser Code First avec une base de données existante. Mais il n’est pas toujours le cas que les noms des classes et des propriétés dans votre domaine correspondent aux noms des tables et des colonnes dans votre base de données.

Ma classe est nommée Blog et par convention, code tout d’abord part du principe que cela permet de mapper vers une table nommée Blogs. Si tel n’est pas le cas, vous pouvez spécifier le nom de la table avec l’attribut de la Table. Ici, par exemple, l’annotation Spécifie que le nom de table est InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

L’annotation de la colonne est un adept plus en spécifiant les attributs d’une colonne mappée. Vous pouvez stipuler un nom, type de données ou même l’ordre dans lequel une colonne apparaît dans la table. Voici un exemple de l’attribut de colonne.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Ne confondez pas attribut TypeName de la colonne avec le DataType DataAnnotation. Type de données est une annotation utilisée pour l’interface utilisateur et est ignorée par Code First.

Voici la table une fois qu’il est régénéré. Le nom de la table a changé à InternalBlogs et colonne de Description à partir du type complex est désormais BlogDescription. Étant donné que le nom a été spécifié dans l’annotation, code tout d’abord n’utilise pas la convention de commencer le nom de colonne avec le nom du type complexe.

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Une fonctionnalités de base de données important est la possibilité d’avoir les propriétés calculées. Si vous mappez vos classes de Code First pour les tables qui contiennent des colonnes calculées, vous ne voulez Entity Framework pour tenter de mettre à jour ces colonnes. Mais vous ne souhaitez pas que EF pour renvoyer ces valeurs à partir de la base de données une fois que vous avez inséré ou mis à jour des données. Vous pouvez utiliser l’annotation DatabaseGenerated pour signaler les propriétés dans votre classe, ainsi que l’énumération calculé. Autres énumérations sont None et d’identité.

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Vous pouvez utiliser la base de données générée sur les colonnes byte ou timestamp lorsque code génère tout d’abord la base de données, sinon vous devez uniquement l’utiliser en pointant sur les bases de données existantes, car le code ne sont pas tout d’abord être en mesure de déterminer la formule pour la colonne calculée.

Vous lisez supérieur à celui par défaut, une propriété de clé est un entier deviendra une clé d’identité dans la base de données. Qui serait le même que l’affectation DatabaseGenerated DatabaseGenerationOption.Identity. Si vous ne souhaitez pas qu’il soit une clé d’identité, vous pouvez définir la valeur à DatabaseGenerationOption.None.

 

## <a name="index"></a>Index

> [!NOTE]
> **EF6.1 et versions ultérieures uniquement** -attribut de l’Index a été introduite dans Entity Framework 6.1. Si vous utilisez une version antérieure les informations contenues dans cette section ne s’applique pas.

Vous pouvez créer un index sur une ou plusieurs colonnes à l’aide de la **IndexAttribute**. Ajout de l’attribut à une ou plusieurs propriétés sera provoquer d’EF créer l’index correspondant dans la base de données lorsqu’il crée la base de données ou structurer le correspondantes **CreateIndex** appelle si vous utilisez Code First Migrations.

Par exemple, le code suivant entraîne un index créé sur le **évaluation** colonne de la **billets** table dans la base de données.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

Par défaut, l’index sera nommé **IX\_&lt;nom de la propriété&gt;**  (IX\_Rating dans l’exemple ci-dessus). Vous pouvez également spécifier un nom pour l’index cependant. L’exemple suivant spécifie que l’index doit être nommé **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Par défaut, les index sont non uniques, mais vous pouvez utiliser la **IsUnique** nommé de paramètre pour spécifier qu’un index doit être unique. L’exemple suivant présente un index unique sur une **utilisateur**du nom de connexion.

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a>Index de plusieurs colonnes

Les index qui s’étendent sur plusieurs colonnes sont spécifiés à l’aide du même nom dans plusieurs annotations d’Index pour une table donnée. Lorsque vous créez des index multi-colonnes, vous devez spécifier un ordre pour les colonnes dans l’index. Par exemple, le code suivant crée un index multicolonnes sur **évaluation** et **BlogId** appelé **IX\_BlogAndRating**. **BlogId** est la première colonne dans l’index et **évaluation** est le deuxième.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Relations d’attributs : InverseProperty et ForeignKey

> [!NOTE]
> Cette page fournit des informations sur la configuration des relations dans votre modèle Code First à l’aide des Annotations de données. Pour obtenir des informations générales sur les relations dans Entity Framework et comment accéder à et manipuler des données à l’aide de relations, consultez [relations & Propriétés de Navigation](~/ef6/fundamentals/relationships.md). *

Convention de premier code s’occupe des relations plus courantes dans votre modèle, mais il existe quelques cas où il a besoin d’aide.

Modification du nom de la propriété de clé dans la classe Blog créée un problème avec sa relation à Post. 

Lors de la génération de la base de données, code tout d’abord voit la propriété BlogId dans la classe de publication et la reconnaît, par la convention qu’elle correspond à un nom de classe et un « Id », comme une clé étrangère à la classe de Blog. Mais il n’existe aucune propriété BlogId dans la classe de blog. La solution consiste à créer une propriété de navigation dans le billet d’utiliser le DataAnnotation étrangère pour aider à code tout d’abord comprendre comment créer la relation entre les deux classes, à l’aide de la propriété Post.BlogId, ainsi que la façon de spécifier des contraintes dans la base de données.

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

La contrainte dans la base de données montre une relation entre InternalBlogs.PrimaryTrackingKey et Posts.BlogId. 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

Le InverseProperty est utilisé lorsque vous avez plusieurs relations entre les classes.

Dans la classe Post, voulez-vous effectuer le suivi de qui l’a écrit un billet de blog, ainsi que qui a été modifié. Voici deux nouvelles propriétés de navigation pour la classe de publication.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Vous devrez également ajouter dans la classe Person référencée par ces propriétés. La classe Person a les propriétés de navigation à la publication, un pour tous les messages du écrites par la personne et un pour tous les billets de mise à jour par cette personne.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Tout d’abord code n’est pas capable de faire correspondre les propriétés dans les deux classes sur son propre. La table de base de données pour des publications doit avoir une clé étrangère de la personne CreatedBy et une pour la personne de UpdatedBy, le code, mais tout d’abord créera quatre propriétés de clé étrangère est : personne\_Id, personne\_Id1, CreatedBy\_Id et UpdatedBy\_ID.

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

Pour résoudre ces problèmes, vous pouvez utiliser l’annotation InverseProperty pour spécifier l’alignement des propriétés.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Étant donné que la propriété PostsWritten en personne sait que cela fait référence au type de publication, il générera la relation à Post.CreatedBy. De même, PostsUpdated sera connecté à Post.UpdatedBy. Et tout d’abord code ne crée pas les clés étrangères supplémentaires.

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Récapitulatif

DataAnnotations vous permettent non seulement de décrire la validation côté client et serveur dans vos classes de premier code, mais ils vous permettent également d’améliorer et même corriger les hypothèses code va d’abord faire sur vos classes selon ses conventions. Avec DataAnnotations, vous pouvez optimiser pas uniquement les génération de schéma de base de données, mais vous pouvez également mapper vos classes de premier code pour une base de données existant.

Pendant qu’elles sont très flexibles, n’oubliez pas que DataAnnotations fournir uniquement le meilleur parti généralement les modifications de configuration que vous pouvez effectuer sur vos classes de premier code nécessaires. Pour configurer vos classes pour certains cas edge, vous devriez rechercher au mécanisme de configuration de remplacement API du Code First Fluent.
