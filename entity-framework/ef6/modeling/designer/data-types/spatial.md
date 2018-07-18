---
title: Spatial - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121325"
---
# <a name="spatial---ef-designer"></a>Spatial - Entity Framework Designer
> [!NOTE]
> **EF5 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 5. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

La procédure pas à pas vidéo et pas à pas montre comment mapper des types de données spatiales avec Entity Framework Designer. Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.

Cette procédure pas à pas utilise Model First pour créer une nouvelle base de données, mais le Concepteur EF peut également servir avec la [Database First](~/ef6/modeling/designer/workflows/database-first.md) flux de travail pour mapper à une base de données existante.

Prise en charge de type spatial a été introduite dans Entity Framework 5. Notez que pour utiliser les nouvelles fonctionnalités telles que le type spatial, les enums et les fonctions table, vous devez cibler .NET Framework 4.5. Visual Studio 2012 cible .NET 4.5 par défaut.

Pour utiliser les types de données spatiales, vous devez également utiliser un fournisseur Entity Framework qui prend en charge spatiale. Consultez [prise en charge de fournisseur pour les types spatiaux](~/ef6/fundamentals/providers/spatial-support.md) pour plus d’informations.

Il existe deux types de données spatiales principale : geography et geometry. Le type de données geography stocke des données ellipsoïdes (par exemple, GPS de latitude et longitude coordonnées). Le type de données geometry représente un système de coordonnées euclidien (plat).

## <a name="watch-the-video"></a>Regardez la vidéo
Cette vidéo montre comment mapper des types de données spatiales avec Entity Framework Designer. Il montre également comment utiliser une requête LINQ pour rechercher une distance entre deux emplacements.

**Présenté par**: Julia Kornich

**Vidéo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Conditions préalables

Vous devez disposer de Visual Studio 2012, Ultimate, Premium, Professionnel ou Web Express edition est installé pour terminer cette procédure pas à pas.

## <a name="set-up-the-project"></a>Configurer le projet

1.  Ouvrez Visual Studio 2012
2.  Sur le **fichier** menu, pointez sur **New**, puis cliquez sur **projet**
3.  Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle
4.  Entrez **SpatialEFDesigner** en tant que le nom du projet et cliquez sur **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Créer un nouveau modèle à l’aide du Concepteur EF

1.  Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**
2.  Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles
3.  Entrez **UniversityModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**
4.  Dans la page de l’Assistant Entity Data Model, sélectionnez **modèle vide** dans la boîte de dialogue Choisir le contenu du modèle
5.  Cliquez sur **terminer**

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.

L'assistant exécute les actions suivantes :

-   Génère le fichier EnumTestModel.edmx qui définit le modèle conceptuel, le modèle de stockage et le mappage entre les deux. Définit la propriété Metadata Artifact Processing du fichier .edmx à incorporer dans l’Assembly de sortie pour les fichiers de métadonnées générées sont intégrés dans l’assembly.
-   Ajoute une référence aux assemblys suivants : EntityFramework System.ComponentModel.DataAnnotations et System.Data.Entity.
-   Crée des fichiers UniversityModel.tt et UniversityModel.Context.tt et les ajoute dans le fichier .edmx. Ces fichiers de modèle T4 génèrent le code qui définit le type DbContext dérivée et les types POCO qui mappent aux entités dans le modèle .edmx

## <a name="add-a-new-entity-type"></a>Ajouter un nouveau Type d’entité

1.  Cliquez sur une zone vide de l’aire de conception, sélectionnez **Add -&gt; entité**, s’affiche la boîte de dialogue nouvelle entité
2.  Spécifiez **University** pour le type de nommer et spécifier **UniversityID** pour le nom de la propriété de clé, conservez le type **Int32**
3.  Cliquez sur **OK**.
4.  Cliquez sur l’entité et sélectionnez **Ajouter nouveau -&gt; propriété scalaire**
5.  Renommer la nouvelle propriété à **nom**
6.  Ajouter une autre propriété scalaire et renommez-le **emplacement** ouvrir la fenêtre Propriétés et modifier le type de la nouvelle propriété à **Geography**
7.  Enregistrer le modèle et générer le projet
    > [!NOTE]
    > Lorsque vous générez, les avertissements sur les entités non mappées et les associations peuvent apparaître dans la liste d’erreurs. Vous pouvez ignorer ces avertissements, car une fois que nous avons choisi générer la base de données à partir du modèle, les erreurs disparaîtront.

## <a name="generate-database-from-model"></a>Générer la base de données à partir du modèle

Maintenant, nous pouvons générer une base de données qui est basé sur le modèle.

1.  Cliquez sur un espace vide sur l’aire de conception et sélectionnez le Concepteur d’entités **générer la base de données à partir du modèle**
2.  Choisir votre boîte de dialogue de connexion de données de l’Assistant génération de la base de données est affiché cliquez sur le **nouvelle connexion** bouton spécifier **(localdb)\\mssqllocaldb** pour le nom du serveur et  **Université** pour la base de données et cliquez sur **OK**
3.  Une boîte de dialogue vous demandant si vous souhaitez créer une base de données s’affiche, cliquez sur **Oui**.
4.  Cliquez sur **suivant** et l’Assistant Création de base de données génère le langage de définition de données (DDL) pour créer une base de données le DDL généré est affiché dans le résumé et la Note de boîte de dialogue de paramètres, le DDL ne contenant pas de définition pour un table qui mappe au type énumération
5.  Cliquez sur **Terminer** le script DDL ne s’exécute pas en cliquant sur Terminer.
6.  L’Assistant Création de base de données effectue les opérations suivantes : ouvre le **UniversityModel.edmx.sql** dans l’éditeur T-SQL génère les sections de mappage du schéma et magasin de l’EDMX fichier ajoute les informations de chaîne de connexion au fichier App.config
7.  Cliquez sur le bouton droit de la souris dans l’éditeur T-SQL et sélectionnez **Execute** établir une connexion à la boîte de dialogue serveur s’affiche, entrez les informations de connexion de l’étape 2 sur **Connect**
8.  Pour afficher le schéma généré, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server, puis sélectionnez **actualiser**

## <a name="persist-and-retrieve-data"></a>Conserver et récupérer des données

Ouvrez le fichier Program.cs dans lequel la méthode Main est définie. Ajoutez le code suivant dans la fonction Main.

Le code ajoute deux nouveaux objets de l’université au contexte. Propriétés spatiales sont initialisées à l’aide de la méthode DbGeography.FromText. Le point géographique représenté en tant que WellKnownText est transmise à la méthode. Le code enregistre ensuite les données. Ensuite, la requête LINQ qui retourne un objet de l’université où son emplacement le plus proche de l’emplacement spécifié, construction et l’exécution.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compilez et exécutez l'application. Le programme génère la sortie suivante :

```
The closest University to you is: School of Fine Art.
```

Pour afficher les données dans la base de données, avec le bouton droit sur le nom de base de données dans l’Explorateur d’objets SQL Server et sélectionnez **Actualiser**. Ensuite, cliquez sur le bouton droit de la souris sur la table et sélectionnez **afficher les données**.

## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons vu comment mapper des types de données spatiales à l’aide d’Entity Framework Designer et comment utiliser des types de données spatiales dans le code. 
