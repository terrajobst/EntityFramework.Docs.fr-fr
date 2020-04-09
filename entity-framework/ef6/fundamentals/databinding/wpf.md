---
title: Databinding avec WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639142"
---
> [!IMPORTANT]
> **Ce document est valable pour WPF sur le cadre .NET seulement**
>
> Ce document décrit la databinding pour WPF sur le cadre .NET. Pour les nouveaux projets .NET Core, nous vous recommandons d’utiliser [EF Core](/ef/core) au lieu de Entity Framework 6. La documentation relative à la databinding dans EF Core est suivie dans [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).

# <a name="databinding-with-wpf"></a>Liaison de données avec WPF
Cette procédure pas à pas étape étape montre comment lier les types d’OCO aux contrôles du FMM sous une forme de « master-detail ». L’application utilise les API du Cadre d’entité pour remplir les objets avec les données de la base de données, suivre les modifications et les données persistantes à la base de données.

Le modèle définit deux types qui participent à une relation\\unique : **Catégorie** (maître principal) et **Produit** (détail dépendant).\\ Ensuite, les outils Visual Studio sont utilisés pour lier les types définis dans le modèle aux commandes WPF. Le cadre de liaison de données WPF permet la navigation entre les objets apparentés : la sélection des lignes dans la vue principale provoque la mise à jour de la vue détaillée avec les données correspondantes de l’enfant.

Les captures d’écran et les listes de code dans cette procédure pas à pas sont prises à partir de Visual Studio 2013, mais vous pouvez compléter cette procédure pas à pas avec Visual Studio 2012 ou Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Utilisez l’option «Objet» pour créer des sources de données WPF

Avec la version précédente de Entity Framework, nous avions l’habitude de recommander d’utiliser l’option **base de données** lors de la création d’une nouvelle source de données basée sur un modèle créé avec le concepteur EF. C’est parce que le concepteur générerait un contexte qui dérive des classes ObjectContext et entité qui provenaient de EntityObject. L’utilisation de l’option Base de données vous aiderait à écrire le meilleur code pour interagir avec cette surface API.

Les EF Designers for Visual Studio 2012 et Visual Studio 2013 génèrent un contexte qui dérive de DbContext avec de simples classes d’entités POCO. Avec Visual Studio 2010, nous vous recommandons de passer à un modèle de génération de code qui utilise DbContext comme décrit plus tard dans cette procédure pas à pas.

Lorsque vous utilisez la surface DbContext API, vous devez utiliser **l’option Objet** lors de la création d’une nouvelle source de données, comme indiqué dans cette procédure pas à pas.

