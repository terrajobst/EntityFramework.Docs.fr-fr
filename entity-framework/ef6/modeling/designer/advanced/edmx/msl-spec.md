---
title: Spécification MSL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 6bff1f5407bc0546e60b5bee1178be9aa4748bd8
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460135"
---
# <a name="msl-specification"></a>Spécification MSL
Langage de spécification de mappage (MSL) est un langage basé sur XML qui décrit le mappage entre le modèle conceptuel et le modèle de stockage d’une application Entity Framework.

Dans une application Entity Framework, les métadonnées de mappage sont chargée à partir d’un fichier .msl (écrit en MSL) au moment de la génération. Entity Framework utilise les métadonnées de mappage lors de l’exécution pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques au magasin.

Entity Framework Designer (Concepteur d’EF) stocke des informations de mappage dans un fichier .edmx au moment du design. Au moment de la génération, le Concepteur d’entités utilise les informations dans un fichier .edmx pour créer le fichier .msl qui est nécessaire par Entity Framework lors de l’exécution

Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif. Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez [spécification CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Les versions de code MSL sont différenciées par les espaces de noms XML.

| Version MSL | XML Namespace                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn : schemas-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Élément Alias (MSL)

Le **Alias** élément dans la spécification du langage MSL (mapping) est un enfant de l’élément de mappage qui est utilisé pour définir des alias pour des noms de modèle de stockage et le modèle conceptuels. Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif. Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez l’élément Schema (CSDL). Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez l’élément de schéma (SSDL).

Le **Alias** élément ne peut pas avoir d’éléments enfants.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Alias** élément.

| Nom d'attribut | Requis | Value                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Oui         | L’alias pour l’espace de noms spécifié par le **valeur** attribut. |
| **Valeur**      | Oui         | L’espace de noms pour lequel la valeur de la **clé** élément est un alias.     |

### <a name="example"></a>Exemple

L’exemple suivant montre un **Alias** élément qui définit un alias, `c`, pour les types qui sont définis dans le modèle conceptuel.

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a>AssociationEnd, élément (MSL)

Le **AssociationEnd** élément dans la spécification du langage MSL (mapping) est utilisé lorsque les fonctions de modification d’un type d’entité dans le modèle conceptuel sont mappées aux procédures stockées dans la base de données sous-jacente. Si une modification de procédure stockée accepte un paramètre dont la valeur est conservée dans une propriété d’association, le **AssociationEnd** élément est mappé à la valeur de propriété pour le paramètre. Pour plus d'informations, voir l'exemple ci-dessous.

Pour plus d’informations sur le mappage des fonctions de modification de types d’entités à des procédures stockées, consultez ModificationFunctionMapping élément (MSL) et procédure pas à pas : mappage d’une entité aux procédures stockées.

Le **AssociationEnd** élément peut avoir les éléments enfants suivants :

-   ScalarProperty

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **AssociationEnd** élément.

| Nom d'attribut     | Requis | Value                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Oui         | Nom de l'association mappée.                                                                                                                                 |
| **From**           | Oui         | La valeur de la **FromRole** attribut de la propriété de navigation qui correspond à l’association mappée. Pour plus d’informations, consultez l’élément NavigationProperty (CSDL). |
| **To**             | Oui         | La valeur de la **ToRole** attribut de la propriété de navigation qui correspond à l’association mappée. Pour plus d’informations, consultez l’élément NavigationProperty (CSDL).   |

### <a name="example"></a>Exemple

Considérons le type d'entité de modèle conceptuel suivant :

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

Considérons également la procédure stockée suivante :

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

Pour mapper la fonction de mise à jour de la `Course` entité à cette procédure stockée, vous devez fournir une valeur pour le **DepartmentID** paramètre. La valeur pour `DepartmentID` ne correspond pas à une propriété sur le type d'entité ; elle est contenue dans une association indépendante dont le mappage est indiqué ici :

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

Le code suivant illustre la **AssociationEnd** élément utilisé pour mapper le **DepartmentID** propriété de la **FK\_cours\_département** association à la **UpdateCourse** procédure stockée (dans lequel la fonction de mise à jour de la **cours** type d’entité est mappé) :

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping, élément (MSL)

Le **AssociationSetMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une association dans les colonnes de table et au modèle conceptuels dans la base de données sous-jacente.

Les associations dans le modèle conceptuel sont des types dont les propriétés représentent des colonnes de clé primaire et de clé étrangère dans la base de données sous-jacente. Le **AssociationSetMapping** élément utilise deux éléments EndProperty pour définir les mappages entre les propriétés de type d’association et les colonnes dans la base de données. Vous pouvez placer des conditions sur ces mappages avec l’élément Condition. Mapper l’insert, update et les fonctions de suppression pour les associations à des procédures stockées dans la base de données avec l’élément ModificationFunctionMapping. Définir des mappages en lecture seule entre les associations et les colonnes de table à l’aide d’une chaîne de Entity SQL dans un QueryView (élément).

> [!NOTE]
> Si une contrainte référentielle est définie pour une association dans le modèle conceptuel, l’association ne doit-elle pas être mappées avec un **AssociationSetMapping** élément. Si un **AssociationSetMapping** élément est présent pour une association qui a une contrainte référentielle, les mappages définis dans le **AssociationSetMapping** élément sera ignoré. Pour plus d’informations, consultez élément ReferentialConstraint (CSDL).

Le **AssociationSetMapping** élément peut avoir les éléments enfants suivants

-   QueryView (zéro ou 1)
-   EndProperty (zéro ou deux)
-   Condition (zéro ou plus)
-   ModificationFunctionMapping (zéro ou 1)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **AssociationSetMapping** élément.

| Nom d'attribut     | Requis | Value                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Oui         | Nom de l'ensemble d'associations du modèle conceptuel mappé.                      |
| **TypeName**       | Non          | Nom qualifié par un espace de noms du type d'association du modèle conceptuel mappé. |
| **StoreEntitySet** | Non          | Nom de la table mappée.                                                 |

### <a name="example"></a>Exemple

L’exemple suivant montre un **AssociationSetMapping** élément dans lequel le **FK\_cours\_département** ensemble d’associations dans le modèle conceptuel sont mappée à la  **Cours** table dans la base de données. Mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans l’enfant **EndProperty** éléments.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a>ComplexProperty, élément (MSL)

Un **ComplexProperty** élément dans la spécification du langage MSL (mapping) définit le mappage entre une propriété de type complexe sur un modèle conceptuel entity type et la table colonnes dans la base de données sous-jacente. Les mappages de colonnes de propriété sont spécifiés dans les éléments ScalarProperty enfants.

Le **ComplexType** élément de propriété peut avoir les éléments enfants suivants :

-   ScalarProperty (zéro ou plus)
-   **ComplexProperty** (zéro ou plus)
-   ComplextTypeMapping (zéro ou plus)
-   Condition (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **ComplexProperty** élément :

| Nom d'attribut | Requis | Value                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom de la propriété complexe d'un type d'entité dans le modèle conceptuel mappé. |
| **TypeName**   | Non          | Nom qualifié par un espace de noms du type de propriété de modèle conceptuel.                              |

### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School. Le type complexe suivant a été ajouté au modèle conceptuel :

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

Le **LastName** et **FirstName** propriétés de la **personne** type d’entité ont été remplacées par une propriété complexe, **nom**:

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

Le MSL suivant indique le **ComplexProperty** élément utilisé pour mapper le **nom** propriété aux colonnes dans la base de données sous-jacente :

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping, élément (MSL)

Le **ComplexTypeMapping** élément dans la spécification du langage MSL (mapping) est un enfant de l’élément ResultMapping et définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans sous-jacent base de données lorsque les conditions suivantes sont remplies :

-   L'importation de fonction retourne un type complexe conceptuel.
-   Les noms de colonne retournés par la procédure stockée ne correspondent pas exactement aux noms de propriété du type complexe.

Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type complexe est basé sur les noms de colonne et de propriété. Si les noms de colonnes ne correspondent pas exactement les noms de propriété, vous devez utiliser le **ComplexTypeMapping** élément pour définir le mappage. Pour obtenir un exemple du mappage par défaut, consultez l’élément FunctionImportMapping (MSL).

Le **ComplexTypeMapping** élément peut avoir les éléments enfants suivants :

-   ScalarProperty (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **ComplexTypeMapping** élément.

| Nom d'attribut | Requis | Value                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | Oui         | Nom qualifié par un espace de noms du type complexe mappé. |

### <a name="example"></a>Exemple

Considérons la procédure stockée suivante :

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

De même, considérons le type complexe de modèle conceptuel suivant :

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Pour créer une importation de fonction qui retourne des instances du type complexe précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un **ComplexTypeMapping** élément :

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a>Condition, élément (MSL)

Le **Condition** élément dans la spécification du langage MSL (mapping) impose des conditions sur les mappages entre le modèle conceptuel et la base de données sous-jacente. Le mappage est défini dans un nœud XML est valides si toutes les conditions, en tant qu’enfant spécifié dans **Condition** éléments, sont remplies. À défaut, le mappage n'est pas valide. Par exemple, si un élément MappingFragment contient un ou plusieurs **Condition** éléments enfants, le mappage défini dans le **MappingFragment** nœud n’est valide que si toutes les conditions de l’enfant  **Condition** éléments sont remplies.

Chaque condition peut s’appliquer à un un **nom** (le nom d’une propriété d’entité de modèle conceptuel, spécifié par le **nom** attribut), ou un **ColumnName** (le nom d’une colonne dans la base de données spécifiée par le **ColumnName** attribut). Lorsque le **nom** attribut est défini, la condition est vérifiée par rapport à une valeur de propriété d’entité. Lorsque le **ColumnName** attribut est défini, la condition est vérifiée par rapport à une valeur de colonne. Une des seules la **nom** ou **ColumnName** attribut peut être spécifié dans un **Condition** élément.

> [!NOTE]
> Lorsque le **Condition** élément est utilisé dans un élément FunctionImportMapping uniquement le **nom** attribut n’est pas applicable.

Le **Condition** élément peut être un enfant des éléments suivants :

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

Le **Condition** élément ne peut avoir aucun élément enfant.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **Condition** élément :

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Non          | Nom de la colonne de table dont la valeur est utilisée pour évaluer la condition.                                                                                                                                                                                                                   |
| **IsNull**     | Non          | **True** ou **False**. Si la valeur est **True** et la valeur de colonne est **null**, ou si la valeur est **False** et la valeur de colonne n’est pas **null**, la condition est vraie . Sinon, la condition n'est pas vérifiée (False). <br/> Le **IsNull** et **valeur** les attributs ne peuvent pas être utilisés en même temps. |
| **Valeur**      | Non          | Valeur à laquelle la valeur de colonne est comparée. Si les valeurs sont identiques, la condition est vérifiée (True). Sinon, la condition n'est pas vérifiée (False). <br/> Le **IsNull** et **valeur** les attributs ne peuvent pas être utilisés en même temps.                                                                       |
| **Name**       | Non          | Nom de la propriété d'entité de modèle conceptuel dont la valeur est utilisée pour évaluer la condition. <br/> Cet attribut n’est pas applicable si la **Condition** élément est utilisé dans un élément FunctionImportMapping.                                                                           |

### <a name="example"></a>Exemple

L’exemple suivant **Condition** éléments en tant qu’enfants de **MappingFragment** éléments. Lorsque **HireDate** n’est pas null et **EnrollmentDate** est null, les données sont mappées entre le **SchoolModel.Instructor** type et le **PersonID**et **HireDate** colonnes de la **personne** table. Lorsque **EnrollmentDate** n’est pas null et **HireDate** est null, les données sont mappées entre le **SchoolModel.Student** type et le **PersonID** et **inscription** colonnes de la **personne** table.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a>DeleteFunction, élément (MSL)

Le **DeleteFunction** élément dans la spécification du langage MSL (mapping) est mappé à la fonction de suppression d’un type d’entité ou une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente. Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage. Pour plus d’informations, consultez la fonction élément (SSDL).

> [!NOTE]
> Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.

### <a name="deletefunction-applied-to-entitytypemapping"></a>Application de DeleteFunction à EntityTypeMapping

Lorsqu’il est appliqué à l’élément EntityTypeMapping, le **DeleteFunction** élément est mappé à la fonction de suppression d’un type d’entité dans le modèle conceptuel à une procédure stockée.

Le **DeleteFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à un **EntityTypeMapping** élément :

-   AssociationEnd (zéro ou plus)
-   ComplexProperty (zéro ou plus)
-   ScalarProperty (zéro ou plus)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **DeleteFunction** élément lorsqu’il est appliqué à un **EntityTypeMapping** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Oui         | Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée. La procédure stockée doit être déclarée dans le modèle de stockage. |
| **RowsAffectedParameter** | Non          | Nom du paramètre de sortie qui retourne le nombre de lignes affectées.                                                                               |

#### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre la **DeleteFunction** élément de mappage de la fonction de suppression de la **personne** type d’entité à la **DeletePerson** procédure stockée. Le **DeletePerson** procédure stockée est déclarée dans le modèle de stockage.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a>Application de DeleteFunction à AssociationSetMapping

Lorsqu’il est appliqué à l’élément AssociationSetMapping, le **DeleteFunction** élément est mappé à la fonction de suppression d’une association dans le modèle conceptuel à une procédure stockée.

Le **DeleteFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à la **AssociationSetMapping** élément :

-   EndProperty

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **DeleteFunction** élément lorsqu’il est appliqué à la **AssociationSetMapping** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Oui         | Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée. La procédure stockée doit être déclarée dans le modèle de stockage. |
| **RowsAffectedParameter** | Non          | Nom du paramètre de sortie qui retourne le nombre de lignes affectées.                                                                               |

#### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre la **DeleteFunction** élément utilisé pour mapper la fonction de suppression de la **CourseInstructor** association à la  **DeleteCourseInstructor** procédure stockée. Le **DeleteCourseInstructor** procédure stockée est déclarée dans le modèle de stockage.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a>EndProperty, élément (MSL)

Le **EndProperty** élément dans la spécification du langage MSL (mapping) définit le mappage entre une terminaison ou une fonction de modification d’une association de modèle conceptuel et la base de données sous-jacente. Le mappage de colonne de la propriété est spécifié dans un élément de ScalarProperty enfant.

Quand un **EndProperty** élément est utilisé pour définir le mappage pour la fin d’une association de modèle conceptuel, c’est un enfant d’un élément AssociationSetMapping. Lorsque le **EndProperty** élément est utilisé pour définir le mappage d’une fonction de modification d’une association de modèle conceptuel, c’est un enfant d’un élément InsertFunction ou d’élément DeleteFunction.

Le **EndProperty** élément peut avoir les éléments enfants suivants :

-   ScalarProperty (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **EndProperty** élément :

| Nom d'attribut | Requis | Value                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Name           | Oui         | Nom de la terminaison d'association mappée. |

### <a name="example"></a>Exemple

L’exemple suivant montre un **AssociationSetMapping** élément dans lequel le **FK\_cours\_département** association dans le modèle conceptuel est mappée à la **Cours** table dans la base de données. Mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans l’enfant **EndProperty** éléments.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a>Exemple

L’exemple suivant montre le **EndProperty** élément les fonctions insert et delete d’une association de mappage (**CourseInstructor**) aux procédures stockées dans la base de données sous-jacente. Les fonctions mappées sont déclarées dans le modèle de stockage.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping, élément (MSL)

Le **EntityContainerMapping** élément dans la spécification du langage MSL (mapping) mappe le conteneur d’entités dans le modèle conceptuel au conteneur d’entités du modèle de stockage. Le **EntityContainerMapping** élément est un enfant de l’élément de mappage.

Le **EntityContainerMapping** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   EntitySetMapping (zéro ou plus)
-   AssociationSetMapping (zéro ou plus)
-   FunctionImportMapping (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntityContainerMapping** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Oui         | Nom du conteneur d'entités de modèle de stockage mappé.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Oui         | Nom du conteneur d'entités de modèle conceptuel mappé.                                                                                                                                                                                  |
| **Generateupdateviews comme**   | Non          | **True** ou **False**. Si **False**, aucune vue de la mise à jour n’est générés. Cet attribut doit être défini **False** lorsque vous avez un mappage en lecture seule qui n’est pas valide, car les données ne peuvent pas effectuer un aller-retour avec succès. <br/> La valeur par défaut est **True**. |

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainerMapping** élément qui mappe le **SchoolModelEntities** container (conteneur d’entités du modèle conceptuel) à la  **SchoolModelStoreContainer** conteneur (le modèle entité conteneur de stockage) :

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping, élément (MSL)

Le **EntitySetMapping** élément dans les mappages de langage MSL (Mapping) spécification tous les types dans une entité de modèle conceptuel est définis à l’entité de mappage définit le modèle de stockage. Une jeu d’entités dans le modèle conceptuel sont un conteneur logique pour les instances d’entités du même type (et les types dérivés). Une jeu d’entités dans le modèle de stockage représente une table ou vue dans la base de données sous-jacente. Le jeu d’entités de modèle conceptuel est spécifié par la valeur de la **nom** attribut de la **EntitySetMapping** élément. La mappée vers une table ou la vue est spécifié par le **StoreEntitySet** attribut dans chaque élément de MappingFragment enfant ou dans le **EntitySetMapping** élément lui-même.

Le **EntitySetMapping** élément peut avoir les éléments enfants suivants :

-   EntityTypeMapping (zéro ou plus)
-   QueryView (zéro ou 1)
-   MappingFragment (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntitySetMapping** élément.

| Nom d'attribut           | Requis | Value                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Oui         | Nom du jeu d'entités de modèle conceptuel mappé.                                                                                                                                                             |
| **TypeName** **1**       | Non          | Nom du type d'entité de modèle conceptuel mappé.                                                                                                                                                            |
| **StoreEntitySet** **1** | Non          | Nom du jeu d'entités de modèle de stockage de destination du mappage.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Non          | **True** ou **False** selon si seules des lignes distinctes sont retournées. <br/> Si cet attribut est défini **True**, le **generateupdateviews comme** attribut de l’élément EntityContainerMapping doit être définie sur **False**. |

 

**1** le **TypeName** et **StoreEntitySet** attributs peuvent être utilisés à la place les éléments enfant EntityTypeMapping et MappingFragment pour mapper un type d’entité unique à une seule table.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntitySetMapping** élément qui mappe les trois types (un type de base et deux types dérivés) dans le **cours** jeu d’entités du modèle conceptuel à trois tables différentes dans le base de données sous-jacente. Les tables sont spécifiées par le **StoreEntitySet** attribut dans chaque **MappingFragment** élément.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping, élément (MSL)

Le **EntityTypeMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre un type d’entité dans le modèle conceptuel et les tables ou vues dans la base de données sous-jacente. Pour plus d’informations sur les types d’entité de modèle conceptuel et les tables de base de données ou les vues sous-jacentes, consultez l’élément EntityType (CSDL) et élément EntitySet (SSDL). Le type d’entité de modèle conceptuel mappé est spécifié par le **TypeName** attribut de la **EntityTypeMapping** élément. La table ou vue qui est mappé est spécifié par le **StoreEntitySet** attribut de l’élément MappingFragment enfant.

Le ModificationFunctionMapping, élément enfant peut être utilisé pour mapper l’insertion, mise à jour ou supprimer des fonctions de types d’entité aux procédures stockées dans la base de données.

Le **EntityTypeMapping** élément peut avoir les éléments enfants suivants :

-   MappingFragment (zéro ou plus)
-   ModificationFunctionMapping (zéro ou 1)
-   ScalarProperty
-   Condition

> [!NOTE]
> **MappingFragment** et **ModificationFunctionMapping** éléments ne peut pas être des éléments enfants de la **EntityTypeMapping** élément en même temps.


> [!NOTE]
> Le **ScalarProperty** et **Condition** éléments peuvent être uniquement les éléments enfants de la **EntityTypeMapping** élément lorsqu’il est utilisé dans un élément FunctionImportMapping.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntityTypeMapping** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | Oui         | Nom qualifié par un espace de noms du type d'entité de modèle conceptuel mappé. <br/> Si le type correspond à un type abstrait ou dérivé, la valeur doit être `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Exemple

L’exemple suivant montre un élément EntitySetMapping avec deux enfants **EntityTypeMapping** éléments. Dans la première **EntityTypeMapping** élément, le **SchoolModel.Person** type d’entité est mappé à la **personne** table. Dans la seconde **EntityTypeMapping** élément, la fonctionnalité de mise à jour de la **SchoolModel.Person** type est mappé à une procédure stockée, **UpdatePerson**, dans la base de données .

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Exemple

L'exemple suivant illustre le mappage d'une hiérarchie de types dont le type racine est abstrait. Notez l’utilisation de la `IsOfType` syntaxe pour le **TypeName** attributs.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping, élément (MSL)

Le **FunctionImportMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée ou fonction dans la base de données sous-jacente. Les importations de fonction doivent être déclarées dans le modèle conceptuel et les procédures stockées dans le modèle de stockage. Pour plus d’informations, consultez l’élément FunctionImport (CSDL) et élément (fonction) (SSDL).

> [!NOTE]
> Par défaut, si une importation de fonction retourne un type d'entité ou un type complexe de modèle conceptuel, les noms des colonnes retournés par la procédure stockée sous-jacente doivent correspondre exactement aux noms des propriétés sur le type de modèle conceptuel. Si les noms de colonnes ne correspondent pas exactement les noms de propriété, le mappage doit être défini dans un élément ResultMapping.

Le **FunctionImportMapping** élément peut avoir les éléments enfants suivants :

-   ResultMapping (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **FunctionImportMapping** élément :

| Nom d'attribut         | Requis | Value                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName se** | Oui         | Nom de l'importation de fonction dans le modèle conceptuel mappé.           |
| **FunctionName**       | Oui         | Nom qualifié par un espace de noms de la fonction dans le modèle de stockage mappé. |

### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School. Considérez la fonction suivante dans le modèle de stockage :

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Considérez également cette importation de fonction dans le modèle conceptuel :

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

L’exemple suivant affiche un **FunctionImportMapping** élément utilisé pour mapper la fonction et l’importation de fonction ci-dessus entre eux :

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction, élément (MSL)

Le **InsertFunction** élément dans la spécification du langage MSL (mapping) est mappé à la fonction d’insertion d’un type d’entité ou une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente. Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage. Pour plus d’informations, consultez la fonction élément (SSDL).

> [!NOTE]
> Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.

Le **InsertFunction** élément peut être un enfant de l’élément ModificationFunctionMapping et appliqué à l’élément EntityTypeMapping ou de l’élément AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>Application d'InsertFunction à EntityTypeMapping

Lorsqu’il est appliqué à l’élément EntityTypeMapping, le **InsertFunction** élément est mappé à la fonction d’insertion d’un type d’entité dans le modèle conceptuel à une procédure stockée.

Le **InsertFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à un **EntityTypeMapping** élément :

-   AssociationEnd (zéro ou plus)
-   ComplexProperty (zéro ou plus)
-   ResultBinding (zéro ou 1)
-   ScalarProperty (zéro ou plus)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **InsertFunction** élément lorsqu’il est appliqué à un **EntityTypeMapping** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Oui         | Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée. La procédure stockée doit être déclarée dans le modèle de stockage. |
| **RowsAffectedParameter** | Non          | Nom du paramètre de sortie qui retourne le nombre de lignes affectées.                                                                               |

#### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre la **InsertFunction** élément utilisé pour mapper la fonction d’insertion du type d’entité Person à la **InsertPerson** procédure stockée. Le **InsertPerson** procédure stockée est déclarée dans le modèle de stockage.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a>Application d'InsertFunction à AssociationSetMapping

Lorsqu’il est appliqué à l’élément AssociationSetMapping, le **InsertFunction** élément est mappé à la fonction d’insertion d’une association dans le modèle conceptuel à une procédure stockée.

Le **InsertFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à la **AssociationSetMapping** élément :

-   EndProperty

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **InsertFunction** élément lorsqu’il est appliqué à la **AssociationSetMapping** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Oui         | Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée. La procédure stockée doit être déclarée dans le modèle de stockage. |
| **RowsAffectedParameter** | Non          | Nom du paramètre de sortie qui retourne le nombre de lignes affectées.                                                                               |

#### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre la **InsertFunction** élément utilisé pour mapper la fonction d’insertion de la **CourseInstructor** association à la  **InsertCourseInstructor** procédure stockée. Le **InsertCourseInstructor** procédure stockée est déclarée dans le modèle de stockage.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a>Élément de mappage (MSL)

Le **mappage** élément dans la spécification du langage MSL (mapping) contient des informations de mappage des objets qui sont définis dans un modèle conceptuel à une base de données (comme décrit dans un modèle de stockage). Pour plus d’informations, consultez la spécification CSDL et SSDL Specification.

Le **mappage** élément est l’élément racine pour une spécification de mappage. L’espace de noms XML pour les spécifications de mappage est http://schemas.microsoft.com/ado/2009/11/mapping/cs.

L'élément de mappage peut avoir les éléments enfants suivants (dans l'ordre répertorié) :

-   Alias (zéro ou plus)
-   EntityContainerMapping (exactement un élément)

Les noms de types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif. Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez l’élément Schema (CSDL). Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez l’élément de schéma (SSDL). Alias pour les espaces de noms qui sont utilisés en MSL peuvent être définis avec l’élément de l’Alias.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **mappage** élément.

| Nom d'attribut | Requis | Value                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Espace**      | Oui         | **C-S**. Il s'agit d'une valeur fixe qui ne peut pas être modifiée. |

### <a name="example"></a>Exemple

L’exemple suivant montre un **mappage** élément basé sur une partie du modèle School. Pour plus d’informations sur le modèle School, consultez le Guide de démarrage rapide (Entity Framework) :

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a>MappingFragment, élément (MSL)

Le **MappingFragment** élément dans la spécification du langage MSL (mapping) définit le mappage entre les propriétés d’un type d’entité de modèle conceptuel et une table ou vue dans la base de données. Pour plus d’informations sur les types d’entité de modèle conceptuel et les tables de base de données ou les vues sous-jacentes, consultez l’élément EntityType (CSDL) et élément EntitySet (SSDL). Le **MappingFragment** peut être un élément enfant de l’élément EntityTypeMapping ou l’élément EntitySetMapping.

Le **MappingFragment** élément peut avoir les éléments enfants suivants :

-   ComplexType (zéro ou plus)
-   ScalarProperty (zéro ou plus)
-   Condition (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **MappingFragment** élément.

| Nom d'attribut          | Requis | Value                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Oui         | Nom de la table ou de la vue mappée.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Non          | **True** ou **False** selon si seules des lignes distinctes sont retournées. <br/> Si cet attribut est défini **True**, le **generateupdateviews comme** attribut de l’élément EntityContainerMapping doit être définie sur **False**. |

### <a name="example"></a>Exemple

L’exemple suivant montre un **MappingFragment** élément comme enfant d’un **EntityTypeMapping** élément. Dans cet exemple, les propriétés de la **cours** type dans le modèle conceptuel sont mappées aux colonnes de la **cours** table dans la base de données.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Exemple

L’exemple suivant montre un **MappingFragment** élément comme enfant d’un **EntitySetMapping** élément. Comme dans l’exemple ci-dessus, les propriétés de la **cours** type dans le modèle conceptuel sont mappées aux colonnes de la **cours** table dans la base de données.

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping, élément (MSL)

Le **ModificationFunctionMapping** élément dans la spécification du langage MSL (mapping) mappe l’insertion, mise à jour et supprimer des fonctions d’un type d’entité de modèle conceptuel aux procédures stockées dans la base de données sous-jacente. Le **ModificationFunctionMapping** élément peut également mapper l’insertion et supprimer des fonctions pour les associations plusieurs-à-plusieurs dans le modèle conceptuel aux procédures stockées dans la base de données sous-jacente. Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage. Pour plus d’informations, consultez la fonction élément (SSDL).

> [!NOTE]
> Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.


> [!NOTE]
> Si les fonctions de modification pour une entité dans une hiérarchie d'héritage sont mappées aux procédures stockées, les fonctions de modification de tous les types dans la hiérarchie doivent être mappées aux procédures stockées.

Le **ModificationFunctionMapping** élément peut être un enfant de l’élément AssociationSetMapping ou de l’élément EntityTypeMapping.

Le **ModificationFunctionMapping** élément peut avoir les éléments enfants suivants :

-   DeleteFunction (zéro ou 1)
-   InsertFunction (zéro ou 1)
-   UpdateFunction (zéro ou 1)

Aucun attribut n’est applicable à la **ModificationFunctionMapping** élément.

### <a name="example"></a>Exemple

L’exemple suivant montre l’entité de mappage du jeu du **personnes** jeu d’entités dans le modèle School. Outre le mappage de colonne pour le **personne** type d’entité, le mappage de l’insertion, mise à jour et supprimer des fonctions de la **personne** type sont affichés. Les fonctions mappées sont déclarées dans le modèle de stockage.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Exemple

L’exemple suivant illustre l’association de définir le mappage pour le **CourseInstructor** ensemble d’associations dans le modèle School. Outre le mappage de colonne pour le **CourseInstructor** association, le mappage des fonctions insert et delete de la **CourseInstructor** association sont affichés. Les fonctions mappées sont déclarées dans le modèle de stockage.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a>QueryView, élément (MSL)

Le **QueryView** élément dans la spécification du langage MSL (mapping) définit un mappage en lecture seule entre un type d’entité ou une association dans le modèle conceptuel et une table dans la base de données sous-jacente. Le mappage est défini avec une requête Entity SQL qui est évaluée par rapport au modèle de stockage et vous exprimez le jeu en termes d’une entité ou une association dans le modèle conceptuel de résultats. Les affichages des requêtes étant en lecture seule, les types qu'ils définissent ne peuvent pas être mis à jour au moyen des commandes de mise à jour standard. Les mises à jour de ces types peuvent être effectuées au moyen de fonctions de modification. Pour plus d’informations, consultez Comment : mappage des fonctions de Modification aux procédures stockées.

> [!NOTE]
> Dans le **QueryView** élément, les expressions Entity SQL qui contiennent **GroupBy**, agrégats de groupe ou les propriétés de navigation ne sont pas pris en charge.

 

Le **QueryView** élément peut être un enfant de l’élément EntitySetMapping ou AssociationSetMapping, élément. Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une entité dans le modèle conceptuel. Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une association dans le modèle conceptuel.

> [!NOTE]
> Si le **AssociationSetMapping** élément concerne une association avec une contrainte référentielle, les **AssociationSetMapping** élément est ignoré. Pour plus d’informations, consultez élément ReferentialConstraint (CSDL).

Le **QueryView** élément ne peut pas avoir d’éléments enfants.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **QueryView** élément.

| Nom d'attribut | Requis | Value                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | Non          | Nom du type de modèle conceptuel mappé par l'affichage des requêtes. |

### <a name="example"></a>Exemple

L’exemple suivant montre le **QueryView** élément en tant qu’enfant de le **EntitySetMapping** élément et définit un mappage de vue de requête pour le **département** type d’entité dans le Modèle School.

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

Étant donné que la requête retourne uniquement un sous-ensemble des membres de la **département** type du modèle de stockage, le **département** type dans le modèle School a été modifié en fonction de ce mappage comme suit :

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a>Exemple

L’exemple suivant montre le **QueryView** élément comme enfant d’un **AssociationSetMapping** élément et définit un mappage en lecture seule pour le `FK_Course_Department` association dans le modèle School.

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a>Commentaires

Vous pouvez définir des affichages des requêtes pour activer les scénarios suivants :

-   Définir une entité du modèle conceptuel qui n'inclut pas toutes les propriétés de l'entité dans le modèle de stockage. Cela inclut les propriétés qui n’ont pas de valeurs par défaut et ne gèrent pas **null** valeurs.
-   Mapper des colonnes calculées du modèle de stockage aux propriétés de types d'entités du modèle conceptuel.
-   Définir un mappage dans lequel les conditions utilisées pour partitionner des entités du modèle conceptuel ne sont pas basées sur l'égalité. Lorsque vous spécifiez un mappage conditionnel à l’aide de la **Condition** élément, la condition fournie doit être égale à la valeur spécifiée. Pour plus d’informations, consultez Condition élément (MSL).
-   Mapper la même colonne du modèle de stockage à plusieurs types du modèle conceptuel.
-   Mapper plusieurs types à la même table.
-   Définir des associations dans le modèle conceptuel qui ne sont pas basées sur des clés étrangères du schéma relationnel.
-   Utiliser une logique métier personnalisée pour définir la valeur de propriétés du modèle conceptuel. Par exemple, vous pouvez la mapper la valeur de chaîne « T » dans la source de données à une valeur **true**, une valeur booléenne, dans le modèle conceptuel.
-   Définir des filtres conditionnels pour les résultats de la requête.
-   Appliquer moins de restrictions sur les données dans le modèle conceptuel que dans le modèle de stockage. Par exemple, vous pouvez rendre une propriété dans le modèle conceptuel nullable même si la colonne à laquelle elle est mappée ne prend pas en charge **null**valeurs.

Vous devez tenir compte des points suivants lorsque vous définissez des affichages des requêtes pour les entités :

-   Les affichages des requêtes sont en lecture seule. Les mises à jour des entités ne peuvent être effectuées qu'au moyen de fonctions de modification.
-   Lorsque vous définissez un type d'entité par un affichage des requêtes, vous devez également définir toutes les entités associées par les affichages des requêtes.
-   Lorsque vous mappez une association plusieurs-à-plusieurs à une entité du modèle de stockage qui représente une table de liens dans le schéma relationnel, vous devez définir un **QueryView** élément dans le **AssociationSetMapping** élément pour cette table de liens.
-   Les affichages des requêtes doivent être définis pour tous les types d'une hiérarchie des types. Pour ce faire, vous pouvez procéder de différentes façons :
-   -   Avec un seul **QueryView** élément qui spécifie une seule requête Entity SQL qui retourne une union de tous les types d’entités dans la hiérarchie.
    -   Avec un seul **QueryView** élément qui spécifie une requête Entity SQL unique qui utilise l’opérateur de cas pour retourner un type d’entité spécifique dans la hiérarchie selon une condition spécifique.
    -   Avec un autre **QueryView** élément pour un type spécifique dans la hiérarchie. Dans ce cas, utilisez le **TypeName** attribut de la **QueryView** élément pour spécifier le type d’entité pour chaque vue.
-   Lorsqu’un affichage des requêtes est défini, vous ne pouvez pas spécifier le **StorageSetName** d’attribut sur le **EntitySetMapping** élément.
-   Lorsqu’un affichage des requêtes est défini, le **EntitySetMapping**élément ne peut pas contenir également **propriété** mappages.

## <a name="resultbinding-element-msl"></a>Élément ResultBinding (MSL)

Le **ResultBinding** élément dans la spécification du langage MSL (mapping) mappe les valeurs de colonne sont retournées par les procédures stockées aux propriétés d’entités dans le modèle conceptuel lorsque les fonctions de modification de type entité sont mappées à stockée procédures dans la base de données sous-jacente. Par exemple, lorsque la valeur d’une colonne d’identité est retournée par une instruction insert procédure stockée, le **ResultBinding** élément est mappé à la valeur renvoyée à une propriété de type d’entité dans le modèle conceptuel.

Le **ResultBinding** élément peut être l’enfant de l’élément InsertFunction ou l’élément UpdateFunction.

Le **ResultBinding** élément ne peut pas avoir d’éléments enfants.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui s’appliquent à la **ResultBinding** élément :

| Nom d'attribut | Requis | Value                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom de la propriété d'entité dans le modèle conceptuel mappé. |
| **ColumnName** | Oui         | Nom de la colonne mappée.                                          |

### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre un **InsertFunction** élément utilisé pour mapper la fonction d’insertion de la **personne** type d’entité à la **InsertPerson** procédure stockée. (Le **InsertPerson** procédure stockée est indiquée ci-dessous et est déclarée dans le modèle de stockage.) Un **ResultBinding** élément est utilisé pour mapper une valeur de colonne qui est retournée par la procédure stockée (**NewPersonID**) à une propriété de type d’entité (**PersonID**).

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

Transact-SQL suivante décrit le **InsertPerson** procédure stockée :

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a>Élément ResultMapping (MSL)

Le **ResultMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans la base de données sous-jacente lorsque les conditions suivantes sont remplies :

-   L'importation de fonction retourne un type d'entité de modèle conceptuel ou le type complexe.
-   Les noms des colonnes retournés par la procédure stockée ne correspondent pas exactement aux noms des propriétés sur le type d'entité ou le type complexe.

Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type d'entité ou un type complexe est basé sur les noms de colonne et de propriété. Si les noms de colonnes ne correspondent pas exactement les noms de propriété, vous devez utiliser le **ResultMapping** élément pour définir le mappage. Pour obtenir un exemple du mappage par défaut, consultez l’élément FunctionImportMapping (MSL).

Le **ResultMapping** élément est un élément enfant de l’élément FunctionImportMapping.

Le **ResultMapping** élément peut avoir les éléments enfants suivants :

-   EntityTypeMapping (zéro ou plus)
-   ComplexTypeMapping

Aucun attribut n’est applicable à la **ResultMapping** élément.

### <a name="example"></a>Exemple

Considérons la procédure stockée suivante :

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

De même, considérons le type d'entité de modèle conceptuel suivant :

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

Pour créer une importation de fonction qui retourne des instances du type d’entité précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un **ResultMapping** élément :

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a>ScalarProperty, élément (MSL)

Le **ScalarProperty** élément dans la spécification du langage MSL (mapping) est mappé à une propriété sur un type d’entité de modèle conceptuel, un type complexe ou une association à une colonne de table ou un paramètre de procédure stockée dans la base de données sous-jacente.

> [!NOTE]
> Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage. Pour plus d’informations, consultez la fonction élément (SSDL).

Le **ScalarProperty** élément peut être un enfant des éléments suivants :

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

En tant qu’enfant de le **MappingFragment**, **ComplexProperty**, ou **EndProperty** élément, le **ScalarProperty** élément est mappé à une propriété dans le modèle conceptuel à une colonne dans la base de données. En tant qu’enfant de le **InsertFunction**, **UpdateFunction**, ou **DeleteFunction** élément, le **ScalarProperty** élément est mappé à une propriété dans le modèle conceptuel à un paramètre de procédure stockée.

Le **ScalarProperty** élément ne peut pas avoir d’éléments enfants.

### <a name="applicable-attributes"></a>Attributs applicables

Les attributs qui s’appliquent à la **ScalarProperty** élément diffèrent en fonction du rôle de l’élément.

Le tableau suivant décrit les attributs qui sont applicables lorsque le **ScalarProperty** élément est utilisé pour mapper une propriété de modèle conceptuel à une colonne dans la base de données :

| Nom d'attribut | Requis | Value                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Oui         | Nom de la propriété de modèle conceptuel mappée. |
| **ColumnName** | Oui         | Nom de la colonne de table mappée.              |

Le tableau suivant décrit les attributs qui s’appliquent à la **ScalarProperty** élément lorsqu’il est utilisé pour mapper une propriété de modèle conceptuel à un paramètre de procédure stockée :

| Nom d'attribut    | Requis | Value                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Oui         | Nom de la propriété de modèle conceptuel mappée.                                                                                 |
| **ParameterName** | Oui         | Nom du paramètre mappé.                                                                                                 |
| **Version**       | Non          | **Actuel** ou **d’origine** selon que la valeur actuelle ou la valeur d’origine de la propriété doit être utilisée pour les contrôles d’accès concurrentiel. |

### <a name="example"></a>Exemple

L’exemple suivant montre le **ScalarProperty** élément utilisé de deux manières :

-   Pour mapper les propriétés de la **personne** type d’entité aux colonnes de la **personne**table.
-   Pour mapper les propriétés de la **personne** type d’entité pour les paramètres de la **UpdatePerson** procédure stockée. Les procédures stockées sont déclarées dans le modèle de stockage.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Exemple

L’exemple suivant montre le **ScalarProperty** élément utilisé pour mapper l’insertion et de fonctions de suppression d’une association de modèle conceptuel aux procédures stockées dans la base de données. Les procédures stockées sont déclarées dans le modèle de stockage.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a>UpdateFunction, élément (MSL)

Le **UpdateFunction** élément dans la spécification du langage MSL (mapping) mappe la fonction de mise à jour d’un type d’entité dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente. Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage. Pour plus d’informations, consultez la fonction élément (SSDL).

> [!NOTE]
>  Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.

Le **UpdateFunction** élément peut être un enfant de l’élément ModificationFunctionMapping et appliqué à l’élément EntityTypeMapping.

Le **UpdateFunction** élément peut avoir les éléments enfants suivants :

-   AssociationEnd (zéro ou plus)
-   ComplexProperty (zéro ou plus)
-   ResultBinding (zéro ou 1)
-   ScalarProperty (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **UpdateFunction** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Oui         | Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de mise à jour est mappée. La procédure stockée doit être déclarée dans le modèle de stockage. |
| **RowsAffectedParameter** | Non          | Nom du paramètre de sortie qui retourne le nombre de lignes affectées.                                                                               |

### <a name="example"></a>Exemple

L’exemple suivant est basé sur le modèle School et montre la **UpdateFunction** élément utilisé pour mapper la fonction de mise à jour de la **personne** type d’entité à la **UpdatePerson** procédure stockée. Le **UpdatePerson** procédure stockée est déclarée dans le modèle de stockage.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
