---
title: Code First pour une base de données existante - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490608"
---
# <a name="code-first-to-an-existing-database"></a>Code First pour une base de données existante
Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Code First ciblant une base de données existante. Code tout d’abord vous permet de définir votre modèle à l’aide de C\# ou les classes VB.Net. Une configuration supplémentaire si vous le souhaitez peut être effectuée à l’aide des attributs dans vos classes et les propriétés ou à l’aide d’une API fluent.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo est [désormais disponible sur Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Conditions préalables

Vous devez avoir **Visual Studio 2012** ou **Visual Studio 2013** pour effectuer cette procédure.

Vous devez également version **6.1** (ou version ultérieure) de la **Entity Framework Tools pour Visual Studio** installé. Consultez [obtenir Entity Framework](~/ef6/fundamentals/install.md) pour plus d’informations sur l’installation de la dernière version des outils Entity Framework.

## <a name="1-create-an-existing-database"></a>1. Créer une base de données existante

En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.

Procédons à générer la base de données.

-   Ouvrir Visual Studio
-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de **Explorateur de serveurs** avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données

    ![Sélectionnez la Source de données](~/ef6/media/selectdatasource.png)

-   Connectez-vous à votre instance de base de données locale, puis entrez **blogs** en tant que le nom de la base de données

    ![Connexion de base de données locale](~/ef6/media/localdbconnection.png)

-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**

    ![Base de données de boîte de dialogue Créer](~/ef6/media/createdatabasedialog.png)

-   La nouvelle base de données s’affiche maintenant dans l’Explorateur de serveurs, avec le bouton droit dessus et sélectionnez **nouvelle requête**
-   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Création de l’application

Pour simplifier les choses, nous allons créer une application de console de base qui utilise Code First pour accéder aux données :

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le menu de gauche et **Application Console**
-   Entrez **CodeFirstExistingDatabaseSample** comme nom
-   Sélectionnez **OK**.

 

## <a name="3-reverse-engineer-model"></a>3. Modèle d’ingénierie à rebours

Nous allons utiliser Entity Framework Tools pour Visual Studio pour nous aider à générer du code initial pour mapper à la base de données. Ces outils sont simplement génération du code que vous pouvez également taper manuellement si vous préférez.

-   **Projet -&gt; ajouter un nouvel élément...**
-   Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**
-   Entrez **BloggingContext** comme nom et cliquez sur **OK**
-   Cette opération lance le **Assistant Entity Data Model**
-   Sélectionnez **Code First à partir de la base de données** et cliquez sur **suivant**

    ![Un CFE Assistant](~/ef6/media/wizardonecfe.png)

-   Sélectionnez la connexion à la base de données que vous avez créé dans la première section et cliquez sur **suivant**

    ![CFE deux Assistant](~/ef6/media/wizardtwocfe.png)

-   Cliquez sur la case à cocher en regard **Tables** pour importer toutes les tables, cliquez sur **terminer**

    ![CFE trois Assistant](~/ef6/media/wizardthreecfe.png)

Une fois le processus d’ingénierie à rebours terminé un nombre d’éléments seront ajoutés au projet, nous allons examiner ce qui a été ajouté.

### <a name="configuration-file"></a>fichier de configuration

Un fichier App.config a été ajouté au projet, ce fichier contient la chaîne de connexion à la base de données existante.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Vous remarquerez que trop d’autres paramètres dans le fichier de configuration, il s’agit des paramètres EF par défaut qui indiquent à Code First où créer des bases de données. Étant donné que nous réalisons l’association à une base de données existante, ces paramètres seront ignorés dans notre application.*

### <a name="derived-context"></a>Contexte dérivé

Un **BloggingContext** classe a été ajoutée au projet. Le contexte représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données.
Le contexte expose un **DbSet&lt;TEntity&gt;**  pour chaque type dans notre modèle. Vous remarquerez également que le constructeur par défaut qui appelle un constructeur de base à l’aide de la **nom =** syntaxe. Ce code indique à Code First que la chaîne de connexion à utiliser pour ce contexte doit être chargée à partir du fichier de configuration.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Vous devez toujours utiliser le **nom =** syntaxe lorsque vous utilisez une chaîne de connexion dans le fichier de configuration. Cela garantit que si la chaîne de connexion n’est pas présente puis Entity Framework lève au lieu de créer une nouvelle base de données par convention.*

### <a name="model-classes"></a>Classes de modèle

Enfin, un **Blog** et **Post** classe ont également été ajoutés au projet. Ce sont les classes de domaine qui composent notre modèle. Vous verrez des Annotations de données appliqués aux classes pour spécifier la configuration où les conventions Code First seraient non s’aligner sur la structure de la base de données existante. Par exemple, vous verrez la **StringLength** annotation sur **Blog.Name** et **Blog.Url** , car elles ont une longueur maximale de **200** dans le base de données (la valeur par défaut de Code First consiste à utiliser la longueur maximale prise en charge par le fournisseur de base de données - **nvarchar (max)** dans SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Lire et écrire des données

Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données. Implémentez le **Main** méthode dans **Program.cs** comme indiqué ci-dessous. Ce code crée une nouvelle instance de notre contexte et puis l’utilise pour insérer un nouveau **Blog**. Elle utilise ensuite une requête LINQ pour récupérer tous les **Blogs** à partir de la base de données classé par ordre alphabétique par **titre**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Que se passe-t-il si ma base de données change ?

Code First à l’Assistant de base de données est conçu pour générer un ensemble de point de départ de classes que vous pouvez ensuite modifier et modifier. Si votre schéma de base de données est modifié vous pouvez manuellement modifier les classes ou effectuer une autre ingénierie à rebours pour remplacer les classes.

## <a name="using-code-first-migrations-to-an-existing-database"></a>À l’aide des Migrations Code First pour une base de données existante

Si vous souhaitez utiliser les Migrations Code First avec une base de données existante, consultez [Migrations Code First pour une base de données existante](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu développement Code First à l’aide de la base de données existante. Nous avons utilisé les outils Entity Framework pour Visual Studio pour rétroconcevoir un ensemble de classes qui mappé à la base de données et peut être utilisé pour stocker et récupérer des données.
