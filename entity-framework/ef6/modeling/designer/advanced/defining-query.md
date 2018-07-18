---
title: Définition de requête - Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121121"
---
# <a name="defining-query---ef-designer"></a>Définition de requête - Entity Framework Designer
Cette procédure pas à pas montre comment ajouter une définition de type de requête et une entité correspondante à un modèle à l’aide du Concepteur EF. Une requête de définition est couramment utilisée pour fournir des fonctionnalités semblables à celles fournies par une vue de base de données, mais la vue est définie dans le modèle, pas la base de données. Une requête de définition vous permet d’exécuter une instruction SQL qui est spécifiée dans le **DefiningQuery** élément d’un fichier .edmx. Pour plus d’informations, consultez **DefiningQuery** dans le [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Lorsque vous utilisez des requêtes de définition, vous devez également définir un type d’entité dans votre modèle. Le type d’entité est utilisé pour exposer les données exposées par la requête de définition. Notez que les données révélées par ce type d’entité sont en lecture seule.

Les requêtes paramétrées ne peuvent pas être exécutées en tant que requêtes de définition. Toutefois, les données peuvent être mises à jour en mappant les fonctions d'insertion, de mise à jour et de suppression du type d'entité qui surface les données aux procédures stockées. Pour plus d’informations, consultez [Insert, Update et Delete avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/cud.md).

Cette rubrique montre comment effectuer les tâches suivantes.

-   Ajouter une requête de définition
-   Ajouter un Type d’entité au modèle
-   Mapper la requête de définition pour le Type d’entité

## <a name="prerequisites"></a>Prérequis

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- Une version récente de Visual Studio.
- Le [base de données School exemple](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurer le projet

Cette procédure pas à pas utilise Visual Studio 2012 ou version ultérieure.

-   Ouvrez Visual Studio.
-   Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.
-   Dans le volet gauche, cliquez sur **Visual C\#**, puis sélectionnez le **Application Console** modèle.
-   Entrez **DefiningQuerySample** en tant que le nom du projet et cliquez sur **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Créer un modèle basé sur la base de données School

-   Cliquez sur le nom de projet dans l’Explorateur de solutions, pointez sur **ajouter**, puis cliquez sur **un nouvel élément**.
-   Sélectionnez **données** dans le menu de gauche, puis sélectionnez **ADO.NET Entity Data Model** dans le volet Modèles.
-   Entrez **DefiningQueryModel.edmx** pour le nom de fichier, puis cliquez sur **ajouter**.
-   Dans la boîte de dialogue Choisir le contenu du modèle, sélectionnez **générer à partir de la base de données**, puis cliquez sur **suivant**.
-   Cliquez sur Nouvelle connexion. Dans la boîte de dialogue Propriétés de connexion, entrez le nom du serveur (par exemple, **(localdb)\\mssqllocaldb**), sélectionnez la méthode d’authentification, tapez **School** pour le nom de la base de données, puis Cliquez sur **OK**.
    La boîte de dialogue Choisir votre connexion de données est mis à jour avec le paramètre de votre connexion de base de données.
-   Dans la boîte de dialogue Choisir vos objets de base de données, vérifiez le **Tables** nœud. Cette opération ajoute toutes les tables à la **School** modèle.
-   Cliquez sur **Terminer**.
-   Dans l’Explorateur de solutions, cliquez sur le **DefiningQueryModel.edmx** fichier et sélectionnez **ouvrir avec...** .
-   Sélectionnez **éditeur XML (texte)**.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   Cliquez sur **Oui** si vous y êtes invité avec le message suivant :

    ![Avertissement2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Ajouter une requête de définition

Dans cette étape, nous allons utiliser l’éditeur XML pour ajouter une définition de requête et un type d’entité à la section SSDL du fichier .edmx. 

-   Ajouter un **EntitySet** élément à la section SSDL du fichier .edmx (ligne 5 à 13). Spécifier les éléments suivants :
    -   Uniquement les **nom** et **EntityType** les attributs de la **EntitySet** élément sont spécifiées.
    -   Le nom qualifié complet du type d’entité est utilisé dans le **EntityType** attribut.
    -   L’instruction SQL à exécuter est spécifiée dans le **DefiningQuery** élément.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Ajouter le **EntityType** élément à la section SSDL du fichier. fichier comme indiqué ci-dessous. Notez les points suivants :
    -   La valeur de la **nom** attribut correspond à la valeur de la **EntityType** d’attribut dans le **EntitySet** élément ci-dessus, bien que le nom qualifié complet de la type d’entité est utilisé dans le **EntityType** attribut.
    -   Les noms des propriétés correspondent aux noms de colonnes retournés par l’instruction SQL dans le **DefiningQuery** élément (ci-dessus).
    -   Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Si plus tard vous exécutez le **Assistant modèle de mise à jour** boîte de dialogue, toutes les modifications apportées au modèle de stockage, y compris les requêtes de définition, sera remplacé.

 

## <a name="add-an-entity-type-to-the-model"></a>Ajouter un Type d’entité au modèle

Dans cette étape, nous allons ajouter le type d’entité au modèle conceptuel à l’aide du Concepteur EF.  Notez les points suivants :

-   Le **nom** de l’entité correspond à la valeur de la **EntityType** d’attribut dans le **EntitySet** élément ci-dessus.
-   Les noms des propriétés correspondent aux noms de colonnes retournés par l’instruction SQL dans le **DefiningQuery** élément ci-dessus.
-   Dans cet exemple, la clé d'entité est composée de trois propriétés pour garantir le caractère unique de la valeur de la clé.

Ouvrez le modèle dans le Concepteur EF.

-   Double-cliquez sur le DefiningQueryModel.edmx.
-   Par exemple **Oui** au message suivant :

    ![Avertissement2](~/ef6/media/warning2.png)

 

Le Concepteur d’entités, qui fournit une aire de conception pour la modification de votre modèle, s’affiche.

-   Cliquez sur l’aire du concepteur puis sélectionnez **Ajouter nouveau**-&gt;**entité...** .
-   Spécifiez **GradeReport** pour le nom de l’entité et **CourseID** pour le **propriété Key**.
-   Avec le bouton droit le **GradeReport** entité, puis sélectionnez **Ajouter nouveau** - &gt; **propriété scalaire**.
-   Modifier le nom par défaut de la propriété à **FirstName**.
-   Ajoutez une autre propriété scalaire et spécifiez **LastName** pour le nom.
-   Ajoutez une autre propriété scalaire et spécifiez **Grade** pour le nom.
-   Dans le **propriétés** fenêtre, modifier le **Grade**de **Type** propriété **décimal**.
-   Sélectionnez le **FirstName** et **LastName** propriétés.
-   Dans le **propriétés** fenêtre, de modifier le **EntityKey** valeur de propriété à **True**.

Par conséquent, les éléments suivants ont été ajoutés à la **CSDL** section du fichier .edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Mapper la requête de définition pour le Type d’entité

Dans cette étape, nous allons utiliser la fenêtre Détails de mappage pour mapper les concepts et les types d’entités de stockage.

-   Cliquez sur le **GradeReport** entité sur l’aire de conception et sélectionnez **mappage de Table**.  
    Le **détails de Mapping** fenêtre s’affiche.
-   Sélectionnez **GradeReport** à partir de la **&lt;ajouter une Table ou vue&gt;** liste déroulante (situé sous **Table**s).  
    Par défaut des mappages entre les concepts et de stockage **GradeReport** type d’entité s’affichent.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

Par conséquent, le **EntitySetMapping** élément est ajouté à la section mapping du fichier .edmx. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Compilez l'application.

 

## <a name="call-the-defining-query-in-your-code"></a>Appeler la requête de définition dans votre Code

Vous pouvez maintenant exécuter la requête de définition à l’aide de la **GradeReport** type d’entité. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
