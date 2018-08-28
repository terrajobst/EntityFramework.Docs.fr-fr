---
title: Liaison de données avec WPF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 0b1f4d5ea204cd80acf42caa499732610daa0e31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994821"
---
# <a name="databinding-with-wpf"></a>Liaison de données avec WPF
Cette procédure pas à pas montre comment lier les types POCO à des contrôles WPF dans un formulaire « maître / détail ». L’application utilise les API Entity Framework pour remplir des objets avec des données à partir de la base de données, le suivi des modifications et conserver les données dans la base de données.

Le modèle définit deux types impliquées dans une relation un-à-plusieurs : **catégorie** (principal\\master) et **produit** (dépendants\\détail). Ensuite, les outils de Visual Studio sont utilisés pour lier les types définis dans le modèle pour les contrôles WPF. L’infrastructure de liaison de données WPF permet la navigation entre les objets connexes : sélection de lignes dans la vue principale, la vue de détail mettre à jour avec les données enfants correspondantes.

Les captures d’écran et les listings de code dans cette procédure pas à pas sont effectuées à partir de Visual Studio 2013, mais vous pouvez effectuer cette procédure pas à pas avec Visual Studio 2012 ou Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Utilisez l’Option 'Object' pour la création de Sources de données WPF

Avec une version précédente d’Entity Framework, nous avons utilisé est recommandé d’utiliser le **base de données** option lorsque vous créez une nouvelle Source de données basé sur un modèle créé avec le Concepteur EF. C’est parce que le concepteur génère un contexte dérivé ObjectContext et les classes d’entité dérivé à partir d’EntityObject. À l’aide de l’option de base de données vous permet d’écrire le meilleur code permettant d’interagir avec cette surface d’API.

Les concepteurs d’EF pour Visual Studio 2012 et Visual Studio 2013 générer un contexte qui dérive de DbContext ainsi que de simples classes d’entité POCO. Avec Visual Studio 2010, nous vous recommandons de passer en un modèle de génération de code qui utilise le DbContext comme décrit plus loin dans cette procédure pas à pas.

Lors de l’utilisation de la surface de cette API, vous devez utiliser le **objet** option lors de la création d’une nouvelle Source de données, comme illustré dans cette procédure pas à pas.

Si nécessaire, vous pouvez [revenir à la génération de code en fonction de ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pour les modèles créés avec le Concepteur EF.

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 est installé pour terminer cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également installer le package NuGet. Pour plus d’informations, consultez [installation NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Création de l’application

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le volet gauche et **WPFApplication** dans le volet droit
-   Entrez **WPFwithEFSample** comme nom
-   Sélectionnez **OK**.

## <a name="install-the-entity-framework-nuget-package"></a>Installez le package NuGet Entity Framework

-   Dans l’Explorateur de solutions, cliquez sur le **WinFormswithEFSample** projet
-   Sélectionnez **gérer les Packages NuGet...**
-   Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package
-   Cliquez sur **installer**  
    >[!NOTE]
    > En plus de l’assembly EntityFramework une référence à System.ComponentModel.DataAnnotations est également ajoutée. Si le projet a une référence à System.Data.Entity, puis elle va être supprimée lorsque le package EntityFramework est installé. L’assembly System.Data.Entity est n’est plus utilisé pour les applications Entity Framework 6.

## <a name="define-a-model"></a>Définir un modèle

Dans cette procédure pas à pas, que vous pouvez choisi d’implémenter un modèle à l’aide de Code First ou le Concepteur EF. Effectuez l’une des deux sections suivantes.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1 : Définir un modèle à l’aide de Code First

Cette section montre comment créer un modèle et sa base de données associée à l’aide de Code First. Passez à la section suivante (**Option 2 : définir un modèle à l’aide de la première base de données)** si vous préférez utiliser Database First pour inverser concevoir votre modèle à partir d’une base de données à l’aide du Concepteur EF

Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine).

-   Ajoutez une nouvelle classe à la **WPFwithEFSample :**
    -   Avec le bouton droit sur le nom du projet
    -   Sélectionnez **ajouter**, puis **nouvel élément**
    -   Sélectionnez **classe** et entrez **produit** pour le nom de classe
-   Remplacez le **produit** définition avec le code suivant de la classe :

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

Le **produits** propriété sur le **catégorie** classe et **catégorie** propriété sur le **produit** classe sont des propriétés de navigation. Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités.

Outre la définition des entités, vous devez définir une classe qui dérive de DbContext et expose DbSet&lt;TEntity&gt; propriétés. Le DbSet&lt;TEntity&gt; propriétés permettent le contexte de connaître les types que vous souhaitez inclure dans le modèle.

Une instance du type DbContext dérivée gère les objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, modifier le suivi et la persistance des données à la base de données.

-   Ajouter un nouveau **ProductContext** classe au projet avec la définition suivante :

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Compilez le projet.

### <a name="option-2-define-a-model-using-database-first"></a>Option 2 : Définir un modèle à l’aide de la première base de données

Cette section montre comment utiliser la première base de données à l’ingénierie inverse de votre modèle à partir d’une base de données à l’aide du Concepteur EF. Si vous avez terminé la section précédente (**Option 1 : définir un modèle à l’aide de Code First)**, puis ignorez cette section et passer directement à la **le chargement différé** section.

#### <a name="create-an-existing-database"></a>Créer une base de données existante

En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :

-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.
-   Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) base de données.

