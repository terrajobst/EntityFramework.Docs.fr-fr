---
title: EF6 First - de base de données
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
caps.latest.revision: 3
ms.openlocfilehash: 17bba5fe9883a1bee0f8b9624dfa35da889e6005
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120852"
---
# <a name="database-first"></a>Tout d’abord la base de données
Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement première base de données à l’aide d’Entity Framework. Base de données tout d’abord vous permet rétroconcevoir un modèle à partir d’une base de données existante. Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer. Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo fournit une introduction au développement première base de données à l’aide d’Entity Framework. Base de données tout d’abord vous permet rétroconcevoir un modèle à partir d’une base de données existante. Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer. Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.

**Présentée par** : [Rowan Miller](http://romiller.com/)

**Vidéo**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez avoir au moins Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.

 

## <a name="1-create-an-existing-database"></a>1. Créer une base de données existante

En général, lorsque vous ciblez une base de données existante, qu'il est déjà créé, mais pour cette procédure pas à pas, nous devons créer une base de données pour accéder à.

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :

-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.
-   Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de données.

 

Procédons à générer la base de données.

-   Ouvrir Visual Studio
-   **Vue -&gt; Explorateur de serveurs**
-   Cliquez avec le bouton droit sur **des connexions de données -&gt; ajouter une connexion...**
-   Si vous n’avez pas connecté à une base de données à partir de l’Explorateur de serveurs avant que vous devez sélectionner Microsoft SQL Server comme source de données

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Se connecter à la base de données locale ou de SQL Express, en fonction de celles que vous avez installé, puis entrez **DatabaseFirst.Blogging** en tant que le nom de la base de données

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

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
```

## <a name="2-create-the-application"></a>2. Création de l’application

Pour simplifier les choses, nous allons créer une application de console de base qui utilise la première base de données pour accéder aux données :

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le menu de gauche et **Application Console**
-   Entrez **DatabaseFirstSample** comme nom
-   Sélectionnez **OK**.

 

## <a name="3-reverse-engineer-model"></a>3. Modèle d’ingénierie à rebours

Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.

-   **Projet -&gt; ajouter un nouvel élément...**
-   Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**
-   Entrez **BloggingModel** comme nom et cliquez sur **OK**
-   Cette opération lance le **Assistant Entity Data Model**
-   Sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   Sélectionnez la connexion à la base de données que vous avez créé dans la première section, entrez **BloggingContext** comme nom de la chaîne de connexion et cliquez sur **suivant**

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   Cliquez sur la case à cocher en regard de « Tables » pour importer toutes les tables, cliquez sur « Terminer »

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

Une fois que le processus d’ingénierie à rebours est terminé le nouveau modèle est ajouté à votre projet et ouvert pour l’afficher dans le Concepteur d’Entity Framework. Un fichier App.config a également été ajouté à votre projet avec les détails de connexion pour la base de données.

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Étapes supplémentaires dans Visual Studio 2010

Si vous travaillez dans Visual Studio 2010, il existe quelques étapes supplémentaires à suivre pour mettre à niveau vers la dernière version d’Entity Framework. Il est important de la mise à niveau, car il vous donne accès à une surface d’API améliorée, qui est beaucoup plus facile à utiliser, ainsi que les derniers correctifs de bogues.

Tout d’abord, nous devons obtenir la dernière version d’Entity Framework à partir de NuGet.

-   **Projet –&gt; gérer les Packages NuGet... ** 
     *Si vous n’avez pas le **gérer les Packages NuGet... ** option, vous devez installer le [version la plus récente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Sélectionnez le **Online** onglet
-   Sélectionnez le **EntityFramework** package
-   Cliquez sur **installer**

Ensuite, nous devons échanger notre modèle pour générer du code qui utilise l’API DbContext, qui a été introduit dans les versions ultérieures d’Entity Framework.

-   Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez **ajouter un élément de génération de Code...**
-   Sélectionnez **modèles en ligne** dans le menu de gauche et recherchez **DbContext**
-   Sélectionnez le EF **5.x générateur DbContext pour C\#**, entrez **BloggingModel** comme nom et cliquez sur **ajouter**

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Lire et écrire des données

Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données. Les classes que nous allons utiliser pour accéder aux données sont générés automatiquement pour vous selon le fichier EDMX.

*Cette capture d’écran provient de Visual Studio 2012, si vous utilisez Visual Studio 2010 le BloggingModel.tt et les fichiers BloggingModel.Context.tt seront directement sous votre projet plutôt qu’imbriqué sous le fichier EDMX.*

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Gestion des modifications de base de données

Il est maintenant temps d’apporter certaines modifications à notre schéma de base de données, lorsque nous apporter ces modifications, que nous devons également mettre à jour de notre modèle pour refléter ces modifications.

La première étape consiste à apporter des modifications au schéma de base de données. Nous allons ajouter une table des utilisateurs au schéma.

-   Avec le bouton droit sur le **DatabaseFirst.Blogging** dans l’Explorateur de serveurs de base de données et sélectionnez **nouvelle requête**
-   Copiez le code SQL suivant dans la nouvelle requête, puis avec le bouton droit sur la requête, puis sélectionnez **Execute**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Maintenant que le schéma est mis à jour, il est temps pour mettre à jour le modèle avec ces modifications.

-   Avec le bouton droit sur un endroit vide de votre modèle dans le Concepteur EF et sélectionnez « modèle de mise à jour à partir de la base de données... », cette action lance l’Assistant de mise à jour
-   Sous l’onglet Ajouter de la vérification de l’Assistant Mise à jour la zone en regard des Tables, cela indique que nous souhaitons ajouter les nouvelles tables à partir du schéma.
    *L’onglet de l’actualisation affiche les tables existantes dans le modèle qui sera vérifié les modifications pendant la mise à jour. Les onglets de suppression affichent toutes les tables qui ont été supprimés à partir du schéma et seront également supprimées à partir du modèle dans le cadre de la mise à jour. Les informations sur ces deux onglets sont détectées automatiquement et sont fournies à titre d’information uniquement, vous ne pouvez pas modifier les paramètres.*

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   Cliquez sur Terminer dans l’Assistant de mise à jour

 

Le modèle est désormais mis à jour pour inclure une nouvelle entité d’utilisateur qui est mappé à la table d’utilisateurs que nous avons ajouté à la base de données.

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons étudié développement Database First, qui nous a permis de créer un modèle dans le Concepteur EF basé sur une base de données existante. Ensuite, nous avons utilisé ce modèle pour lire et écrire des données à partir de la base de données. Enfin, nous avons mis à jour le modèle pour refléter les modifications que nous avons apportées au schéma de base de données.
