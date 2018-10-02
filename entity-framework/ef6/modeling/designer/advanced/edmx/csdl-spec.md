---
title: Spécification CSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 438af83b8a1ad51ee8414341181412e950d0e117
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460148"
---
# <a name="csdl-specification"></a>Spécification CSDL
Le langage CSDL (Conceptual Schema Definition Language) est un langage basé sur XML qui décrit les entités, relations et fonctions qui composent un modèle conceptuel d'une application pilotée par les données. Ce modèle conceptuel peut être utilisé par Entity Framework ou WCF Data Services. Les métadonnées qui sont décrites avec le langage CSDL sont utilisée par Entity Framework pour mapper les entités et relations qui sont définies dans un modèle conceptuel à une source de données. Pour plus d’informations, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) et [spécification MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL est l’implémentation d’Entity Framework de l’Entity Data Model.

Dans une application Entity Framework, les métadonnées du modèle conceptuel est chargée à partir d’un fichier .csdl (écrit en langage CSDL) dans une instance de la System.Data.Metadata.Edm.EdmItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework utilise des métadonnées de modèle conceptuel pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques à la source de données.

Le Concepteur EF stocke les informations de modèle conceptuel dans un fichier .edmx au moment du design. Au moment de la génération, le Concepteur EF utilise les informations dans un fichier .edmx pour créer le fichier .csdl qui est nécessaire par Entity Framework lors de l’exécution.

Les versions de CSDL sont différenciées par les espaces de noms XML.

| Version du langage CSDL | XML Namespace                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association, élément (CSDL)

Un **Association** élément définit une relation entre deux types d’entités. Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ». La multiplicité d’une terminaison d’association peut avoir une valeur d’un (1), zéro ou un (0.. 1) ou plusieurs (\*). Ces informations sont spécifiées dans les deux éléments de fin d’enfants.

Il est possible d'accéder aux instances de type d'entité au niveau d'une terminaison d'une association via les propriétés de navigation ou les clés étrangères, si elles sont exposées sur un type d'entité.

Dans une application, une instance d'une association représente une association spécifique entre des instances de types d'entités. Les instances d'association sont regroupées logiquement dans un ensemble d'associations.

Un **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Fin (exactement 2 éléments)
-   ReferentialConstraint (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Association** élément.

| Nom d'attribut | Requis | Value                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | Oui         | Nom de l'association. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association de clés étrangères n’ont pas été exposées sur le **client** et  **Commande** types d’entité. Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**. En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext est supprimé. Si le **client** est supprimé.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association lorsque les clés étrangères ont été exposées sur le **client** et  **Commande** types d’entité. Avec des clés étrangères exposées, la relation entre les entités est gérée avec un **ReferentialConstraint** élément. Un élément AssociationSetMapping correspondant n’est pas nécessaire pour mapper cette association à la source de données.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>AssociationSet, élément (CSDL)

Le **AssociationSet** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les instances d’association du même type. Un ensemble d'associations fournit une définition pour le regroupement d'instances d'association afin qu'elles puissent être mappées à une source de données.  

Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément autorisé)
-   Fin (exactement deux éléments requis)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

Le **Association** attribut spécifie le type d’association qui contient un ensemble d’associations. Les jeux d’entités qui composent les terminaisons d’un ensemble d’associations sont spécifiés avec exactement deux enfants **fin** éléments.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.

| Nom d'attribut  | Requis | Value                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | Oui         | Nom du jeu d'entités. La valeur de la **nom** attribut ne peut pas être identique à la valeur de la **Association** attribut.                                 |
| **Association** | Oui         | Nom qualifié complet de l'association dont l'ensemble d'associations contient des instances. L'association doit être dans le même espace de noms que l'ensemble d'associations. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments :

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>Élément CollectionType (CSDL)

Le **CollectionType** élément de langage de définition de schéma conceptuel (CSDL) Spécifie qu’une fonction ou un paramètre de fonction de type de retour est une collection. Le **CollectionType** élément peut être un enfant de l’élément de paramètre ou de l’élément ReturnType (fonction). Le type de collection peut être spécifié en utilisant le **Type** attribut ou l’un des éléments enfants suivants :

-   **CollectionType**
-   ReferenceType ;
-   RowType ;
-   TypeRef

> [!NOTE]
> Un modèle ne sera pas validé si le type d’une collection est spécifié avec à la fois le **Type** attribut et un élément enfant.

 

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **CollectionType** élément. Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables aux collections de **types Edmsimpletype**.

| Nom d'attribut                                                          | Requis | Value                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | Non          | Type de la collection.                                                                                                                                                                                                      |
| **Nullable**                                                            | Non          | **True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null. <br/> [!NOTE]                                                                                                                 |
| > Dans le langage CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | Non          | Valeur par défaut de la propriété.                                                                                                                                                                                               |
| **MaxLength**                                                           | Non          | Longueur maximale de la valeur de propriété.                                                                                                                                                                                        |
| **FixedLength**                                                         | Non          | **True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.                                                                                                                           |
| **Précision**                                                           | Non          | Précision de la valeur de propriété.                                                                                                                                                                                             |
| **Échelle**                                                               | Non          | Échelle de la valeur de propriété.                                                                                                                                                                                                 |
| **SRID**                                                                | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux.   Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Non          | **True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.                                                                                                                                |
| **classement**                                                           | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                    |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant illustre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de **personne** types d’entités (comme spécifié par le **ElementType** attribut).

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

L’exemple suivant montre une fonction définie par modèle qui utilise le **CollectionType** élément pour spécifier que la fonction accepte en tant que paramètre à une collection de **département** types d’entité.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>ComplexType, élément (CSDL)

Un **ComplexType** élément définit une structure de données composée de **EdmSimpleType** propriétés ou autres types complexes.  Un type complexe peut être une propriété d'un type d'entité ou d'un autre type complexe. Un type complexe est semblable à un type d'entité du fait qu'un type complexe définit des données. Toutefois, il existe des différences clés entre les types complexes et les types d'entités :

-   Les types complexes n'ont pas d'identité (ni de clés), par conséquent, ils ne peuvent pas exister indépendamment. Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.
-   Les types complexes ne peuvent pas participer aux associations. Ni la fin d’une association peut être un type complexe, et par conséquent les propriétés de navigation ne peut pas être définies pour les types complexes.
-   Une propriété de type complexe ne peut pas avoir de valeur NULL, bien que les propriétés scalaires d'un type complexe puissent chacune être définie sur NULL.