Procédons à générer la base de données.

-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé, puis entrez **produits** en tant que le nom de la base de données

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   La nouvelle base de données s’affiche maintenant dans l’Explorateur de serveurs, avec le bouton droit dessus et sélectionnez **nouvelle requête**
-   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Modèle d’ingénierie à rebours

Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.

-   **Projet -&gt; ajouter un nouvel élément...**
-   Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**
-   Entrez **ProductModel** comme nom et cliquez sur **OK**
-   Cette opération lance le **Assistant Entity Data Model**
-   Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Sélectionnez la connexion à la base de données que vous avez créé dans la première section, entrez **ProductContext** comme nom de la chaîne de connexion et cliquez sur **suivant**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Cliquez sur la case à cocher en regard de « Tables » pour importer toutes les tables, cliquez sur « Terminer »

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

Une fois que le processus d’ingénierie à rebours est terminé le nouveau modèle est ajouté à votre projet et ouvert pour l’afficher dans le Concepteur d’Entity Framework. Un fichier App.config a également été ajouté à votre projet avec les détails de connexion pour la base de données.

#### <a name="additional-steps-in-visual-studio-2010"></a>Étapes supplémentaires dans Visual Studio 2010

Si vous travaillez dans Visual Studio 2010 vous devrez mettre à jour le Concepteur EF pour utiliser la génération de code EF6.

-   Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**
-   Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**
-   Sélectionnez le **EF 6.x générateur DbContext pour C\#,** entrez **ProductsModel** comme nom et cliquez sur Ajouter

#### <a name="updating-code-generation-for-data-binding"></a>Mise à jour de la génération de code pour la liaison de données

Entity Framework génère du code à partir de votre modèle à l’aide de modèles T4. Les modèles fournis avec Visual Studio ou téléchargé à partir de la galerie Visual Studio sont destinés à usage général. Cela signifie que les entités générées à partir de ces modèles ont ICollection simple&lt;T&gt; propriétés. Toutefois, lors de données de la liaison à l’aide de WPF il est souhaitable d’utiliser **ObservableCollection** pour les propriétés de collection afin que WPF peut effectuer le suivi de modifications apportées aux collections. Dans cette optique, nous allons pour modifier les modèles pour utiliser ObservableCollection.

