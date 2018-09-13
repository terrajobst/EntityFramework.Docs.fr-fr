---
title: Model First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 8e010f95db40261073b4af80a3c0e3225a2cd1cf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490478"
---
# <a name="model-first"></a>Tout d’abord de modèle
Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Model First à l’aide d’Entity Framework. Modèle tout d’abord vous permet de créer un nouveau modèle à l’aide d’Entity Framework Designer, puis générez un schéma de base de données à partir du modèle. Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer. Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.

## <a name="watch-the-video"></a>Regardez la vidéo
Cette procédure pas à pas vidéo et pas à pas fournissent une introduction au développement Model First à l’aide d’Entity Framework. Modèle tout d’abord vous permet de créer un nouveau modèle à l’aide d’Entity Framework Designer, puis générez un schéma de base de données à partir du modèle. Le modèle est stocké dans un fichier EDMX (extension .edmx) et peut être affiché et modifié dans l’Entity Framework Designer. Les classes que vous interagissez avec dans votre application sont automatiquement générées à partir du fichier EDMX.

**Présentée par** : [Rowan Miller](http://romiller.com/)

**Vidéo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2010 ou Visual Studio 2012 est installé pour terminer cette procédure pas à pas.

Si vous utilisez Visual Studio 2010, vous devez également avoir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installé.

## <a name="1-create-the-application"></a>1. Création de l’application

Pour simplifier les choses, nous allons créer une application de console de base qui utilise le premier modèle pour accéder aux données :

-   Ouvrir Visual Studio
-   **Fichier -&gt; nouveau -&gt; projet...**
-   Sélectionnez **Windows** dans le menu de gauche et **Application Console**
-   Entrez **ModelFirstSample** comme nom
-   Sélectionnez **OK**.

## <a name="2-create-model"></a>2. Créer le modèle

Nous allons utiliser Entity Framework Designer, qui est inclus dans le cadre de Visual Studio, pour créer notre modèle.

-   **Projet -&gt; ajouter un nouvel élément...**
-   Sélectionnez **données** dans le menu de gauche, puis **ADO.NET Entity Data Model**
-   Entrez **BloggingModel** comme nom et cliquez sur **OK**, cette action lance l’Assistant Entity Data Model
-   Sélectionnez **modèle vide** et cliquez sur **terminer**

    ![Créer le modèle vide](~/ef6/media/createemptymodel.png)

Entity Framework Designer s’ouvre avec un modèle vide. Maintenant, nous pouvons commencer l’ajout d’entités, les propriétés et les associations au modèle.

-   Avec le bouton droit sur l’aire de conception et sélectionnez **propriétés**
-   Dans la modification de la fenêtre Propriétés la **Nom_conteneur_entités** à **BloggingContext**
    *il s’agit du nom de contexte dérivé qui sera généré pour vous, le contexte représente une session avec la base de données, ce qui nous permet d’interroger et d’enregistrer des données*
-   Avec le bouton droit sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; entité...**
-   Entrez **Blog** en tant que le nom de l’entité et **BlogId** comme le nom de la clé et cliquez sur **OK**

    ![Ajouter une entité de Blog](~/ef6/media/addblogentity.png)

-   Avec le bouton droit sur la nouvelle entité sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**, entrez **nom** comme nom de la propriété.
-   Répétez ce processus pour ajouter un **Url** propriété.
-   Avec le bouton droit sur le **Url** propriété sur l’aire de conception et sélectionnez **propriétés**, lors de la modification de la fenêtre Propriétés la **Nullable** à **True** 
     *Cela nous permet d’enregistrer un Blog à la base de données sans lui assigner une Url*
-   En utilisant les techniques que vous venez d’apprendre, d’ajouter un **Post** entité avec une **IDMessage** propriété de clé
-   Ajouter **titre** et **contenu** propriétés scalaires à le **Post** entité

Maintenant que nous avons deux entités, il est temps d’ajouter une association (ou la relation) entre eux.

-   Avec le bouton droit sur l’aire de conception et sélectionnez **Ajouter nouveau -&gt; Association...**
-   Qu’une extrémité de la relation pointent vers **Blog** avec une multiplicité de **un** et le point de terminaison à **Post** avec une multiplicité de **nombreux** 
     *Cela signifie qu’un Blog a de nombreuses publications et une publication appartient à un Blog*
-   Vérifiez le **ajouter des propriétés de clé étrangère à l’entité 'Post'** case est activée et cliquez sur **OK**

    ![Ajouter une Association MF](~/ef6/media/addassociationmf.png)

Nous disposons désormais d’un modèle simple que nous pouvons générer une base de données et l’utiliser pour lire et écrire des données.

![Modèle Initial](~/ef6/media/modelinitial.png)

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

    ![Modèle de DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Génération de la base de données

Compte tenu de notre modèle, Entity Framework peut calculer un schéma de base de données qui nous permettra de stocker et récupérer des données à l’aide du modèle.

Le serveur de base de données qui est installé avec Visual Studio est différent selon la version de Visual Studio que vous avez installé :

-   Si vous utilisez Visual Studio 2010, vous créerez une base de données SQL Express.
-   Si vous utilisez Visual Studio 2012, vous créerez un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de données.

Procédons à générer la base de données.

-   Avec le bouton droit sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle...**
-   Cliquez sur **nouvelle connexion...** et spécifiez la base de données locale ou de SQL Express, selon la version de Visual Studio que vous utilisez, entrez **ModelFirst.Blogging** en tant que le nom de la base de données.

    ![Connexion de base de données locale MF](~/ef6/media/localdbconnectionmf.png)

    ![Connexion SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Sélectionnez **OK** et vous demandera si vous souhaitez créer une base de données, sélectionnez **Oui**
-   Sélectionnez **suivant** et Entity Framework Designer calculera un script pour créer le schéma de base de données
-   Une fois le script s’affiche, cliquez sur **Terminer** et le script sera ajouté à votre projet et ouvert
-   Avec le bouton droit sur le script et sélectionnez **Execute**, vous devrez spécifier la base de données pour se connecter à, spécifiez LocalDB ou Express de SQL Server, selon la version de Visual Studio que vous utilisez

## <a name="4-reading--writing-data"></a>4. Lire et écrire des données

Maintenant que nous avons un modèle, il est temps de les utiliser pour accéder à des données. Les classes que nous allons utiliser pour accéder aux données sont générés automatiquement pour vous selon le fichier EDMX.

*Cette capture d’écran provient de Visual Studio 2012, si vous utilisez Visual Studio 2010 le BloggingModel.tt et les fichiers BloggingModel.Context.tt seront directement sous votre projet plutôt qu’imbriqué sous le fichier EDMX.*

![Classes générées](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Gestion des modifications du modèle

Il est maintenant temps d’apporter des modifications à notre modèle, lorsque nous apporter ces modifications, que nous devons également mettre à jour le schéma de base de données.

Nous allons commencer en ajoutant une nouvelle entité d’utilisateur à notre modèle.

-   Ajouter un nouveau **utilisateur** nom de l’entité avec **nom d’utilisateur** comme nom de clé et **chaîne** comme type de propriété pour la clé

    ![Ajouter une entité d’utilisateur](~/ef6/media/adduserentity.png)

-   Avec le bouton droit sur le **nom d’utilisateur** propriété sur l’aire de conception et sélectionnez **propriétés**, changement de fenêtre dans les propriétés le **MaxLength** à **50 ** 
     *Cela limite les données qui peuvent être stockées dans le nom d’utilisateur à 50 caractères*
-   Ajouter un **DisplayName** propriété scalaire à la **utilisateur** entité

Nous disposons désormais d’un modèle mis à jour et nous sommes prêts à mettre à jour de la base de données pour prendre en charge de notre nouveau type d’entité utilisateur.

-   Avec le bouton droit sur l’aire de conception et sélectionnez **générer la base de données à partir du modèle...** , Entity Framework calculera un script pour recréer un schéma basé sur le modèle mis à jour.
-   Cliquez sur **terminer**
-   Vous pouvez recevoir des avertissements relatifs à remplacer le script DDL existant et les parties de stockage et de mappage du modèle, cliquez sur **Oui** pour ces deux avertissements
-   Le script SQL mis à jour pour créer la base de données est ouvert pour vous  
    *Le script généré supprime toutes les tables existantes et puis recréer le schéma à partir de zéro. Cela peut fonctionner pour un développement local, mais n’est pas viable pour l’envoi des modifications à une base de données qui a déjà été déployé. Si vous avez besoin publier les modifications dans une base de données qui a déjà été déployé, vous devez modifier le script ou utiliser un outil de comparaison de schéma pour calculer un script de migration.*
-   Avec le bouton droit sur le script et sélectionnez **Execute**, vous devrez spécifier la base de données pour se connecter à, spécifiez LocalDB ou Express de SQL Server, selon la version de Visual Studio que vous utilisez

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons étudié développement Model First, qui nous a permis de créer un modèle dans le Concepteur EF et puis générer une base de données à partir de ce modèle. Ensuite, nous avons utilisé le modèle pour lire et écrire des données à partir de la base de données. Enfin, nous mis à jour le modèle et puis recréer le schéma de base de données pour faire correspondre le modèle.
