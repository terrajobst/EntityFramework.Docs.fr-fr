---
title: Concepteur CUD procédures stockées - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
caps.latest.revision: 3
ms.openlocfilehash: 6b6a1f843142713153fa86309ef55f9d6e804766
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120857"
---
# <a name="designer-cud-stored-procedures"></a>CUD Concepteur de procédures stockées
Cette procédure pas à pas montrent comment mapper la créer\\insérer, mettre à jour et supprimer des opérations (CUD) d’un type d’entité aux procédures stockées à l’aide d’Entity Framework Designer (Concepteur d’EF).  Par défaut, Entity Framework génère automatiquement les instructions SQL pour les opérations CUD, mais vous pouvez également mapper des procédures stockées à ces opérations.  

Notez que Code First ne prend pas en charge le mappage de procédures stockées ou fonctions. Toutefois, vous pouvez appeler des procédures stockées ou fonctions à l’aide de la méthode System.Data.Entity.DbSet.SqlQuery. Exemple :
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Considérations lors du mappage d’opérations CUD à des procédures stockées

Lorsque vous mappez les opérations CUD à des procédures stockées, les considérations suivantes s’appliquent : 

-   Si vous mappez une des opérations CUD à une procédure stockée, mapper l’ensemble d'entre eux. Si vous ne mappez pas les trois, les opérations non mappées échouera si exécutée et qu’un **UpdateException** sera levée.
-   Vous devez mapper chaque paramètre de la procédure stockée aux propriétés de l’entité.
-   Si le serveur génère la valeur de clé primaire pour la ligne insérée, vous devez mapper cette valeur à la propriété de clé de l’entité. Dans l’exemple qui suit, la **InsertPerson** procédure stockée retourne la clé primaire nouvellement créée en tant que partie du jeu de résultats de la procédure stockée. La clé primaire est mappée à la clé d’entité (**PersonID**) à l’aide de la **&lt;ajouter un Result Binding&gt;** fonctionnalité du Concepteur EF.
-   Les appels de procédure stockée sont mappés 1:1 avec les entités dans le modèle conceptuel. Par exemple, si vous implémentez une hiérarchie d’héritage dans votre modèle conceptuel et le mappage puis le CUD procédures stockées pour le **Parent** (base) et le **enfant** des entités (dérivées), l’enregistrement de la **Enfant** modifications appelle uniquement la **enfant**de procédures stockées, elle ne déclenche pas le **Parent**des appels de procédures stockées.

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

-   Ouvrez Visual Studio 2012.
-   Sélectionnez **fichier -&gt; nouveau -&gt; projet**
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Console** modèle.
-   Entrez **CUDSProcsSample** comme nom.
-   Sélectionnez **OK**.

## <a name="create-a-model"></a>Créer un modèle

-   Cliquez sur le nom du projet dans l’Explorateur de solutions, puis sélectionnez **Add -&gt; un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **CUDSProcs.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.
-   Cliquez sur **nouvelle connexion**. Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la choisir vos objets de base de données boîte de dialogue, sous le **Tables** nœud, sélectionnez le **personne** table.
-   En outre, sélectionnez les procédures stockées suivantes sous la **de procédures stockées et fonctions** nœud : **DeletePerson**, **InsertPerson**, et **UpdatePerson** . 
-   À partir de Visual Studio 2012, le Concepteur EF prend en charge l’importation en bloc des procédures stockées. Le **importation sélectionné des procédures stockées et fonctions dans le modèle d’entité** est activée par défaut. Étant donné que dans cet exemple nous avons des procédures stockées qui insérant, mettre à jour et supprimer des types d’entité, nous ne souhaitez pas les importer et sera décochez cette case à cocher. 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   Cliquez sur **Terminer**.
    Le Concepteur EF, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapper l’entité Person à des procédures stockées

-   Cliquez sur le **personne** type d’entité, puis sélectionnez **mappage de procédure stockée**.
-   Les mappages de la procédure stockée s’affichent dans le **détails de Mapping** fenêtre.
-   Cliquez sur  **&lt;Sélectionnez Insérer une fonction&gt;**.
    Le champ devient une liste déroulante des procédures stockées dans le modèle de stockage qui peut être mappé aux types d'entité dans le modèle conceptuel.
    Sélectionnez **InsertPerson** dans la liste déroulante.
-   Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent. Notez que les flèches indiquent le sens du mappage : les valeurs des propriétés sont fournies aux paramètres des procédures stockées.
-   Cliquez sur  **&lt;ajouter un Result Binding&gt;**.
-   Type **NewPersonID**, le nom du paramètre retourné par la **InsertPerson** procédure stockée. Veillez ne pas à taper les espaces de début ou de fin.
-   Appuyez sur **Entrée**.
-   Par défaut, **NewPersonID** est mappé à la clé d’entité **PersonID**. Notez qu'une flèche indique le sens du mappage : la valeur de la colonne de résultats est fournie à la propriété.

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   Cliquez sur **&lt;sélectionner un Update Function&gt;** et sélectionnez **UpdatePerson** dans la liste déroulante résultante.
-   Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent.
-   Cliquez sur **&lt;sélectionner un Delete Function&gt;** et sélectionnez **DeletePerson** dans la liste déroulante résultante.
-   Les mappages par défaut entre les paramètres des procédures stockées et les propriétés d'entité apparaissent.

L’instruction insert, update et delete d’opérations de le **personne** type d’entité sont maintenant mappées à des procédures stockées.

Si vous souhaitez activer l’accès concurrentiel lors de la mise à jour ou suppression d’une entité avec des procédures stockées, utilisez une des options suivantes :

-   Utilisez un **sortie** paramètre pour retourner le nombre d’affecté des lignes à partir de la procédure stockée et la vérification de la **paramètre des lignes affectées** case à cocher en regard du nom de paramètre. Si la valeur retournée est zéro lorsque l’opération est appelée, un [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) sera levée.
-   Vérifier le **utiliser Original Value** case à cocher en regard d’une propriété que vous souhaitez utiliser pour le contrôle d’accès concurrentiel. Lorsqu’une mise à jour est tentée, la valeur de la propriété qui a été initialement lue à partir de la base de données sera utilisée lors de l’écriture des données dans la base de données. Si la valeur ne correspond pas à la valeur dans la base de données, un **OptimisticConcurrencyException** sera levée.

## <a name="use-the-model"></a>Utiliser le modèle

Ouvrez le **Program.cs** fichier où le **Main** méthode est définie. Ajoutez le code suivant dans la fonction Main.

Le code crée un nouveau **personne** objet, puis met à jour l’objet et enfin supprime l’objet.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   Compilez et exécutez l'application. Le programme génère la sortie suivante *
    >[!NOTE]
> PersonID étant généré automatiquement par le serveur, vous verrez très probablement un autre nombre *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Si vous travaillez avec la version finale de Visual Studio, vous pouvez utiliser Intellitrace avec le débogueur pour voir les instructions SQL qui sont exécutées.

![IntelliTrace](~/ef6/media/intellitrace.png)