-   Ouvrez le **l’Explorateur de solutions** et recherchez **ProductModel.edmx** fichier
-   Rechercher la **ProductModel.tt** fichier qui doit être imbriqué sous le fichier ProductModel.edmx

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   Double-cliquez sur le fichier ProductModel.tt pour l’ouvrir dans l’éditeur Visual Studio
-   Rechercher et remplacer les deux occurrences de «**ICollection**« avec »**ObservableCollection**». Il s’agit trouve environ à lignes 296 et 484.
-   Rechercher et remplacer la première occurrence de «**HashSet**« avec »**ObservableCollection**». Cet événement se trouve approximativement à la ligne 50. **Ne le faites pas** remplacer la deuxième occurrence de HashSet figure plus loin dans le code.
-   Rechercher et remplacer la seule occurrence de «**System.Collections.Generic**« avec »**System.Collections.ObjectModel**». Cela se trouve approximativement à la ligne 424.
-   Enregistrez le fichier ProductModel.tt. Cela doit provoquer le code pour les entités d’être régénérée. Si le code ne régénère pas automatiquement, puis avec le bouton droit sur ProductModel.tt et choisissez « Exécuter un outil personnalisé ».

Si vous ouvrez le fichier Category.cs (qui est imbriqué sous ProductModel.tt), vous devez voir que la collection de produits a le type **ObservableCollection&lt;produit&gt;**.

Compilez le projet.

## <a name="lazy-loading"></a>Chargement différé

Le **produits** propriété sur le **catégorie** classe et **catégorie** propriété sur le **produit** classe sont des propriétés de navigation. Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités.

Entity Framework vous offre une option de chargement des entités connexes à partir de la base de données automatiquement la première fois que vous accédez à la propriété de navigation. Avec ce type de chargement (appelé chargement différé), n’oubliez pas que la première fois que vous accédez à chaque propriété de navigation une requête distincte sera exécutée la base de données si le contenu n’est pas déjà dans le contexte.

Lorsque vous utilisez des types d’entités POCO, EF réalise le chargement différé par la création d’instances de types de proxy dérivée pendant l’exécution, puis en remplaçant les propriétés virtuelles dans vos classes pour ajouter le raccordement de chargement. Pour obtenir le chargement différé d’objets connexes, vous devez déclarer les accesseurs Get de propriété en tant que navigation **public** et **virtuels** (**Overridable** en Visual Basic), et vous classe ne doit pas être **sealed** (**NotOverridable** en Visual Basic). Lors de la base de données à l’aide des propriétés de navigation premier sont automatiquement effectuées virtuelles pour activer le chargement différé. Dans la section de Code First que nous avons choisi créer les propriétés de navigation virtuelle pour la même raison

## <a name="bind-object-to-controls"></a>Lier des objets aux contrôles

Ajoutez les classes qui sont définies dans le modèle en tant que sources de données pour cette application WPF.

-   Double-cliquez sur **MainWindow.xaml** dans l’Explorateur de solutions pour ouvrir le formulaire principal
-   Dans le menu principal, sélectionnez **projet -&gt; ajouter une nouvelle Source de données...**
    (dans Visual Studio 2010, vous devez sélectionner **données -&gt; ajouter nouvelle Source de données...** )
-   Dans la zone Choisir un Typewindow de Source de données, sélectionnez **objet** et cliquez sur **suivant**
-   Dans le, sélectionnez la boîte de dialogue des objets de données, dérouler les **WPFwithEFSample** deux fois, puis sélectionnez **catégorie**  
    *Il est inutile de sélectionner le **produit** de source de données, car nous le verrons par le biais le **produit**de propriété sur le **catégorie** source de données*  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   Cliquez sur **terminer.**
-   La fenêtre Sources de données est ouverte en regard de la fenêtre de MainWindow.xaml *si la fenêtre Sources de données ne s’affichent pas, sélectionnez **vue -&gt; autres Windows -&gt; des Sources de données***
-   Appuyez sur l’icône d’épingle, afin de la fenêtre Sources de données ne sont pas automatique masquer. Vous devrez peut-être appuyer sur le bouton de rafraîchissement si la fenêtre a été déjà visible.

    ![DataSources](~/ef6/media/datasources.png)

