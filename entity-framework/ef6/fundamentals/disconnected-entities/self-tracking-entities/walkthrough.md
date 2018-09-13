---
title: Entités procédure pas à pas - EF6 de suivi automatique
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: d89c452410d34bea71e8220aae141c3bfca3e1ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490270"
---
# <a name="self-tracking-entities-walkthrough"></a>Procédure pas à pas de suivi automatique entités
> [!IMPORTANT]
> Nous ne recommandons plus d’utiliser le modèle des entités de suivi automatique. Il reste disponible uniquement pour prendre en charge les applications existantes. Si votre application doit utiliser des graphiques d’entités déconnectés, envisagez d’autres alternatives comme les [Entités traçables](http://trackableentities.github.io/), qui présentent une technologie similaire aux entités de suivi automatique, mais développée de manière plus active par la communauté, ou bien l’écriture de code personnalisé à l’aide des API de suivi de changements de bas niveau.

Cette procédure pas à pas illustre le scénario dans lequel un service Windows Communication Foundation (WCF) expose une opération qui retourne un graphique d’entité. Ensuite, une application cliente manipule ce graphique et soumet les modifications à une opération de service qui valide et enregistre les mises à jour dans une base de données à l’aide d’Entity Framework.

Avant d’effectuer cette procédure pas à pas, vérifiez que vous lire le [Self-Tracking Entities](index.md) page.

Cette procédure pas à pas effectue les actions suivantes :

-   Crée une base de données pour accéder à.
-   Crée une bibliothèque de classes qui contient le modèle.
-   Remplace le modèle Générateur d’entité de suivi automatique.
-   Déplace les classes d’entité vers un projet distinct.
-   Crée un service WCF qui expose des opérations pour interroger et enregistrer des entités.
-   Crée des applications (Console et WPF) qui utilisent le service client.

Nous allons utiliser à la première base de données dans cette procédure pas à pas, mais les mêmes techniques s’appliquent également à Model First.

## <a name="pre-requisites"></a>Conditions préalables

Pour effectuer cette procédure pas à pas, vous devez une version récente de Visual Studio.

## <a name="create-a-database"></a>Créer une base de données

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :

-   Si vous utilisez Visual Studio 2012 puis vous allez créer une base de données de base de données locale.
-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.

Procédons à générer la base de données.

-   Ouvrir Visual Studio
-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devrez sélectionner **Microsoft SQL Server** comme source de données
-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé
-   Entrez **STESample** en tant que le nom de la base de données
-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**
-   La nouvelle base de données s’affiche désormais dans l’Explorateur de serveurs
-   Si vous utilisez Visual Studio 2012
    -   Avec le bouton droit sur la base de données dans l’Explorateur de serveurs, puis sélectionnez **nouvelle requête**
    -   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**
-   Si vous utilisez Visual Studio 2010
    -   Sélectionnez **données -&gt; Transact SQL éditeur -&gt; nouvelle connexion à la requête...**
    -   Entrez **.\\ SQLEXPRESS** en tant que nom du serveur et cliquez sur **OK**
    -   Sélectionnez le **STESample** de base de données dans la liste déroulante vers le bas en haut de l’éditeur de requête
    -   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **exécuter SQL**

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Créer le modèle

Tout d’abord, nous avons besoin d’un projet de placer le modèle dans.

-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Visual C\#**  dans le volet gauche, puis **bibliothèque de classes**
-   Entrez **STESample** comme nom et cliquez sur **OK**

Nous allons maintenant créer un modèle simple dans le Concepteur EF pour accéder à notre base de données :

-   **Projet -&gt; ajouter un nouvel élément...**
-   Sélectionnez **données** dans le volet gauche, puis **ADO.NET Entity Data Model**
-   Entrez **BloggingModel** comme nom et cliquez sur **OK**
-   Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**
-   Entrez les informations de connexion pour la base de données que vous avez créé dans la section précédente
-   Entrez **BloggingContext** comme nom de la chaîne de connexion et cliquez sur **suivant**
-   Cochez la case à côté **Tables** et cliquez sur **terminer**

## <a name="swap-to-ste-code-generation"></a>Échange à la génération de Code STE

Nous devons à présent de désactiver la génération de code par défaut et l’échange à Self-Tracking Entities.

### <a name="if-you-are-using-visual-studio-2012"></a>Si vous utilisez Visual Studio 2012

-   Développez **BloggingModel.edmx** dans **l’Explorateur de solutions** et supprimer le **BloggingModel.tt** et **BloggingModel.Context.tt** 
     *Cela désactive la génération de code par défaut*
-   Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une zone vide **ajouter un élément de génération de Code...**
-   Sélectionnez **Online** dans le volet gauche et recherchez **STE Générateur**
-   Sélectionnez le **générateur STE pour C\#**  modèle, entrez **STETemplate** comme nom et cliquez sur **ajouter**
-   Le **STETemplate.tt** et **STETemplate.Context.tt** fichiers sont ajoutés imbriqué sous le fichier BloggingModel.edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Si vous utilisez Visual Studio 2010

-   Avec le bouton droit sur le Concepteur EF aire de conception et sélectionnez une zone vide **ajouter un élément de génération de Code...**
-   Sélectionnez **Code** dans le volet gauche, puis **Générateur d’entité de suivi automatique ADO.NET**
-   Entrez **STETemplate** comme nom et cliquez sur **ajouter**
-   Le **STETemplate.tt** et **STETemplate.Context.tt** fichiers sont ajoutés directement à votre projet

## <a name="move-entity-types-into-separate-project"></a>Déplacer les Types d’entités dans un projet distinct

Pour utiliser Self-Tracking Entities notre application cliente doit accéder aux classes d’entité générée à partir de notre modèle. Étant donné que nous ne souhaitons pas exposer l’ensemble du modèle à l’application cliente, nous allons déplacer les classes d’entité dans un projet distinct.

La première étape consiste à arrêter la génération de classes d’entité dans le projet existant :

-   Avec le bouton droit sur **STETemplate.tt** dans **l’Explorateur de solutions** et sélectionnez **propriétés**
-   Dans le **propriétés** fenêtre clair **TextTemplatingFileGenerator** à partir de la **CustomTool** propriété
-   Développez **STETemplate.tt** dans **l’Explorateur de solutions** et supprimez tous les fichiers imbriqués dans celui-ci

Ensuite, nous allons ajouter un nouveau projet et générer les classes d’entité qu’il contient

-   **Fichier -&gt; Add -&gt; projet...**
-   Sélectionnez **Visual C\#**  dans le volet gauche, puis **bibliothèque de classes**
-   Entrez **STESample.Entities** comme nom et cliquez sur **OK**
-   **Projet -&gt; ajouter un élément existant...**
-   Accédez à la **STESample** dossier du projet
-   Sélectionnez cette option pour afficher les **tous les fichiers (\*.\*)**
-   Sélectionnez le **STETemplate.tt** fichier
-   Cliquez sur la flèche déroulante à côté du **ajouter** bouton et sélectionnez **ajouter en tant que lien**

    ![Ajouter le modèle lié](~/ef6/media/addlinkedtemplate.png)

Nous allons également s’assurer que les classes d’entité sont générés dans le même espace de noms comme contexte. Simplement, cela réduit le nombre d’à l’aide des instructions, que nous devons ajouter tout au long de notre application.

-   Avec le bouton droit sur le texte lié **STETemplate.tt** dans **l’Explorateur de solutions** et sélectionnez **propriétés**
-   Dans le **propriétés** ensemble de la fenêtre **Namespace d’outil personnalisé** à **STESample**

Le code généré par le modèle STE sera besoin d’une référence à **System.Runtime.Serialization** afin de compiler. Cette bibliothèque est nécessaire pour WCF **DataContract** et **DataMember** attributs qui sont utilisés sur les types d’entité sérialisables.

-   Cliquez avec le bouton droit sur le **STESample.Entities** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence...**
    -   Dans Visual Studio 2012 – la case à cocher à côté **System.Runtime.Serialization** et cliquez sur **OK**
    -   Dans Visual Studio 2010 - sélectionnez **System.Runtime.Serialization** et cliquez sur **OK**

Enfin, le projet avec notre contexte qu’il contient est besoin d’une référence aux types d’entité.

-   Cliquez avec le bouton droit sur le **STESample** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence...**
    -   Dans Visual Studio 2012 - sélectionnez **Solution** dans le volet gauche, cochez la case à côté **STESample.Entities** et cliquez sur **OK**
    -   Dans Visual Studio 2010 - sélectionner le **projets** onglet, sélectionnez **STESample.Entities** et cliquez sur **OK**

>[!NOTE]
> Une autre option pour déplacer les types d’entité vers un projet distinct consiste à déplacer le fichier de modèle, plutôt que de les lier à partir de son emplacement par défaut. Si vous procédez ainsi, vous devez mettre à jour le **inputFile** variable dans le modèle pour fournir le chemin d’accès relatif au fichier edmx (dans cet exemple serait **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Créer un Service WCF

Il est à présent temps ajouter un Service WCF pour exposer nos données, nous allons commencer par créer le projet.

-   **Fichier -&gt; Add -&gt; projet...**
-   Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application de Service WCF**
-   Entrez **STESample.Service** comme nom et cliquez sur **OK**
-   Ajoutez une référence à la **System.Data.Entity** assembly
-   Ajoutez une référence à la **STESample** et **STESample.Entities** projets

Nous avons besoin de copier la chaîne de connexion EF à ce projet afin qu’il est trouvé lors de l’exécution.

-   Ouvrez le **App.Config** de fichiers pour le ** STESample ** projet, puis copiez le **connectionStrings** élément
-   Coller le **connectionStrings** en tant qu’un élément enfant de le **configuration** élément de la **Web.Config** de fichiers dans le **STESample.Service** projet

Il est maintenant temps d’implémenter le service réel.

-   Ouvrez **IService1.cs** et remplacez le contenu par le code suivant

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Ouvrez **Service1.svc** et remplacez le contenu par le code suivant

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Consommer le Service à partir d’une Application Console

Nous allons créer une application console qui utilise notre service.

-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application Console**
-   Entrez **STESample.ConsoleTest** comme nom et cliquez sur **OK**
-   Ajoutez une référence à la **STESample.Entities** projet

Nous avons besoin d’une référence de service à notre service WCF

-   Avec le bouton droit le **STESample.ConsoleTest** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence de Service...**
-   Cliquez sur **découvrir**
-   Entrez **BloggingService** comme l’espace de noms et cliquez sur **OK**

Nous pouvons maintenant écrire du code pour consommer le service.

-   Ouvrez **Program.cs** et remplacez le contenu par le code suivant.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Vous pouvez à présent exécuter l’application pour la voir en action.

-   Avec le bouton droit le **STESample.ConsoleTest** projet **l’Explorateur de solutions** et sélectionnez **déboguer -&gt; démarrer une nouvelle instance**

Vous verrez la sortie suivante lorsque l’application s’exécute.

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Consommer le Service à partir d’une Application WPF

Nous allons créer une application WPF qui utilise notre service.

-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Visual C\#**  dans le volet gauche, puis **Application WPF**
-   Entrez **STESample.WPFTest** comme nom et cliquez sur **OK**
-   Ajoutez une référence à la **STESample.Entities** projet

Nous avons besoin d’une référence de service à notre service WCF

-   Avec le bouton droit le **STESample.WPFTest** projet **l’Explorateur de solutions** et sélectionnez **ajouter une référence de Service...**
-   Cliquez sur **découvrir**
-   Entrez **BloggingService** comme l’espace de noms et cliquez sur **OK**

Nous pouvons maintenant écrire du code pour consommer le service.

-   Ouvrez **MainWindow.xaml** et remplacez le contenu par le code suivant.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Ouvrez le code-behind de MainWindow (**MainWindow.xaml.cs**) et remplacez le contenu par le code suivant

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Vous pouvez à présent exécuter l’application pour la voir en action.

-   Avec le bouton droit le **STESample.WPFTest** projet **l’Explorateur de solutions** et sélectionnez **déboguer -&gt; démarrer une nouvelle instance**
-   Vous pouvez manipuler les données à l’aide de l’écran et l’enregistrer via le service en utilisant le **enregistrer** bouton

![Fenêtre principale de WPF](~/ef6/media/wpf.png)
