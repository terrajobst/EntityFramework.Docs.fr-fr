---
title: Liaison de données avec WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 071172810f7dac45f42aca0efa7f329bac31e9cd
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251191"
---
# <a name="databinding-with-winforms"></a>Liaison de données avec WinForms
Cette procédure pas à pas montre comment lier les types POCO aux contrôles Windows Forms (WinForms) dans un formulaire « maître / détail ». L’application utilise Entity Framework pour remplir des objets avec des données à partir de la base de données, le suivi des modifications et conserver les données dans la base de données.

Le modèle définit deux types impliquées dans une relation un-à-plusieurs : catégorie (principal\\master) et Product (dépendants\\détail). Ensuite, les outils de Visual Studio sont utilisés pour lier les types définis dans le modèle pour les contrôles WinForms. L’infrastructure de liaison de données WinForms permet la navigation entre les objets connexes : sélection de lignes dans la vue principale, la vue de détail mettre à jour avec les données enfants correspondantes.

Les captures d’écran et les listings de code dans cette procédure pas à pas sont effectuées à partir de Visual Studio 2013, mais vous pouvez effectuer cette procédure pas à pas avec Visual Studio 2012 ou Visual Studio 2010.

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 est installé pour terminer cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également installer le package NuGet. Pour plus d’informations, consultez [installation NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Création de l’application

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le volet gauche et **Windows FormsApplication** dans le volet droit
-   Entrez **WinFormswithEFSample** comme nom
-   Sélectionnez **OK**.

## <a name="install-the-entity-framework-nuget-package"></a>Installez le package NuGet Entity Framework

-   Dans l’Explorateur de solutions, cliquez sur le **WinFormswithEFSample** projet
-   Sélectionnez **gérer les Packages NuGet...**
-   Dans la boîte de dialogue Gérer les Packages NuGet, sélectionnez le **Online** onglet et sélectionnez le **EntityFramework** package
-   Cliquez sur **installer**  
    > [!NOTE]
    > En plus de l’assembly EntityFramework une référence à System.ComponentModel.DataAnnotations est également ajoutée. Si le projet a une référence à System.Data.Entity, puis elle va être supprimée lorsque le package EntityFramework est installé. L’assembly System.Data.Entity est n’est plus utilisé pour les applications Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implémentation IListSource pour les Collections

Propriétés de la collection doivent implémenter l’interface IListSource pour permettre la liaison de données bidirectionnelle avec tri lors de l’utilisation de Windows Forms. Pour ce faire, nous allons étendre ObservableCollection pour ajouter des fonctionnalités IListSource.

-   Ajouter un **ObservableListSource** classe au projet :
    -   Avec le bouton droit sur le nom du projet
    -   Sélectionnez **Add -&gt; un nouvel élément**
    -   Sélectionnez **classe** et entrez **ObservableListSource** pour le nom de classe
-   Remplacez le code généré par défaut par le code suivant :

*Cette classe permet de données bidirectionnelle de liaison ainsi que de tri. La classe dérive de ObservableCollection&lt;T&gt; et ajoute une implémentation explicite de IListSource. La méthode GetList() de IListSource est implémentée pour retourner une implémentation IBindingList qui reste synchronisée avec ObservableCollection. L’implémentation IBindingList générée par ToBindingList prend en charge le tri. La méthode d’extension ToBindingList est définie dans l’assembly EntityFramework.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Définir un modèle

Dans cette procédure pas à pas, que vous pouvez choisi d’implémenter un modèle à l’aide de Code First ou le Concepteur EF. Effectuez l’une des deux sections suivantes.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1 : Définir un modèle à l’aide de Code First

Cette section montre comment créer un modèle et sa base de données associée à l’aide de Code First. Passez à la section suivante (**Option 2 : définir un modèle à l’aide de la première base de données)** si vous préférez utiliser Database First pour inverser concevoir votre modèle à partir d’une base de données à l’aide du Concepteur EF

Lors de l’utilisation du développement Code First vous commencez généralement par écriture de classes .NET Framework qui définissent votre modèle conceptuel (domaine).

-   Ajouter un nouveau **produit** classe au projet
-   Remplacez le code généré par défaut par le code suivant :

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Ajouter un **catégorie** classe au projet.
-   Remplacez le code généré par défaut par le code suivant :

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Outre la définition des entités, vous devez définir une classe qui dérive de **DbContext** et expose **DbSet&lt;TEntity&gt;**  propriétés. Le **DbSet** propriétés permettent le contexte de connaître les types que vous souhaitez inclure dans le modèle. Le **DbContext** et **DbSet** types sont définis dans l’assembly EntityFramework.

Une instance du type DbContext dérivée gère les objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, modifier le suivi et la persistance des données à la base de données.

-   Ajouter un nouveau **ProductContext** classe au projet.
-   Remplacez le code généré par défaut par le code suivant :

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![Modifier la source de données](~/ef6/media/changedatasource.png)

-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé, puis entrez **produits** en tant que le nom de la base de données

    ![Ajouter la connexion base de données locale](~/ef6/media/addconnectionlocaldb.png)

    ![Ajouter des connexions Express](~/ef6/media/addconnectionexpress.png)

-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**

    ![Créer une base de données](~/ef6/media/createdatabase.png)

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

    ![Choisir votre connexion](~/ef6/media/chooseyourconnection.png)

-   Cliquez sur la case à cocher en regard de « Tables » pour importer toutes les tables, cliquez sur « Terminer »

    ![Choisir vos objets de](~/ef6/media/chooseyourobjects.png)

Une fois que le processus d’ingénierie à rebours est terminé le nouveau modèle est ajouté à votre projet et ouvert pour l’afficher dans le Concepteur d’Entity Framework. Un fichier App.config a également été ajouté à votre projet avec les détails de connexion pour la base de données.

#### <a name="additional-steps-in-visual-studio-2010"></a>Étapes supplémentaires dans Visual Studio 2010

Si vous travaillez dans Visual Studio 2010 vous devrez mettre à jour le Concepteur EF pour utiliser la génération de code EF6.

-   Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**
-   Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**
-   Sélectionnez le **EF 6.x générateur DbContext pour C\#,** entrez **ProductsModel** comme nom et cliquez sur Ajouter

#### <a name="updating-code-generation-for-data-binding"></a>Mise à jour de la génération de code pour la liaison de données

Entity Framework génère du code à partir de votre modèle à l’aide de modèles T4. Les modèles fournis avec Visual Studio ou téléchargé à partir de la galerie Visual Studio sont destinés à usage général. Cela signifie que les entités générées à partir de ces modèles ont ICollection simple&lt;T&gt; propriétés. Toutefois, lors de la liaison de données, il est souhaitable d’avoir des propriétés de collection qui implémentent IListSource. C’est pourquoi nous avons créé la classe ObservableListSource ci-dessus et nous allons maintenant modifier les modèles s’utiliser de cette classe.

-   Ouvrez le **l’Explorateur de solutions** et recherchez **ProductModel.edmx** fichier
-   Rechercher la **ProductModel.tt** fichier qui doit être imbriqué sous le fichier ProductModel.edmx

    ![Modèle de produit](~/ef6/media/productmodeltemplate.png)

-   Double-cliquez sur le fichier ProductModel.tt pour l’ouvrir dans l’éditeur Visual Studio
-   Rechercher et remplacer les deux occurrences de «**ICollection**« avec »**ObservableListSource**». Ceux-ci sont situés dans des lignes environ 296 et 484.
-   Rechercher et remplacer la première occurrence de «**HashSet**« avec »**ObservableListSource**». Cet événement se trouve environ à la ligne 50. **Ne le faites pas** remplacer la deuxième occurrence de HashSet figure plus loin dans le code.
-   Enregistrez le fichier ProductModel.tt. Cela doit provoquer le code pour les entités d’être régénérée. Si le code ne régénère pas automatiquement, puis avec le bouton droit sur ProductModel.tt et choisissez « Exécuter un outil personnalisé ».

Si vous ouvrez le fichier Category.cs (qui est imbriqué sous ProductModel.tt), vous devez voir que la collection de produits a le type **ObservableListSource&lt;produit&gt;**.

Compilez le projet.

## <a name="lazy-loading"></a>Chargement différé

Le **produits** propriété sur le **catégorie** classe et **catégorie** propriété sur le **produit** classe sont des propriétés de navigation. Dans Entity Framework, les propriétés de navigation permettent de naviguer d’une relation entre deux types d’entités.

Entity Framework vous offre une option de chargement des entités connexes à partir de la base de données automatiquement la première fois que vous accédez à la propriété de navigation. Avec ce type de chargement (appelé chargement différé), n’oubliez pas que la première fois que vous accédez à chaque propriété de navigation une requête distincte sera exécutée la base de données si le contenu n’est pas déjà dans le contexte.

Lorsque vous utilisez des types d’entités POCO, EF réalise le chargement différé par la création d’instances de types de proxy dérivée pendant l’exécution, puis en remplaçant les propriétés virtuelles dans vos classes pour ajouter le raccordement de chargement. Pour obtenir le chargement différé d’objets connexes, vous devez déclarer les accesseurs Get de propriété en tant que navigation **public** et **virtuels** (**Overridable** en Visual Basic), et vous classe ne doit pas être **sealed** (**NotOverridable** en Visual Basic). Lors de la base de données à l’aide des propriétés de navigation premier sont automatiquement effectuées virtuelles pour activer le chargement différé. Dans la section de Code First que nous avons choisi créer les propriétés de navigation virtuelle pour la même raison

## <a name="bind-object-to-controls"></a>Lier des objets aux contrôles

Ajoutez les classes qui sont définies dans le modèle en tant que sources de données pour cette application WinForms.

-   Dans le menu principal, sélectionnez **projet -&gt; ajouter une nouvelle Source de données...**
    (dans Visual Studio 2010, vous devez sélectionner **données -&gt; ajouter nouvelle Source de données...** )
-   Dans la fenêtre Choisir un Type de Source de données, sélectionnez **objet** et cliquez sur **suivant**
-   Dans le, sélectionnez la boîte de dialogue des objets de données, dérouler les **WinFormswithEFSample** deux fois, puis sélectionnez **catégorie** il est inutile de sélectionner la source de données de produit, car nous allons lui par le biais du produit propriété sur la source de données de catégorie.

    ![Source de données](~/ef6/media/datasource.png)

-   Cliquez sur **terminer.** 
     *Si la fenêtre Sources de données ne s’affichent pas, sélectionnez *** vue -&gt; autres Windows -&gt; des Sources de données**
-   Appuyez sur l’icône d’épingle, afin de la fenêtre Sources de données ne sont pas automatique masquer. Vous devrez peut-être appuyer sur le bouton de rafraîchissement si la fenêtre a été déjà visible.

    ![Source de données 2](~/ef6/media/datasource2.png)

-   Dans l’Explorateur de solutions, double-cliquez sur le **Form1.cs** fichier à ouvrir le formulaire principal dans le concepteur.
-   Sélectionnez le **catégorie** source de données et faites-la glisser sur le formulaire. Par défaut, un nouveau DataGridView (**categoryDataGridView**) et contrôles de barre d’outils de Navigation sont ajoutées au concepteur. Ces contrôles sont liés à la BindingSource (**categoryBindingSource**) et le navigateur de liaison (**categoryBindingNavigator**) les composants qui sont également créés.
-   Modifier les colonnes sur la **categoryDataGridView**. Nous souhaitons définir la **CategoryId** colonne en lecture seule. La valeur de la **CategoryId** propriété est générée par la base de données une fois que nous enregistrons les données.
    -   Le contrôle DataGridView d’avec le bouton droit et sélectionnez Modifier les colonnes...
    -   Sélectionnez la colonne CategoryId et en lecture seule la valeur True
    -   Appuyez sur OK
-   Sélectionnez les produits figurant dans la source de données de catégorie et faites-le glisser sur le formulaire. Le productDataGridView et productBindingSource sont ajoutés au formulaire.
-   Modifier les colonnes sur la productDataGridView. Nous souhaitons masquer les colonnes CategoryId et la catégorie et la valeur ProductId en lecture seule. La valeur de la propriété ProductId est générée par la base de données une fois que nous enregistrons les données.
    -   Cliquez sur le contrôle DataGridView et sélectionnez **modifier les colonnes...** .
    -   Sélectionnez le **ProductId** colonne et les jeux **ReadOnly** à **True**.
    -   Sélectionnez le **CategoryId** colonne, puis appuyez sur la **supprimer** bouton. Faire de même avec le **catégorie** colonne.
    -   Appuyez sur **OK**.

    Jusqu’ici, nous associées nos contrôles DataGridView aux composants BindingSource dans le concepteur. Dans la section suivante, nous allons ajouter code pour le code-behind pour définir categoryBindingSource.DataSource à la collection d’entités qui sont actuellement suivies par DbContext. Lorsque nous glisser-déplacer de produits sous la catégorie, le WinForms a pris en charge de configuration de la propriété productsBindingSource.DataSource à la propriété categoryBindingSource et productsBindingSource.DataMember aux produits. En raison de cette liaison, seuls les produits qui appartiennent à la catégorie actuellement sélectionnée seront affichera dans le productDataGridView.
-   Activer la **enregistrer** dans la barre d’outils de Navigation en cliquant sur le bouton droit de la souris et en sélectionnant **activé**.

    ![Concepteur de formulaires 1](~/ef6/media/form1-designer.png)

-   Ajouter le Gestionnaire d’événements pour l’enregistrement bouton en double-cliquant sur le bouton. Cela ajoute le Gestionnaire d’événements et vous permettent du code-behind pour le formulaire. Le code pour le **categoryBindingNavigatorSaveItem\_cliquez sur** Gestionnaire d’événements sera ajouté dans la section suivante.

## <a name="add-the-code-that-handles-data-interaction"></a>Ajoutez le Code qui gère l’Interaction de données

Nous allons maintenant ajouter le code pour utiliser le ProductContext pour accéder aux données. Mettre à jour le code de la fenêtre du formulaire principal comme indiqué ci-dessous.

Le code déclare une instance d’exécution longue de ProductContext. L’objet ProductContext est utilisé pour interroger et enregistrer les données dans la base de données. La méthode Dispose() sur l’instance ProductContext est ensuite appelée à partir de la méthode OnClosing substituée. Les commentaires du code fournissent des informations sur ce que fait le code.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Tester l’Application de formulaires Windows

-   Compiler et exécuter l’application et vous pouvez tester la fonctionnalité.

    ![Écran 1 avant l’enregistrement](~/ef6/media/form1beforesave.png)

-   Après avoir enregistré les clés de magasin généré sont affichées sur l’écran.

    ![Écran 1 après enregistrement](~/ef6/media/form1aftersave.png)

-   Si vous avez utilisé un Code First, vous verrez également qu’un **WinFormswithEFSample.ProductContext** base de données est créée pour vous.

    ![Explorateur d’objets Server](~/ef6/media/serverobjexplorer.png)