-   Sélectionnez le ** catégorie ** source de données et faites-la glisser sur le formulaire.

Les éléments suivants s’est produite lorsque nous avons fait glisser cette source :

-   Le **categoryViewSource** ressource et le ** categoryDataGrid ** contrôle ont été ajoutés à XAML. Pour plus d’informations sur DataViewSources, consultez http://bea.stollnitz.com/blog/?p=387.
-   La propriété DataContext sur l’élément de grille parent a été définie sur « {StaticResource **categoryViewSource** } ».  Le **categoryViewSource** ressources servant de source de liaison externe\\élément de grille parent. Les éléments internes de la grille puis héritent de la valeur DataContext parent grille (propriété ItemsSource de la categoryDataGrid est définie sur « {Binding} »). 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Ajout d’une grille de détails

Maintenant que nous avons une grille pour afficher les catégories de nous allons ajouter une grille de détails pour afficher les produits associés.

-   Sélectionnez le ** produits ** propriété sous la ** catégorie ** source de données et faites-la glisser sur le formulaire.
    -   Le **categoryProductsViewSource** ressource et **productDataGrid** grille sont ajoutés à XAML
    -   Le chemin de liaison pour cette ressource est défini pour les produits
    -   Infrastructure de liaison de données WPF permet de s’assurer que seuls les produits liés à la catégorie sélectionnée apparaissent dans **productDataGrid**
-   Dans la boîte à outils, faites glisser **bouton** une session sur le formulaire. Définir le **nom** propriété **buttonSave** et **contenu** propriété **enregistrer**.

Le formulaire doit ressembler à ceci :

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Ajoutez le Code qui gère l’Interaction de données

Il est temps d’ajouter des gestionnaires d’événements à la fenêtre principale.

-   Dans la fenêtre XAML, cliquez sur le  **&lt;fenêtre** élément, cette opération sélectionne la fenêtre principale
-   Dans le **propriétés** fenêtre choisissez **événements** en haut à droite, puis double-cliquez sur la zone de texte à droite de la **Loaded** étiquette

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   Également ajouter le **cliquez sur** événement pour le **enregistrer** bouton en double-cliquant sur le bouton Enregistrer dans le concepteur. 

Cela vous amène à du code-behind pour le formulaire, nous allons maintenant modifier le code pour utiliser le ProductContext pour accéder aux données. Mettre à jour le code pour le MainWindow comme indiqué ci-dessous.

Le code déclare une instance d’exécution longue de **ProductContext**. Le **ProductContext** objet est utilisé pour interroger et enregistrer les données dans la base de données. Le **Dispose**() sur le **ProductContext** instance est ensuite appelée à partir de l’élément substitué **OnClosing** (méthode). Les commentaires du code fournissent des informations sur ce que fait le code.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Tester l’Application WPF

-   Compilez et exécutez l'application. Si vous avez utilisé un Code First, vous verrez qu’un **WPFwithEFSample.ProductContext** base de données est créée pour vous.
-   Entrez un nom de catégorie dans les noms de produit et de la grille supérieures dans la grille inférieure *n’entrez pas quoi que ce soit dans les colonnes ID, car la clé primaire est générée par la base de données*

    ![Screen1](~/ef6/media/screen1.png)

-   Appuyez sur la **enregistrer** bouton pour enregistrer les données dans la base de données

Après l’appel à du DbContext **SaveChanges**(), les ID sont remplies avec les valeurs de la base de données générée. Étant donné que nous avons appelé **Actualiser**() après **SaveChanges**() le **DataGrid** contrôles sont mis à jour avec les nouvelles valeurs également.

![Écran2](~/ef6/media/screen2.png)