Un **ComplexType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Propriété (zéro ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ComplexType** élément.

| Nom d'attribut                                                                                                 | Requis | Value                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                                                                                                           | Oui         | Nom du type complexe. Le nom d'un type complexe ne peut pas être identique au nom d'un autre type complexe, d'un type d'entité ou d'une association qui figure dans l'étendue du modèle. |
| BaseType                                                                                                       | Non          | Nom d'un autre type complexe qui est le type de base du type complexe en cours de définition. <br/> [!NOTE]                                                                     |
| > Cet attribut n’est pas applicable dans CSDL v1. L'héritage pour les types complexes n'est pas pris en charge dans cette version. |             |                                                                                                                                                                                     |
| Abstract                                                                                                       | Non          | **True** ou **False** (la valeur par défaut) selon que le type complexe est un type abstrait. <br/> [!NOTE]                                                                  |
| > Cet attribut n’est pas applicable dans CSDL v1. Les types complexes dans cette version ne peuvent pas être des types abstraits.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ComplexType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant illustre un type complexe, **adresse**, avec le **EdmSimpleType** propriétés **StreetAddress**, **Ville**,  **StateOrProvince**, **pays**, et **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Pour définir le type complexe **adresse** (ci-dessus) en tant que propriété d’un type d’entité, vous devez déclarer le type de propriété dans la définition de type d’entité. L’exemple suivant montre le **adresse** propriété comme un type complexe sur un type d’entité (**Publisher**) :

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression, élément (CSDL)

Le **DefiningExpression** élément de langage de définition de schéma conceptuel (CSDL) contient une expression Entity SQL qui définit une fonction dans le modèle conceptuel.  

> [!NOTE]
> À des fins de validation, un **DefiningExpression** élément peut contenir du contenu arbitraire. Toutefois, Entity Framework lève une exception lors de l’exécution si un **DefiningExpression** élément ne contient pas de valide Entity SQL.

 

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **DefiningExpression** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant utilise un **DefiningExpression** élément pour définir une fonction qui retourne le nombre d’années depuis un ouvrage a été publié. Le contenu de la **DefiningExpression** élément est écrit dans Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent, élément (CSDL)

Le **dépendants** élément dans le langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint et définit la terminaison dépendante d’une contrainte référentielle. Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle. De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité. Le type d’entité référencé est appelé le *terminaison principale* de la contrainte. Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte. **PropertyRef** éléments sont utilisés pour spécifier les clés qui référencent la terminaison principale.

Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   PropertyRef (un ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **dépendants** élément.

| Nom d'attribut | Requis | Value                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rôle**       | Oui         | Nom du type d'entité au niveau de la terminaison dépendante de l'association. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association. Le **PublisherId** propriété de la **livre** type d’entité compose la terminaison dépendante de la contrainte référentielle.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Documentation, élément (CSDL)

Le **Documentation** élément dans le langage de définition de schéma conceptuel (CSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent. Dans un fichier .edmx, lorsque le **Documentation** élément est un enfant d’un élément qui apparaît sous la forme d’un objet sur l’aire de conception du Concepteur EF (par exemple, une entité, associations ou une propriété), le contenu de la **Documentation**  élément apparaîtra dans Visual Studio **propriétés** fenêtre de l’objet.

Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   **Résumé**: une brève description de l’élément parent. (zéro ou un élément).
-   **LongDescription**: une description détaillée de l’élément parent. (zéro ou un élément).
-   Éléments d’annotation. (zéro, un ou plusieurs éléments).

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType. Si l’extrait de code ci-dessous ont été dans le langage CSDL contenu d’un .edmx de fichiers, le contenu de la **Résumé** et **LongDescription** éléments apparaîtrait dans Visual Studio **propriétés** fenêtre lorsque vous cliquez sur le `Customer` type d’entité.

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>End, élément (CSDL)

Le **fin** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément Association ou de l’élément AssociationSet. Dans chaque cas, le rôle de la **fin** élément est différent et les attributs applicables sont différents.

### <a name="end-element-as-a-child-of-the-association-element"></a>Élément End comme enfant de l'élément Association

Un **fin** élément (en tant qu’enfant de le **Association** élément) identifie le type d’entité sur une extrémité d’une association et le nombre d’instances de type d’entité qui peuvent exister à cette fin d’une association. Les terminaisons d'association sont définies dans le cadre d'une association ; une association doit avoir exactement deux terminaisons d'association. Il est possible d'accéder aux instances de type d'entité au niveau de la terminaison d'une association via les propriétés de navigation ou les clés étrangères si elles sont exposées sur un type d'entité.  

Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   OnDelete (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.

| Nom d'attribut   | Requis | Value                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Oui         | Nom du type d'entité au niveau de la terminaison de l'association.                                                                                                                                                                                                                                                                                                                                                         |
| **Rôle**         | Non          | Nom de la terminaison de l'association. Si aucun nom n'est fourni, le nom du type d'entité au niveau de la terminaison de l'association sera utilisé.                                                                                                                                                                                                                                                                                           |
| **Multiplicité** | Oui         | **1**, **valeur 0.. 1**, ou **\*** selon le nombre d’instances de type d’entité qui peut être à la fin de l’association. <br/> **1** indique cette instance de type exactement une entité existe à la fin de l’association. <br/> **valeur 0.. 1** indique que zéro ou une instance de type d’entité existe au niveau de la terminaison d’association. <br/> **\*** Indique que zéro, une ou plusieurs instances de type d’entité existent à la fin de l’association. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association. Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**. En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et qui ont été chargés dans ObjectContext sera if supprimé le **client** est supprimé.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Élément End comme enfant de l'élément AssociationSet

Le **fin** élément spécifie une extrémité d’un ensemble d’associations. Le **AssociationSet** élément doit contenir deux **fin** éléments. Les informations contenues dans un **fin** élément est utilisé pour mapper une ensemble d’associations à une source de données.

Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

> [!NOTE]
> Les éléments d'annotation doivent figurer après tous les autres éléments enfants. Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.

 

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Oui         | Le nom de la **EntitySet** élément qui définit une extrémité du parent **AssociationSet** élément. Le **EntitySet** élément doit être défini dans le même conteneur d’entités en tant que parent **AssociationSet** élément. |
| **Rôle**       | Non          | Nom de la terminaison de l'ensemble d'associations. Si le **rôle** attribut n’est pas utilisé, le nom de l’ensemble d’associations sera le nom du jeu d’entités.                                                                   |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments, chacun avec deux **fin** éléments :

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer, élément (CSDL)

Le **EntityContainer** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les jeux d’entités, les ensembles d’associations et les importations de fonction. Un conteneur d’entités de modèle conceptuel est mappé à un conteneur d’entités du modèle via l’élément EntityContainerMapping stockage. Un conteneur d'entités de modèle de stockage décrit la structure de la base de données : les jeux d'entités décrivent les tables, les ensembles d'associations décrivent les contraintes de clé étrangère et les importations de fonction décrivent les procédures stockées dans une base de données.

Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation. Si un **Documentation** élément est présent, il doit précéder tous les **EntitySet**, **AssociationSet**, et **FunctionImport** éléments.

Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :

-   EntitySet ;
-   AssociationSet ;
-   FunctionImport ;
-   éléments d'annotation.

Vous pouvez étendre un **EntityContainer** élément pour inclure le contenu d’un autre **EntityContainer** qui figure dans le même espace de noms. Pour inclure le contenu d’un autre **EntityContainer**, dans le référencement **EntityContainer** élément, définissez la valeur de la **étend** nom à l’attribut de la  **EntityContainer** élément que vous souhaitez inclure. Tous les éléments enfants de l’élément inclus **EntityContainer** élément est traité comme éléments enfants de la référence **EntityContainer** élément.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Using** élément.

| Nom d'attribut | Requis | Value                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Oui         | Nom du conteneur d'entités.                               |
| **Étend**    | Non          | Le nom d'un autre conteneur d'entités au sein du même espace de noms. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément qui définit trois jeux d’entités et les deux ensembles d’associations.

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>EntitySet, élément (CSDL)

Le **EntitySet** élément dans le langage de définition de schéma conceptuel est un conteneur logique pour les instances d’un type d’entité et les instances de n’importe quel type dérivé de ce type d’entité. La relation entre un type d'entité et un jeu d'entités est analogue à la relation entre une ligne et une table dans une base de données relationnelle. Comme une ligne, un type d'entité définit un jeu de données connexes et, comme une table, un jeu d'entités contient des instances de cette définition. Un jeu d'entités fournit une construction pour le regroupement d'instances du type d'entité, afin qu'elles puissent être mappées aux structures de données associées dans une source de données.  

Plusieurs jeux d'entités peuvent être définis pour un type d'entité particulier.

> [!NOTE]
> Le Concepteur EF ne prend pas en charge les modèles conceptuels qui contiennent des jeux d’entités multiples par type.

 

Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation, élément (zéro ou un élément autorisé)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.

| Nom d'attribut | Requis | Value                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du jeu d'entités.                                                              |
| **EntityType** | Oui         | Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément avec trois **EntitySet** éléments :

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

Il est possible de définir des jeux d'entités multiples par type (MEST). L’exemple suivant définit un conteneur d’entités avec deux jeux d’entités pour le **livre** type d’entité :

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>EntityType, élément (CSDL)

Le **EntityType** élément représente la structure d’un concept de niveau supérieur, tel qu’un client ou l’ordre, dans un modèle conceptuel. Un type d'entité est un modèle pour des instances de types d'entités dans une application. Chaque modèle contient les informations suivantes :

-   Nom unique. (Obligatoire.)
-   Clé d'entité définie par une ou plusieurs propriétés. (Obligatoire.)
-   Propriétés pour contenir des données. (Facultatif)
-   Propriétés de navigation qui permettent de naviguer d'une terminaison d'une association à l'autre terminaison. (Facultatif)

Dans une application, une instance d'un type d'entité représente un objet spécifique (tel qu'un client ou une commande spécifique). Chaque instance d'un type d'entité doit avoir une clé d'entité unique dans un jeu d'entités.

Deux instances de type d'entité sont considérées égales seulement si elles sont du même type et si les valeurs de leurs clés d'entité sont identiques.

Un **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Clé (zéro ou un élément)
-   Propriété (zéro ou plusieurs éléments)
-   NavigationProperty (zéro ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.

| Nom d'attribut                                                                                                                                  | Requis | Value                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | Oui         | Nom du type d'entité.                                                                     |
| **BaseType**                                                                                                                                    | Non          | Nom d'un autre type d'entité qui est le type de base du type d'entité en cours de définition.  |
| **Abstract**                                                                                                                                    | Non          | **True** ou **False**, selon que le type d’entité est un type abstrait.                 |
| **OpenType**                                                                                                                                    | Non          | **True** ou **False** selon que le type d’entité est un type d’entité ouverte. <br/> [!NOTE] |
| > Le **OpenType** attribut s’applique uniquement aux types d’entités qui sont définies dans les modèles conceptuels utilisés avec ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments et deux **NavigationProperty** éléments :

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>EnumType, élément (CSDL)

Le **EnumType** élément représente un type énuméré.

Un **EnumType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Membre (zéro ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EnumType** élément.

| Nom d'attribut     | Requis | Value                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | Oui         | Nom du type d'entité.                                                                                                                                                                  |
| **IsFlags**        | Non          | **True** ou **False**, selon si le type enum peut être utilisé comme un jeu d’indicateurs. La valeur par défaut est **False.**.                                                                     |
| **UnderlyingType** | Non          | **Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** ou **Edm.SByte** définition de la plage de valeurs du type.   La valeur par défaut, le type sous-jacent d’éléments de l’énumération est **Edm.Int32.**. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EnumType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function, élément (CSDL)

Le **fonction** élément dans le langage de définition de schéma conceptuel (CSDL) est utilisé pour définir ou déclarer des fonctions dans le modèle conceptuel. Une fonction est définie à l’aide d’un élément DefiningExpression.  

Un **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Paramètre (zéro ou plusieurs éléments)
-   DefiningExpression (zéro ou un élément)
-   ReturnType (fonction) (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (Function) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux. Les types de retour possibles correspondent à tout type EdmSimpleType, type d'entité, type complexe, type de ligne ou type REF (ou à une collection de l'un de ces types).

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **fonction** élément.

| Nom d'attribut | Requis | Value                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | Oui         | Nom de la fonction.          |
| **ReturnType** | Non          | Type retourné par la fonction. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années écoulées depuis l’embauche d’un formateur.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport, élément (CSDL)

Le **FunctionImport** élément de langage de définition de schéma conceptuel (CSDL) représente une fonction qui est définie dans la source de données mais disponible pour les objets via le modèle conceptuel. Par exemple, un élément de la fonction dans le modèle de stockage peut être utilisé pour représenter une procédure stockée dans une base de données. Un **FunctionImport** élément dans le modèle conceptuel représente la fonction correspondante dans une application Entity Framework et est mappé à la fonction de modèle de stockage à l’aide de l’élément FunctionImportMapping. Lorsque la fonction est appelée dans l'application, la procédure stockée correspondante est exécutée dans la base de données.

Le **FunctionImport** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément autorisé)
-   Paramètre (zéro ou plusieurs éléments autorisés)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)
-   ReturnType (FunctionImport) (zéro ou plusieurs éléments autorisés)

Un **paramètre** élément doit être défini pour chaque paramètre qui accepte la fonction.

Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (FunctionImport) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux. La valeur de type de retour doit être une collection de type EdmSimpleType, EntityType ou ComplexType.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.

| Nom d'attribut   | Requis | Value                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Oui         | Nom de la fonction importée.                                                                                                                                                                    |
| **ReturnType**   | Non          | Type retourné par la fonction. N'utilisez pas cet attribut si la fonction ne retourne pas de valeur. Sinon, la valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.        |
| **EntitySet**    | Non          | Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection. Sinon, le **EntitySet** attribut ne doit pas être utilisé. |
| **IsComposable** | Non          | Si la valeur est définie sur true, la fonction est composable (Table-valued Function) et peut être utilisée dans une requête LINQ.  La valeur par défaut est **false**.                                                           |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **FunctionImport** élément qui accepte un paramètre et retourne une collection de types d’entités :

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key, élément (CSDL)

Le **clé** élément est un élément enfant de l’élément EntityType et définit un *clé d’entité* (une propriété ou un ensemble de propriétés qui déterminent l’identité d’un type d’entité). Les propriétés qui composent une clé d'entité sont choisies au moment du design. Les valeurs des propriétés de clé d'entité doivent identifier de façon unique une instance de type d'entité dans un jeu d'entités au moment de l'exécution. Les propriétés qui composent une clé d'entité doivent être choisies pour garantir l'unicité des instances dans un jeu d'entités. Le **clé** élément définit une clé d’entité en faisant référence à un ou plusieurs des propriétés d’un type d’entité.

Le **clé** élément peut avoir les éléments enfants suivants :

-   PropertyRef (un ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **clé** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple ci-dessous définit un type d’entité nommé **livre**. La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

Le **ISBN** propriété est un bon choix pour la clé d’entité, car un International Standard Book nombre (ISBN) identifie de façon unique un livre.

L’exemple suivant montre un type d’entité (**auteur**) qui a une clé d’entité qui se compose de deux propriétés : **nom** et **adresse**.

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

À l’aide de **nom** et **adresse** pour l’entité de clé est un choix raisonnable, étant donné que deux auteurs portant le même nom sont peu susceptibles de vie à la même adresse. Toutefois, ce choix pour une clé d'entité ne garantit pas vraiment l'unicité des clés d'entité dans un jeu d'entités. Ajout d’une propriété, tel que **AuthorId**, qui peut être utilisé pour identifier de manière unique un auteur est recommandé dans ce cas.

 

## <a name="member-element-csdl"></a>Member, élément (CSDL)

Le **membre** élément est un élément enfant de l’élément EnumType et définit un membre du type énuméré.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du membre.                                                                                                                                                                  |
| **Valeur**      | Non          | Valeur du membre. Par défaut, le premier membre a la valeur 0, et la valeur de chaque énumérateur successif est incrémentée de 1. Il existe peut-être plusieurs membres avec les mêmes valeurs. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty, élément (CSDL)

Un **NavigationProperty** élément définit une propriété de navigation, qui fournit une référence à l’autre extrémité d’une association. Contrairement aux propriétés définies par l’élément de propriété, les propriétés de navigation ne définissent pas la forme et les caractéristiques des données. Elles fournissent un moyen d'explorer une association entre deux types d'entités.

Notez que les propriétés de navigation sont facultatives sur les deux types d'entités au niveau des terminaisons d'une association. Si vous définissez une propriété de navigation sur un type d'entité au niveau de la terminaison d'une association, il n'est pas nécessaire de définir une propriété de navigation sur le type d'entité à l'autre terminaison de l'association.

Le type de données retourné par une propriété de navigation est déterminé par la multiplicité de sa terminaison d'association distante. Par exemple, une propriété de navigation, **OrdersNavProp**, existe sur un **client** type d’entité et de naviguer d’une association un-à-plusieurs entre **client** et  **Commande**. Étant donné que la terminaison d’association distante pour la propriété de navigation a une multiplicité plusieurs (\*), son type de données est une collection (de **ordre**). De même, si une propriété de navigation, **CustomerNavProp**, existe sur le **ordre** type d’entité, son type de données serait **client** dans la mesure où la multiplicité de la terminaison distante est un (1).

Un **NavigationProperty** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **NavigationProperty** élément.

| Nom d'attribut   | Requis | Value                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Oui         | Nom de la propriété de navigation.                                                                                                                                                                                                             |
| **Relation** | Oui         | Nom d'une association figurant dans l'étendue du modèle.                                                                                                                                                                                |
| **ToRole**       | Oui         | Terminaison de l'association à laquelle la navigation prend fin. La valeur de la **ToRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd).       |
| **FromRole**     | Oui         | Terminaison de l'association où la navigation commence. La valeur de la **FromRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd). |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **NavigationProperty** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant définit un type d’entité (**livre**) avec deux propriétés de navigation (**PublishedBy** et **inscrite**) :

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>OnDelete, élément (CSDL)

Le **OnDelete** élément dans le langage de définition de schéma conceptuel (CSDL) définit le comportement qui est connecté avec une association. Si le **Action** attribut a la valeur **Cascade** sur une extrémité d’une association, liées à des types d’entités à l’autre extrémité de l’association sont supprimés lorsque le type d’entité de la première terminaison est supprimé. Si l’association entre deux types d’entité est une relation clé primaire / clé primaire, un objet dépendant chargé est supprimé lorsque l’objet principal à l’autre extrémité de l’association est supprimé, indépendamment de la **OnDelete** spécification.  

> [!NOTE]
> Le **OnDelete** élément affecte uniquement le comportement d’exécution d’une application ; elle n’affecte pas le comportement de la source de données. Le comportement défini dans la source de données doit être le même que le comportement défini dans l'application.

 

Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Action**     | Oui         | **Cascade** ou **aucun**. Si **Cascade**, types d’entités dépendants sont supprimés lorsque le type d’entité principal est supprimé. Si **aucun**, types d’entités dépendants ne sont pas supprimés lorsque le type d’entité principal est supprimé. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association. Le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext seront supprimés lorsque le  **Client** est supprimé.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter, élément (CSDL)

Le **paramètre** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément FunctionImport ou l’élément Function.

### <a name="functionimport-element-application"></a>Application de l'élément FunctionImport

Un **paramètre** élément (en tant qu’enfant de le **FunctionImport** élément) est utilisé pour définir les paramètres d’entrée et de sortie pour les importations de fonction qui sont déclarées dans le langage CSDL.

Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément autorisé)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du paramètre.                                                                                                                                                                                                      |
| **Type**       | Oui         | Type du paramètre. La valeur doit être un **EDMSimpleType** ou un type complexe qui est dans l’étendue du modèle.                                                                                                             |
| **Mode**       | Non          | **Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.                                                                                                                |
| **MaxLength**  | Non          | Longueur maximale autorisée du paramètre.                                                                                                                                                                                    |
| **Précision**  | Non          | Précision du paramètre.                                                                                                                                                                                                 |
| **Échelle**      | Non          | Échelle du paramètre.                                                                                                                                                                                                     |
| **SRID**       | Non          | Identificateur de référence spatiale système. Valide uniquement pour les paramètres des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant montre un **FunctionImport** élément avec un **paramètre** élément enfant. La fonction accepte un paramètre d'entrée et retourne une collection de types d'entités.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Application de l'élément Function

Un **paramètre** élément (en tant qu’enfant de le **fonction** élément) définit les paramètres pour les fonctions qui sont définies ou déclarées dans un modèle conceptuel.

Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   CollectionType (zéro ou un élément)
-   ReferenceType (zéro ou un élément)
-   RowType (zéro ou un élément)

> [!NOTE]
> Seul l’un de la **CollectionType**, **ReferenceType**, ou **RowType** éléments peuvent être un élément enfant d’un **propriété** élément.

 

-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

> [!NOTE]
> Les éléments d'annotation doivent figurer après tous les autres éléments enfants. Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.

 

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.

| Nom d'attribut   | Requis | Value                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Oui         | Nom du paramètre.                                                                                                                                                                                                      |
| **Type**         | Non          | Type du paramètre. Un paramètre peut correspondre à l'un quelconque des types suivants (ou à des collections de ces types) : <br/> **EdmSimpleType** <br/> type d'entité <br/> type complexe <br/> type de ligne <br/> type référence                             |
| **Nullable**     | Non          | **True** (la valeur par défaut) ou **False** selon si la propriété peut avoir un **null** valeur.                                                                                                                          |
| **DefaultValue** | Non          | Valeur par défaut de la propriété.                                                                                                                                                                                              |
| **MaxLength**    | Non          | Longueur maximale de la valeur de propriété.                                                                                                                                                                                       |
| **FixedLength**  | Non          | **True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.                                                                                                                          |
| **Précision**    | Non          | Précision de la valeur de propriété.                                                                                                                                                                                            |
| **Échelle**        | Non          | Échelle de la valeur de propriété.                                                                                                                                                                                                |
| **SRID**         | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Non          | **True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.                                                                                                                               |
| **classement**    | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                   |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant montre un **fonction** élément qui utilise un **paramètre** élément enfant pour définir un paramètre de fonction.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal, élément (CSDL)

Le **Principal** en langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte référentielle. Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle. De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité. Le type d’entité référencé est appelé le *terminaison principale* de la contrainte. Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte. **PropertyRef** éléments sont utilisés pour spécifier les clés référencées par la terminaison dépendante.

Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   PropertyRef (un ou plusieurs éléments)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Principal** élément.

| Nom d'attribut | Requis | Value                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rôle**       | Oui         | Nom du type d'entité au niveau de la terminaison principale de l'association. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **ReferentialConstraint** élément qui fait partie de la définition de la **PublishedBy** association. Le **Id** propriété de la **Publisher** type d’entité compose la terminaison principale de la contrainte référentielle.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Property, élément (CSDL)

Le **propriété** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément EntityType, ComplexType, élément ou l’élément RowType.

### <a name="entitytype-and-complextype-element-applications"></a>Applications des éléments EntityType et ComplexType

**Propriété** éléments (en tant qu’enfants de **EntityType** ou **ComplexType** éléments) définissent la forme et les caractéristiques des données qui contient une instance de type d’entité ou d’une instance de type complexe . Les propriétés dans un modèle conceptuel sont analogues aux propriétés qui sont définies sur une classe. De même que les propriétés sur une classe définissent la forme de la classe et acheminent des informations sur les objets, les propriétés dans un modèle conceptuel définissent la forme d'un type d'entité et acheminent des informations sur les instances de type d'entité.

Le **propriété** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation, élément (zéro ou un élément autorisé)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

Les facettes suivantes peuvent être appliqués à un **propriété** élément : **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **précision**, **mise à l’échelle**, **Unicode**, **classement**,  **ConcurrencyMode**. Les facettes sont des attributs XML qui fournissent des informations sur la manière dont les valeurs de propriété sont stockées dans la banque de données.

> [!NOTE]
> Facettes peuvent uniquement être appliqués aux propriétés de type **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.

| Nom d'attribut                                                         | Requis | Value                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | Oui         | Nom de la propriété.                                                                                                                                                                                                       |
| **Type**                                                               | Oui         | Type de la valeur de propriété. Le type de valeur de propriété doit être un **EDMSimpleType** ou un type complexe (indiqué par un nom qualifié complet) qui se trouve dans la portée du modèle.                                                 |
| **Nullable**                                                           | Non          | **True** (la valeur par défaut) ou <strong>False</strong> selon si la propriété peut avoir une valeur null. <br/> [!NOTE]                                                                                                   |
| > Dans le langage CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | Non          | Valeur par défaut de la propriété.                                                                                                                                                                                              |
| **MaxLength**                                                          | Non          | Longueur maximale de la valeur de propriété.                                                                                                                                                                                       |
| **FixedLength**                                                        | Non          | **True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.                                                                                                                          |
| **Précision**                                                          | Non          | Précision de la valeur de propriété.                                                                                                                                                                                            |
| **Échelle**                                                              | Non          | Échelle de la valeur de propriété.                                                                                                                                                                                                |
| **SRID**                                                               | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Non          | **True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.                                                                                                                               |
| **classement**                                                          | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Non          | **Aucun** (la valeur par défaut) ou **fixe**. Si la valeur est définie sur **fixe**, la valeur de propriété sera utilisée dans les contrôles d’accès concurrentiel optimiste.                                                                                  |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments :

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

L’exemple suivant montre un **ComplexType** élément avec cinq **propriété** éléments :

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Application de l'élément RowType

**Propriété** éléments (comme enfants d’un **RowType** élément) définissent la forme et les caractéristiques des données qui peuvent être passées à ou retournées à partir d’une fonction définie par le modèle.  

Le **propriété** élément peut avoir exactement un des éléments enfants suivants :

-   CollectionType ;
-   ReferenceType ;
-   RowType ;

Le **propriété** élément peut avoir n’importe quel nombre éléments d’annotation enfant.

> [!NOTE]
> Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.

 

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.

| Nom d'attribut                                                     | Requis | Value                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | Oui         | Nom de la propriété.                                                                                                                                                                                                       |
| **Type**                                                           | Oui         | Type de la valeur de propriété.                                                                                                                                                                                                 |
| **Nullable**                                                       | Non          | **True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null. <br/> [!NOTE]                                                                                                                |
| > Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Non          | Valeur par défaut de la propriété.                                                                                                                                                                                              |
| **MaxLength**                                                      | Non          | Longueur maximale de la valeur de propriété.                                                                                                                                                                                       |
| **FixedLength**                                                    | Non          | **True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.                                                                                                                          |
| **Précision**                                                      | Non          | Précision de la valeur de propriété.                                                                                                                                                                                            |
| **Échelle**                                                          | Non          | Échelle de la valeur de propriété.                                                                                                                                                                                                |
| **SRID**                                                           | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Non          | **True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.                                                                                                                               |
| **classement**                                                      | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                   |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

#### <a name="example"></a>Exemple

L’exemple suivant **propriété** éléments utilisés pour définir la forme du type de retour d’une fonction définie par modèle.

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef, élément (CSDL)

Le **PropertyRef** élément dans le langage de définition de schéma conceptuel (CSDL) fait référence à une propriété d’un type d’entité pour indiquer que la propriété effectuera l’un des rôles suivants :

-   Partie de la clé de l'entité (une propriété ou un jeu de propriétés d'un type d'entité qui détermine l'identité). Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé d’entité.
-   Terminaison dépendante ou principale d'une contrainte référentielle.

Le **PropertyRef** élément peut avoir uniquement des éléments d’annotation (zéro ou plus) en tant qu’éléments enfants.

> [!NOTE]
> Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.

 

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.

| Nom d'attribut | Requis | Value                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Oui         | Nom de la propriété référencée. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple ci-dessous définit un type d’entité (**livre**). La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

Dans l’exemple suivant, deux **PropertyRef** éléments sont utilisés pour indiquer que deux propriétés (**Id** et **PublisherId**) sont les terminaisons principales et dépendantes d’un référentiel contrainte.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>Élément ReferenceType (CSDL)

Le **ReferenceType** élément de langage de définition de schéma conceptuel (CSDL) spécifie une référence à un type d’entité. Le **ReferenceType** élément peut être un enfant des éléments suivants :

-   ReturnType (fonction)
-   Paramètre
-   CollectionType ;

Le **ReferenceType** élément est utilisé lors de la définition d’un type de paramètre ou de retour pour une fonction.

Un **ReferenceType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ReferenceType** élément.

| Nom d'attribut | Requis | Value                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | Oui         | Nom du type d'entité référencé. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferenceType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **paramètre** élément dans une fonction définie par modèle qui accepte une référence à un **personne** entité type :

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **ReturnType** élément (fonction) dans une fonction définie par modèle qui retourne une référence à un **personne**type d’entité :

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint, élément (CSDL)

Un **ReferentialConstraint** élément dans le langage de définition de schéma conceptuel (CSDL) définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle. De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité. Le type d’entité référencé est appelé le *terminaison principale* de la contrainte. Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.

Si une clé étrangère qui est exposée sur un type d’entité fait référence à une propriété sur un autre type d’entité, le **ReferentialConstraint** élément définit une association entre les deux types d’entité. Étant donné que le **ReferentialConstraint** élément fournit des informations sur la manière dont deux types d’entités sont liées, aucun élément AssociationSetMapping correspondant n’est nécessaire dans le langage de spécification de mappage (MSL). Une association entre deux types d’entités qui n’ont pas de clés étrangères exposées doit avoir un correspondant **AssociationSetMapping** élément afin de mapper les informations d’association à la source de données.

Si une clé étrangère n’est pas exposée sur un type d’entité, le **ReferentialConstraint** élément peut uniquement définir une contrainte de clé primaire / clé primaire entre le type d’entité et un autre type d’entité.

Un **ReferentialConstraint** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Principal (exactement un élément)
-   Dépendante (exactement un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le **ReferentialConstraint** élément peut avoir n’importe quel nombre d’attributs d’annotation (attributs XML personnalisés). Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>Élément ReturnType (fonction) (CSDL)

Le **ReturnType** (Function), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément de la fonction. Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.

Retourner les types peuvent être les **EdmSimpleType**, type d’entité, type complexe, type de ligne, type ref ou une collection d’un de ces types.

Le type de retour d’une fonction peut être spécifié avec soit le **Type** attribut de la **ReturnType** élément (fonction), ou avec l’un des éléments enfants suivants :

-   CollectionType ;
-   ReferenceType ;
-   RowType ;

> [!NOTE]
> Un modèle ne sera pas validé si vous spécifiez une fonction de type de retour avec à la fois le **Type** attribut de la **ReturnType** élément (Function) et l’autre des éléments enfants.

 

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** élément (Function).

| Nom d'attribut | Requis | Value                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Non          | Type retourné par la fonction. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément (Function). Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années un livre a été imprimé. Notez que le type de retour est spécifié par le **Type** attribut d’un **ReturnType** élément (Function).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) élément (CSDL)

Le **ReturnType** (FunctionImport), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément FunctionImport. Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.

Retourner les types peuvent être n’importe quelle collection de type d’entité, type complexe, ou **EdmSimpleType**,

Le type de retour d’une fonction est spécifié avec la **Type** attribut de la **ReturnType** (FunctionImport) élément.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** (FunctionImport) élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | Non          | Type retourné par la fonction. La valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.                                                                                      |
| **EntitySet**  | Non          | Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection. Sinon, le **EntitySet** attribut ne doit pas être utilisé. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** (FunctionImport) élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant utilise un **FunctionImport** qui retourne la documentation et les serveurs de publication. Notez que la fonction retourne deux jeux de résultats et par conséquent deux **ReturnType** (FunctionImport) éléments sont spécifiés.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Élément RowType (CSDL)

Un **RowType** élément dans le langage de définition de schéma conceptuel (CSDL) définit une structure sans nom comme paramètre ou type de retour pour une fonction définie dans le modèle conceptuel.

Un **RowType** élément peut être l’enfant des éléments suivants :

-   CollectionType ;
-   Paramètre
-   ReturnType (fonction)

Un **RowType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Propriété (un ou plusieurs)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **RowType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>Schema, élément (CSDL)

Le **schéma** élément est l’élément racine d’une définition de modèle conceptuel. Il contient des définitions pour les objets, fonctions et conteneurs qui composent un modèle conceptuel.

Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   Association
-   ComplexType
-   Fonction

Un **schéma** élément peut contenir zéro ou un élément d’Annotation.

> [!NOTE]
> Le **fonction** élément et les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.

 

Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour le type d’entité, type complexe et objets d’association dans un modèle conceptuel. Dans un espace de noms, deux objets ne peuvent pas avoir le même nom. Espaces de noms peuvent couvrir plusieurs **schéma** éléments et plusieurs fichiers .csdl.

Un espace de noms du modèle conceptuel est différent de l’espace de noms XML de la **schéma** élément. Un espace de noms de modèle conceptuel (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités, types complexes et types d’associations. L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément. Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage CSDL. Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espace de noms**  | Oui         | Espace de noms du modèle conceptuel. La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type. Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms Simple.Example.Model, le nom qualifié complet de le **EntityType** est SimpleExampleModel.Customer. <br/> Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**. La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma SSDL. |
| **Alias**      | Non          | Identificateur utilisé à la place du nom de l'espace de noms. Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms Simple.Example.Model et la valeur de la **Alias** attribut est *modèle*, vous pouvez utiliser modèle.client comme nom qualifié complet de le **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **schéma** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>Élément TypeRef (CSDL)

Le **TypeRef** élément dans le langage de définition de schéma conceptuel (CSDL) fournit une référence à un type nommé existant. Le **TypeRef** élément peut être un enfant de l’élément CollectionType, qui est utilisé pour spécifier qu’une fonction a une collection comme un paramètre ou type de retour.

Un **TypeRef** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **TypeRef** élément. Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables à **types Edmsimpletype**.

| Nom d'attribut                                                     | Requis | Value                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | Non          | Nom du type référencé.                                                                                                                                                                                          |
| **Nullable**                                                       | Non          | **True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null. <br/> [!NOTE]                                                                                                                |
| > Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Non          | Valeur par défaut de la propriété.                                                                                                                                                                                              |
| **MaxLength**                                                      | Non          | Longueur maximale de la valeur de propriété.                                                                                                                                                                                       |
| **FixedLength**                                                    | Non          | **True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.                                                                                                                          |
| **Précision**                                                      | Non          | Précision de la valeur de propriété.                                                                                                                                                                                            |
| **Échelle**                                                          | Non          | Échelle de la valeur de propriété.                                                                                                                                                                                                |
| **SRID**                                                           | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Non          | **True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.                                                                                                                               |
| **classement**                                                      | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                   |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre une fonction définie par modèle qui utilise le **TypeRef** élément (en tant qu’enfant d’un **CollectionType** élément) pour spécifier que la fonction accepte une collection de  **Département** types d’entité.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>Using, élément (CSDL)

Le **Using** élément dans le langage de définition de schéma conceptuel (CSDL) importe le contenu d’un modèle conceptuel qui existe dans un espace de noms différent. En définissant la valeur de la **Namespace** attribut, vous pouvez faire référence à des types d’entités, types complexes et les types d’associations sont définies dans un autre modèle conceptuel. Plusieurs **Using** élément peut être un enfant d’un **schéma** élément.

> [!NOTE]
> Le **Using** élément dans le langage CSDL ne fonctionne pas exactement comme un **à l’aide de** instruction dans un langage de programmation. En important un espace de noms avec un **à l’aide de** instruction dans un langage de programmation, vous n’affectez pas les objets dans l’espace de noms d’origine. Dans le langage CSDL, un espace de noms importé peut contenir un type d'entité dérivé d'un type d'entité figurant dans l'espace de noms d'origine. Cela peut affecter les jeux d'entités déclarés dans l'espace de noms d'origine.

 

Le **Using** élément peut avoir les éléments enfants suivants :

-   Documentation (zéro ou un élément autorisé)
-   Éléments d’annotation (zéro ou plusieurs éléments autorisés)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **Using** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espace de noms**  | Oui         | Nom de l'espace de noms importé.                                                                                                                                                |
| **Alias**      | Oui         | Identificateur utilisé à la place du nom de l'espace de noms. Bien que cet attribut soit obligatoire, il n'est pas nécessaire qu'il soit utilisé à la place du nom de l'espace de noms pour qualifier les noms d'objets. |

 

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Using** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

 

### <a name="example"></a>Exemple

L’exemple suivant montre le **Using** élément utilisé pour importer un espace de noms qui est défini ailleurs. Notez que l’espace de noms pour le **schéma** élément indiqué est `BooksModel`. Le `Address` propriété sur le `Publisher` **EntityType** est un type complexe qui est défini dans le `ExtendedBooksModel` espace de noms (importé avec le **Using** élément).

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>Attributs d'annotation (CSDL)

Les attributs d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des attributs XML personnalisés dans le modèle conceptuel. En plus d'avoir une structure XML valide, les attributs d'annotation doivent satisfaire les critères suivants :

-   Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.
-   Plusieurs attributs d'annotation peuvent être appliqués à un élément CSDL donné.
-   Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.

Les attributs d'annotation peuvent être utilisés pour fournir des métadonnées supplémentaires sur des éléments dans un modèle conceptuel. Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec un attribut d’annotation (**CustomAttribute**). L'exemple fait également apparaître un élément d'annotation appliqué à l'élément de type d'entité.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Le code suivant récupère les métadonnées dans l'attribut d'annotation et les écrit dans la console :

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Le code ci-dessus suppose que le fichier `School.csdl` se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Éléments d'annotation (CSDL)

Les éléments d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des éléments XML personnalisés dans le modèle conceptuel. En plus d'avoir une structure XML valide, les éléments d'annotation doivent satisfaire les critères suivants :

-   Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.
-   Plusieurs éléments d'annotation peuvent être des enfants d'un élément CSDL donné.
-   Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.
-   Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément CSDL donné.

Les éléments d'annotation permettent de fournir des métadonnées supplémentaires sur les éléments dans un modèle conceptuel. À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec un élément d’annotation (**CustomElement**). L'exemple fait également apparaître un attribut d'annotation appliqué à l'élément de type d'entité.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Le code suivant récupère les métadonnées dans l'élément d'annotation et les écrit dans la console :

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Le code ci-dessus suppose que le fichier School.csdl se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Types de modèles conceptuels (CSDL)

Langage de définition de schéma conceptuel (CSDL) prend en charge un ensemble de types de données primitif abstrait, appelé **types Edmsimpletype**, qui définissent des propriétés dans un modèle conceptuel. **Types Edmsimpletype** sont des proxys pour les types de données primitifs sont prises en charge dans le stockage ou d’un environnement d’hébergement.

Le tableau suivant répertorie les types de données primitifs pris en charge par CSDL. Le tableau répertorie également les facettes peuvent être appliquées à chaque **EDMSimpleType**.

| EDMSimpleType                    | Description                                                | Facettes applicables                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | Contient des données binaires.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Edm.Boolean**                  | Contient la valeur **true** ou **false**.                  | Nullable, Default                                                        |
| **Edm.Byte**                     | Contient une valeur d'entier 8 bits non signé.                  | Precision, Nullable, Default                                             |
| **Edm.DateTime**                 | Représente une date et une heure.                                | Precision, Nullable, Default                                             |
| **Edm.DateTimeOffset**           | Contient une date et une heure en tant que décalage en minutes par rapport à l'heure GMT. | Precision, Nullable, Default                                             |
| **Edm.Decimal**                  | Contient une valeur numérique avec une précision et une échelle fixes.   | Precision, Nullable, Default                                             |
| **Edm.Double**                   | Contient un nombre avec une précision de 15 chiffres à virgule flottante   | Precision, Nullable, Default                                             |
| **Edm.Float**                    | Contient un nombre à virgule flottante avec une précision de 7 chiffres.   | Precision, Nullable, Default                                             |
| **Edm.Guid**                     | Contient un identificateur unique de 16 octets.                      | Precision, Nullable, Default                                             |
| **Edm.Int16**                    | Contient une valeur d'entier 16 bits signé.                    | Precision, Nullable, Default                                             |
| **Edm.Int32**                    | Contient une valeur d'entier 32 bits signé.                    | Precision, Nullable, Default                                             |
| **Edm.Int64**                    | Contient une valeur d'entier 64 bits signé.                    | Precision, Nullable, Default                                             |
| **Edm.SByte**                    | Contient une valeur d'entier 8 bits signé.                     | Precision, Nullable, Default                                             |
| **Edm.String**                   | Contient des données caractères.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **Edm.Time**                     | Contient une heure.                                    | Precision, Nullable, Default                                             |
| **Edm.Geography**                |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.Geometry**                 |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Nullable, par défaut, le SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Nullable, par défaut, le SRID                                                  |

## <a name="facets-csdl"></a>Facettes (CSDL)

Les facettes dans le langage CSDL (Conceptual Schema Definition Language) représentent des contraintes sur les propriétés de types d'entités et de types complexes. Les facettes apparaissent comme des attributs XML sur les éléments CSDL suivants :

-   Propriété
-   TypeRef
-   Paramètre

Le tableau ci-dessous décrit les facettes prises en charge dans le langage CSDL. Toutes les facettes sont facultatives. Certaines facettes répertoriées ci-dessous sont utilisées par Entity Framework lors de la génération d’une base de données à partir d’un modèle conceptuel.

> [!NOTE]
> Pour plus d’informations sur les types de données dans un modèle conceptuel, consultez Types de modèle conceptuel (CSDL).

| Facette               | Description                                                                                                                                                                                                                                                   | S'applique à                                                                                                                                                                                                                                                                                                                                                                           | Utilisée pour la génération de base de données. | Utilisée par le runtime. |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **classement**       | Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Oui                              | Non                  |
| **ConcurrencyMode** | Indique que la valeur de la propriété doit être utilisée pour des contrôles d'accès concurrentiel optimiste.                                                                                                                                                                    | Tous les **EDMSimpleType** propriétés                                                                                                                                                                                                                                                                                                                                                     | Non                               | Oui                 |
| **Default**         | Spécifie la valeur par défaut de la propriété si aucune valeur n'est fournie en cas d'instanciation.                                                                                                                                                                       | Tous les **EDMSimpleType** propriétés                                                                                                                                                                                                                                                                                                                                                     | Oui                              | Oui                 |
| **FixedLength**     | Spécifie si la longueur de la valeur de propriété peut varier.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Oui                              | Non                  |
| **MaxLength**       | Spécifie la longueur maximale de la valeur de propriété.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Oui                              | Non                  |
| **Nullable**        | Spécifie si la propriété peut avoir un **null** valeur.                                                                                                                                                                                                     | Tous les **EDMSimpleType** propriétés                                                                                                                                                                                                                                                                                                                                                     | Oui                              | Oui                 |
| **Précision**       | Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir. Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de propriété. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Oui                              | Non                  |
| **Échelle**           | Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de propriété.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Oui                              | Non                  |
| **SRID**            | Spécifie l’ID système Spatial référence système. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Non                               | Oui                 |
| **Unicode**         | Indique si la valeur de propriété est stockée au format Unicode.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Oui                              | Oui                 |

>[!NOTE]
> Lors de la génération d’une base de données à partir d’un modèle conceptuel, l’Assistant génération de base de données reconnaîtra la valeur de la **StoreGeneratedPattern** attribut sur un **propriété** élément si elle est la suivante espace de noms : http://schemas.microsoft.com/ado/2009/02/edm/annotation. Les valeurs prises en charge pour l’attribut sont **identité** et **calculé**. La valeur **identité** produira une colonne de base de données avec une valeur d’identité générée dans la base de données. La valeur **calculé** produira une colonne avec une valeur qui est calculée dans la base de données.

### <a name="example"></a>Exemple

L'exemple suivant illustre l'application de facettes aux propriétés d'un type d'entité :

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