Si nécessaire, vous pouvez [revenir à la génération de code basée sur ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pour les modèles créés avec le concepteur EF.

## <a name="pre-requisites"></a>Prérequis

Vous devez avoir Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 installé pour compléter cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également installer NuGet. Pour plus d’informations, voir [Installer NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Création de l’application

-   Ouvrez Visual Studio.
-   **Dossier&gt; -&gt; Nouveau - Projet....**
-   Sélectionnez **Windows** dans le volet gauche et **WPFApplication** dans le volet droit
-   Entrez **WPFwithEFSample** comme nom
-   Sélectionnez **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Installer le paquet Entity Framework NuGet

-   Dans Solution Explorer, cliquez à droite sur le projet **WinFormswithEFSample**
-   Sélectionnez **Gérer les forfaits NuGet...**
-   Dans le dialogue Manage NuGet Packages, Sélectionnez l’onglet **En ligne** et choisissez le forfait **EntityFramework**
-   Cliquez **sur Installation**  
    >[!NOTE]
    > En plus de l’assemblage EntityFramework une référence à System.ComponentModel.DataAnnotations est également ajoutée. Si le projet a une référence à System.Data.Entity, alors il sera supprimé lorsque le paquet EntityFramework est installé. L’assemblage System.Data.Entity n’est plus utilisé pour les applications Entity Framework 6.

## <a name="define-a-model"></a>Définir un modèle

Dans cette procédure pas à pas, vous pouvez choisir de mettre en œuvre un modèle à l’aide de Code First ou de l’EF Designer. Complétez l’une des deux sections suivantes.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1 : Définir un modèle à l’aide du code d’abord

Cette section montre comment créer un modèle et sa base de données associée à l’aide de Code First. Passer à la section suivante **(Option 2 : Définir un modèle à l’aide de Database First)** si vous préférez utiliser Database First pour inverser l’ingénierie de votre modèle à partir d’une base de données à l’aide du concepteur EF

Lorsque vous utilisez code d’abord, vous commencez généralement par écrire des classes-cadres .NET qui définissent votre modèle conceptuel (domaine).

-   Ajoutez une nouvelle classe au **WPFwithEFSample :**
    -   Clic droit sur le nom du projet
    -   Sélectionnez **Ajouter,** puis **Nouvel article**
    -   Sélectionnez **classe** et entrez **le produit** pour le nom de classe
-   Remplacer la définition de la classe **de produit** par le code suivant :

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

La propriété **Produits** sur la catégorie **catégorie** et la propriété **de catégorie** sur la catégorie de **produit** sont des propriétés de navigation. Dans Entity Framework, les propriétés de navigation permettent de naviguer dans une relation entre deux types d’entités.

En plus de définir des entités, vous devez définir une classe qui dérive&lt;de&gt; DbContext et expose les propriétés DbSet TEntity. Les propriétés&lt;DbSet TEntity&gt; permettent au contexte de savoir quels types vous souhaitez inclure dans le modèle.

Une instance du type dérivé DbContext gère les objets de l’entité pendant le temps d’exécution, qui comprend le peuplement d’objets avec des données à partir d’une base de données, le suivi des modifications et la persistance des données à la base de données.

-   Ajoutez une nouvelle classe **ProductContext** au projet avec la définition suivante :

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

### <a name="option-2-define-a-model-using-database-first"></a>Option 2 : Définir un modèle à l’aide de Database First

Cette section montre comment utiliser Database First pour inverser l’ingénierie de votre modèle à partir d’une base de données à l’aide du concepteur EF. Si vous avez terminé la section précédente **(Option 1 : Définissez un modèle à l’aide du code d’abord),** alors sautez cette section et allez directement à la section **Chargement paresseux.**

#### <a name="create-an-existing-database"></a>Créer une base de données existante

Typiquement, lorsque vous ciblez une base de données existante, il sera déjà créé, mais pour ce pas, nous devons créer une base de données pour y accéder.

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé:

-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.
-   Si vous utilisez Visual Studio 2012, vous créerez une base de données [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Allons-y et générons la base de données.

-   **Afficher&gt; - Server Explorer**
-   Cliquez à droite sur **les connexions données -&gt; Ajouter la connexion...**
-   Si vous n’avez pas connecté à une base de données de Server Explorer avant d’avoir besoin de sélectionner Microsoft SQL Server comme source de données

    ![Changer la source de données](~/ef6/media/changedatasource.png)

-   Connectez-vous à LocalDB ou SQL Express, selon celui que vous avez installé, et entrez **produits** comme nom de base de données

    ![Ajouter la connexion LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Ajouter Connection Express](~/ef6/media/addconnectionexpress.png)

-   Sélectionnez **OK** et vous serez demandé si vous voulez créer une nouvelle base de données, sélectionnez **Oui**

    ![Créer une base de données](~/ef6/media/createdatabase.png)

-   La nouvelle base de données apparaîtra désormais dans Server Explorer, cliquez à droite dessus et sélectionnez **New Requête**
-   Copiez la SQL suivante dans la nouvelle requête, puis cliquez à droite sur la requête et **sélectionnez Exécuter**

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

#### <a name="reverse-engineer-model"></a>Modèle d’ingénieur inverse

Nous allons faire usage de Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.

-   **Projet&gt; - Ajouter un nouvel article...**
-   Sélectionnez les **données** du menu gauche, puis **ADO.NET modèle de données d’entité**
-   Entrez **ProductModel** comme nom et cliquez sur **OK**
-   Cela lance **l’Entité Data Model Wizard**
-   Sélectionnez **Générer dans la base de données** et cliquez sur **Next**

    ![Choisir le contenu du modèle](~/ef6/media/choosemodelcontents.png)

-   Sélectionnez la connexion à la base de données que vous avez créée dans la première section, entrez **ProductContext** comme nom de la chaîne de connexion et cliquez sur **Next**

    ![Choisissez votre connexion](~/ef6/media/chooseyourconnection.png)

-   Cliquez sur la case à cocher à côté de 'Tables' pour importer toutes les tables et cliquez sur 'Finish'

    ![Choisissez vos objets](~/ef6/media/chooseyourobjects.png)

Une fois le processus d’ingénieur inversé terminé, le nouveau modèle est ajouté à votre projet et ouvert à vous pour le visualiser dans le concepteur de cadre d’entité. Un fichier App.config a également été ajouté à votre projet avec les détails de connexion de la base de données.

#### <a name="additional-steps-in-visual-studio-2010"></a>Étapes supplémentaires dans Visual Studio 2010

Si vous travaillez dans Visual Studio 2010, vous devrez mettre à jour le concepteur EF pour utiliser la génération de code EF6.

-   Cliquez à droite sur une tache vide de votre modèle dans le concepteur EF et sélectionnez **Ajouter l’élément de génération de code...**
-   Sélectionnez **des modèles en ligne** dans le menu gauche et recherchez **DbContext**
-   Sélectionnez le **générateur EF 6.x\#DbContext pour C ,** entrez **ProductsModel** comme nom et cliquez sur Ajouter

#### <a name="updating-code-generation-for-data-binding"></a>Mise à jour de la génération de code pour la liaison de données

EF génère du code à partir de votre modèle à l’aide de modèles T4. Les modèles expédiés avec Visual Studio ou téléchargés à partir de la galerie Visual Studio sont destinés à une utilisation générale. Cela signifie que les entités générées à&lt;partir&gt; de ces modèles ont des propriétés ICollection T simples. Toutefois, lorsque vous effectuez des liaisons de données à l’aide de WPF, il est souhaitable d’utiliser **ObservableCollection** pour les propriétés de collecte afin que WPF puisse suivre les modifications apportées aux collections. À cette fin, nous allons modifier les modèles pour utiliser ObservableCollection.

-   Ouvrez le **fichier Solution Explorer** et trouvez le fichier **ProductModel.edmx**
-   Trouvez le fichier **ProductModel.tt** qui sera imbriqué sous le fichier ProductModel.edmx

    ![Modèle de modèle de produit WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Double-clic sur le fichier ProductModel.tt pour l’ouvrir dans l’éditeur Visual Studio
-   Trouver et remplacer les deux occurrences de "**ICollection**" par "**ObservableCollection**". Celles-ci sont situées approximativement aux lignes 296 et 484.
-   Trouver et remplacer la première occurrence de "**HashSet**" par "**ObservableCollection**". L’accident est situé à peu près à la ligne 50. **Ne remplacez pas** la deuxième occurrence de HashSet trouvée plus tard dans le code.
-   Trouver et remplacer la seule occurrence de "**System.Collections.Generic**" par "**System.Collections.ObjectModel**". Il est situé à peu près à la ligne 424.
-   Enregistrez le fichier ProductModel.tt. Cela devrait entraîner la régénéra urgénérité du code pour les entités. Si le code ne se régénère pas automatiquement, cliquez à droite sur ProductModel.tt et choisissez "Run Custom Tool".

Si vous ouvrez maintenant le fichier Category.cs (qui est imbriqué sous ProductModel.tt), alors vous devriez voir que la collection de produits a le type **De produit&lt;&gt;ObservableCollection**.

Compilez le projet.

## <a name="lazy-loading"></a>Chargement différé

La propriété **Produits** sur la catégorie **catégorie** et la propriété **de catégorie** sur la catégorie de **produit** sont des propriétés de navigation. Dans Entity Framework, les propriétés de navigation permettent de naviguer dans une relation entre deux types d’entités.

EF vous donne automatiquement la possibilité de charger des entités apparentées à partir de la base de données la première fois que vous accédez à la propriété de navigation. Avec ce type de chargement (appelé chargement paresseux), sachez que la première fois que vous accédez à chaque propriété de navigation une requête distincte sera exécutée contre la base de données si le contenu n’est pas déjà dans le contexte.

Lors de l’utilisation des types d’entités POCO, EF réalise le chargement paresseux en créant des instances de types de proxy dérivés pendant le temps d’exécution, puis en dominant les propriétés virtuelles dans vos classes pour ajouter le crochet de chargement. Pour obtenir le chargement paresseux des objets connexes, vous devez déclarer les getters de propriété de navigation comme **publics** et **virtuels** **(Overridable** dans Visual Basic), et votre classe ne doit pas être **scellée** **(NotOverridable** dans Visual Basic). Lors de l’utilisation des propriétés de navigation Database First sont automatiquement rendus virtuels pour permettre le chargement paresseux. Dans la section Code First, nous avons choisi de rendre les propriétés de navigation virtuelles pour la même raison

## <a name="bind-object-to-controls"></a>Bind Object to Controls

Ajoutez les classes qui sont définies dans le modèle comme sources de données pour cette application WPF.

-   Double-clic **MainWindow.xaml** dans Solution Explorer pour ouvrir la forme principale
-   Dans le menu principal, sélectionnez **Projet -&gt; Ajouter de nouvelles sources de données ...**
    (dans Visual Studio 2010, vous devez sélectionner **des données -&gt; Ajouter de nouvelles sources de données...**)
-   Dans le Choisir un typewindow source de données, sélectionnez **l’objet** et cliquez sur **Next**
-   Dans le dialogue Select the Data Objects, dépliez le **WPFwithEFSample** deux fois et sélectionnez **catégorie**  
    *Il n’est pas nécessaire de sélectionner la source de données **du produit,** car nous y arriverons par la propriété du **produit**sur la source de données de la **catégorie***  

    ![Sélectionner des objets de données](~/ef6/media/selectdataobjects.png)

-   Cliquez sur **Terminer**.
-   La fenêtre Data Sources est ouverte à côté de la fenêtre MainWindow.xaml *Si la fenêtre Data Sources n’est pas disponible, **sélectionnez Vue -&gt; Autres sources de données Windows-&gt; ** *
-   Appuyez sur l’icône de la broche, de sorte que la fenêtre Data Sources ne se cache pas automatiquement. Vous devrez peut-être appuyer sur le bouton de rafraîchissement si la fenêtre était déjà visible.

    ![Sources de données](~/ef6/media/datasources.png)

-   Sélectionnez la source de données **de catégorie** et faites-la glisser sur le formulaire.

Ce qui suit s’est produit lorsque nous avons traîné cette source:

-   La **ressource De catégorieViewSource** et le contrôle **de la catégorieDataGrid** ont été ajoutés à XAML 
-   La propriété DataContext sur l’élément grille parente a été définie à « 'StaticResource **categoryViewSource** '.".La **ressource de la catégorieViewSource** sert\\de source contraignante pour l’élément réseau parent externe. Les éléments de grille interne héritent alors de la valeur DataContext de la grille mère (la propriété ItemsSource de la catégorieDataGrid est définie à « Liaison »)

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

Maintenant que nous avons une grille pour afficher les catégories, ajoutons une grille de détails pour afficher les produits associés.

-   Sélectionnez la propriété **Produits** dans le cadre de la source de données **de catégorie** et faites-la glisser sur le formulaire.
    -   La **catégorieProductsViewSource** ressources et **produitDataGrid** grille sont ajoutées à XAML
    -   La voie contraignante de cette ressource est définie pour Les produits
    -   Le cadre de liaison des données WPF garantit que seuls les produits liés à la catégorie sélectionnée apparaissent dans **productDataGrid**
-   De la boîte à outils, faites glisser **Button** sur le formulaire. Définissez la propriété **Nom** à **buttonSave** et la propriété **Content** à **enregistrer**.

Le formulaire doit ressembler à ceci :

![Concepteur](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Ajouter du code qui gère l’interaction des données

Il est temps d’ajouter des gestionnaires d’événements à la fenêtre principale.

-   Dans la fenêtre XAML, cliquez sur l’élément ** &lt;Fenêtre,** ce qui sélectionne la fenêtre principale
-   Dans la fenêtre **Propriétés** choisir **Événements** en haut à droite, puis double-cliquez sur la boîte de texte à droite de l’étiquette **chargée**

    ![Propriétés de fenêtre principale](~/ef6/media/mainwindowproperties.png)

-   Ajoutez également l’événement **Cliquez** pour le bouton **Enregistrer** en cliquant doublement sur le bouton Enregistrer dans le concepteur. 

Cela vous amène au code derrière pour le formulaire, nous allons maintenant modifier le code pour utiliser le ProductContext pour effectuer l’accès aux données. Mettre à jour le code du MainWindow tel qu’il est indiqué ci-dessous.

Le code déclare une instance de longue date de **ProductContext**. **L’objet ProductContext** est utilisé pour interroger et enregistrer des données dans la base de données. Le **dispos ()** sur l’instance **ProductContext** est alors appelé à partir de la méthode **OnClosing** dépassée.Les commentaires de code fournissent des détails sur ce que fait le code.

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

## <a name="test-the-wpf-application"></a>Testez l’application WPF

-   Compilez et exécutez l'application. Si vous avez utilisé Code First, vous verrez qu’une base de données **WPFwithEFSample.ProductContext** est créée pour vous.
-   Entrez un nom de catégorie dans la grille supérieure et les noms de produits dans la grille inférieure *N’entrez rien dans les colonnes d’identification, parce que la clé principale est générée par la base de données*

    ![Fenêtre principale avec de nouvelles catégories et produits](~/ef6/media/screen1.png)

-   Appuyez sur le bouton **Enregistrer** pour enregistrer les données dans la base de données

Après l’appel à **SaveChanges**DbContext() , les ID sont peuplés avec les valeurs générées par la base de données. Parce que nous avons appelé **Refresh()** après **SaveChanges ()** les contrôles **DataGrid** sont mis à jour avec les nouvelles valeurs ainsi.

![Fenêtre principale avec DIU peuplé](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Ressources supplémentaires

Pour en savoir plus sur la liaison de données aux collections à l’aide de WPF, consultez [ce sujet](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) dans la documentation WPF.  
