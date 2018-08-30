---
title: Code First pour une base de données - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 50c6a4710bc50879304f64e781a46c4836f86882
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152476"
---
# <a name="code-first-to-a-new-database"></a>Code First pour une base de données
Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Code First ciblant une base de données. Ce scénario inclut une base de données qui n’existe pas de ciblage et Code First crée, ou une base de données vide ce Code First l’ajoutera des tables à. Code tout d’abord vous permet de définir votre modèle à l’aide de C\# ou les classes VB.Net. Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo fournit une introduction au développement Code First ciblant une base de données. Ce scénario inclut une base de données qui n’existe pas de ciblage et Code First crée, ou une base de données vide ce Code First l’ajoutera des tables à. Code tout d’abord vous permet de définir votre modèle à l’aide de classes c# ou VB.Net. Une configuration supplémentaire peut éventuellement être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.

**Présentée par** : [Rowan Miller](http://romiller.com/)

**Vidéo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Conditions préalables

Vous devez avoir au moins Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.

## <a name="1-create-the-application"></a>1. Création de l’application

Pour simplifier les choses, nous allons créer une application de console de base qui utilise Code First pour accéder aux données.

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le menu de gauche et **Application Console**
-   Entrez **CodeFirstNewDatabaseSample** comme nom
-   Sélectionnez **OK**.

## <a name="2-create-the-model"></a>2. Créer le modèle

Nous allons définir un modèle très simple à l’aide de classes. Nous allons simplement les définir dans le fichier Program.cs, mais dans une application réelle que vous serez diviser vos classes out en fichiers distincts et, potentiellement, un projet distinct.

Sous la définition de classe de programme dans le fichier Program.cs, ajoutez les deux classes suivantes.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Vous remarquerez que nous apportons les deux propriétés de navigation (Blog.Posts et Post.Blog) virtuel. Ainsi, la fonctionnalité de chargement différé d’Entity Framework. Le chargement différé signifie que le contenu de ces propriétés est automatiquement chargé à partir de la base de données lorsque vous essayez d’y accéder.

## <a name="3-create-a-context"></a>3. Créer un contexte

Il est maintenant temps de définir un contexte dérivé qui représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données. Nous définissons un contexte qui dérive de System.Data.Entity.DbContext et expose un DbSet typé&lt;TEntity&gt; pour chaque classe dans notre modèle.

Maintenant, nous commençons à utiliser des types à partir d’Entity Framework afin de nous devons ajouter le package EntityFramework NuGet.

-   **Projet –&gt; gérer les Packages NuGet...**
    Remarque : Si vous n’avez pas le **gérer les Packages NuGet...** option, vous devez installer le [version la plus récente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Sélectionnez le **Online** onglet
-   Sélectionnez le **EntityFramework** package
-   Cliquez sur **installer**

Ajoutez une instruction pour System.Data.Entity en haut du fichier Program.cs.

``` csharp
using System.Data.Entity;
```

Ci-dessous la classe Post dans le fichier Program.cs, ajoutez le contexte dérivé suivant.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Voici une liste complète de ce que Program.cs doit maintenant contenir.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

C’est tout le code que nous devons commencer stocker et récupérer des données. Évidemment, il existe un certain passe dans les coulisses et nous étudierons un que dans un moment, mais premier nous allons voir en action.

## <a name="4-reading--writing-data"></a>4. Lire et écrire des données

Implémentez la méthode Main dans Program.cs, comme indiqué ci-dessous. Ce code crée une nouvelle instance de notre contexte et il utilise ensuite pour insérer un nouveau Blog. Il utilise ensuite une requête LINQ pour récupérer tous les Blogs à partir de la base de données classé par ordre alphabétique par titre.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Vous pouvez maintenant exécuter l’application et le tester.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Où sont mes données ?

Par convention DbContext a créé une base de données pour vous.

-   Si une instance locale de SQL Express est disponible (installé par défaut avec Visual Studio 2010) puis Code First a créé la base de données sur cette instance
-   Si SQL Express n’est pas disponible, Code First sera tester et d’utiliser [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installé par défaut avec Visual Studio 2012)
-   La base de données est nommée après le nom qualifié complet de contexte dérivé, dans notre cas est **CodeFirstNewDatabaseSample.BloggingContext**

Il s’agit simplement les conventions par défaut et il existe différentes manières de modifier la base de données qui utilise Code First, informations supplémentaires sont disponibles dans le **comment DbContext détecte le modèle et la connexion de base de données** rubrique.
Vous pouvez vous connecter à cette base de données à l’aide de l’Explorateur de serveurs dans Visual Studio

-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données** et sélectionnez **ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé

Nous pouvons maintenant inspecter le schéma que Code First créé.

![SchemaInitial](~/ef6/media/schemainitial.png)

DbContext élaboré quelles classes à inclure dans le modèle en examinant les propriétés DbSet que nous avons défini. Il utilise ensuite l’ensemble de conventions Code First par défaut pour déterminer les noms de table et de colonne, de déterminer les types de données, de trouver les clés primaires, etc. Plus loin dans cette procédure pas à pas, nous allons examiner comment vous pouvez remplacer ces conventions.

## <a name="5-dealing-with-model-changes"></a>5. Gestion des modifications du modèle

Il est maintenant temps d’apporter des modifications à notre modèle, lorsque nous apporter ces modifications, que nous devons également mettre à jour le schéma de base de données. Pour ce faire, nous allons utiliser une fonctionnalité appelée Migrations Code First, ou des Migrations pour faire plus court.

Migrations nous permet d’avoir un ensemble ordonné d’étapes qui décrivent comment mettre à niveau (et rétrograder) notre schéma de base de données. Chacune de ces étapes, appelés une migration, contient du code qui décrit les modifications à appliquer. 

La première étape consiste à activer les Migrations Code First pour notre BloggingContext.

-   **Outils -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**
-   Exécutez la commande **Enable-Migrations** dans la Console du Gestionnaire de Package
-   Un nouveau dossier Migrations a été ajouté à notre projet qui contient deux éléments :
    -   **Configuration.cs** – ce fichier contient les paramètres de Migrations utilisera pour la migration BloggingContext. Nous n’avez pas besoin de modifier quoi que ce soit pour cette procédure pas à pas, mais voici où vous pouvez spécifier les données de valeur initiale, inscrire des fournisseurs pour les autres bases de données, modifie l’espace de noms que les migrations sont générées, etc.
    -   **&lt;horodatage&gt;\_InitialCreate.cs** – il s’agit votre première migration, il représente les modifications qui ont déjà été appliquées à la base de données en ne soient pas d’une base de données vide à celle qui inclut les tables de Blogs et des publications . Bien que nous permettent de Code First automatiquement créer ces tables pour nous, maintenant que nous avons choisi de Migrations qu’ils ont été converties en une Migration. Code tout d’abord a également enregistrée dans notre base de données locale que cette Migration a déjà été appliquée. L’horodatage sur le nom de fichier est utilisé pour le classement à des fins.

    Maintenant nous allons apporter une modification à notre modèle, ajoutez une propriété d’Url à la classe de Blog :

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Exécutez le **Add-Migration AddUrl** commande dans la Console du Gestionnaire de Package.
    La commande Add-Migration vérifie les modifications apportées depuis votre dernière migration et la structure d’une nouvelle migration avec toutes les modifications qui se trouvent. Nous pouvons nommez migrations ; Dans ce cas, nous appelons la « AddUrl » de migration.
    Le code généré automatiquement est à dire que nous devons ajouter une colonne de l’Url, qui peut contenir des données de chaîne, à dbo. Table de blogs. Si nécessaire, nous pouvons modifier le code généré automatiquement, mais qui n’est pas obligatoire dans ce cas.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Exécutez le **Update-Database** commande dans la Console du Gestionnaire de Package. Cette commande s’applique les migrations en attente à la base de données. Notre migration InitialCreate a déjà été appliquée afin de migrations seront applique seulement notre nouvelle migration AddUrl.
    Conseil : Vous pouvez utiliser la **– Verbose** commutateur lors de l’appel de mise à jour la base de données pour afficher le code SQL en cours d’exécution par rapport à la base de données.

La nouvelle colonne de l’Url est maintenant ajoutée à la table de Blogs dans la base de données :

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Annotations de données

Jusqu'à présent, nous avons simplement laisser EF découvrir le modèle à l’aide de ses conventions par défaut, mais il doit y être parfois nos classes ne suivent pas les conventions et nous devons être en mesure d’effectuer une configuration supplémentaire. Il existe deux options pour cela ; Nous allons examiner les Annotations de données dans cette section, puis sur l’API fluent dans la section suivante.

-   Nous allons ajouter une classe d’utilisateur à notre modèle

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Nous devons également ajouter un jeu à notre contexte dérivé

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Si nous avons essayé d’ajouter une migration vous obtenez un message d’erreur indiquant «*EntityType « User » ne possède aucune clé définie. Définissez la clé pour cet EntityType. »* Étant donné que EF n’a aucun moyen de savoir que nom d’utilisateur doit être la clé primaire de l’utilisateur.
-   Nous allons utiliser des Annotations de données maintenant nous devons ajouter une à l’aide de l’instruction en haut du fichier Program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Annoter désormais la propriété de nom d’utilisateur pour identifier la clé primaire

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Utilisez le **Add-Migration AddUser** commande pour générer automatiquement une migration pour appliquer ces modifications à la base de données
-   Exécutez le **Update-Database** commande pour appliquer la nouvelle migration à la base de données

La nouvelle table est maintenant ajoutée à la base de données :

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

La liste complète des annotations prises en charge par Entity Framework est :

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. API Fluent

Dans la section précédente, nous avons vu à l’aide des Annotations de données pour compléter ou remplacer ce qui a été détecté par convention. L’autre méthode permettant de configurer le modèle est via l’API fluent Code First.

La plupart des configuration de modèle peut être effectuée à l’aide des annotations de données simple. L’API fluent est un moyen plus avancé de spécifier une configuration de modèle qui couvre toutes les annotations de données peuvent faire de plus pour une configuration plus avancée n’est pas possible avec des annotations de données. Annotations de données et l’API fluent peuvent être utilisés ensemble.

Pour accéder à l’API fluent vous substituez la méthode OnModelCreating de DbContext. Supposons que nous voulions renommer la colonne User.DisplayName est stocké dans pour afficher\_nom.

-   Substituez la méthode OnModelCreating sur BloggingContext avec le code suivant

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Utilisez le **Add-Migration ChangeDisplayName** commande pour générer automatiquement une migration pour appliquer ces modifications à la base de données.
-   Exécutez le **Update-Database** commande pour appliquer la nouvelle migration à la base de données.

La colonne DisplayName a été renommée pour afficher\_nom :

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu développement Code First à l’aide d’une base de données. Nous défini un modèle à l’aide de classes, puis ce modèle permet de créer une base de données et de stocker et récupérer des données. Une fois que la base de données a été créé, nous avons utilisé les Migrations Code First pour modifier le schéma en tant que notre modèle a évolué. Nous avons également vu comment configurer un modèle à l’aide des Annotations de données et l’API Fluent.
