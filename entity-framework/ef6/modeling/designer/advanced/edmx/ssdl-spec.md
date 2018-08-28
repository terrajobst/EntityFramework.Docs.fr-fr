---
title: Spécification SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995277"
---
# <a name="ssdl-specification"></a>Spécification SSDL
SSDL (Store Schema Definition Language) est un langage basé sur XML qui décrit le modèle de stockage d'une application Entity Framework.

Dans une application Entity Framework, les métadonnées de modèle de stockage est chargée à partir d’un fichier .ssdl (écrit en SSDL) dans une instance de la System.Data.Metadata.Edm.StoreItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework utilise des métadonnées de modèle de stockage pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques au magasin.

Entity Framework Designer (Concepteur d’EF) stocke des informations de modèle de stockage dans un fichier .edmx au moment du design. Au moment de la génération du Concepteur d’entités utilise les informations dans un fichier .edmx pour créer le fichier .ssdl qui est nécessaire par Entity Framework lors de l’exécution.

Les versions de SSDL sont différenciées par les espaces de noms XML.

| Version du langage SSDL | XML Namespace                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association, élément (SSDL)

Un **Association** élément de langage de définition de schéma de magasin (SSDL) spécifie les colonnes de table qui participent à dans une contrainte de clé étrangère dans la base de données sous-jacente. Deux éléments de fin enfant obligatoire spécifient des tables situées aux terminaisons de l’association et la multiplicité à chaque extrémité. Un élément ReferentialConstraint facultatif spécifie les terminaisons principales et dépendantes de l’association, ainsi que les colonnes participantes. Si aucun **ReferentialConstraint** élément est présent, un élément AssociationSetMapping doit être utilisé pour spécifier les mappages de colonnes pour l’association.

Le **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Fin (exactement deux éléments)
-   ReferentialConstraint (zéro ou 1)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Association** élément.

| Nom d'attribut | Requis | Value                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom de la contrainte de clé étrangère correspondante dans la base de données sous-jacente. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a>AssociationSet, élément (SSDL)

Le **AssociationSet** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère entre deux tables dans la base de données sous-jacente. Les colonnes de table qui participent à la contrainte de clé étrangère sont spécifiées dans un élément de l’Association. Le **Association** élément qui correspond à une donnée **AssociationSet** élément est spécifié dans le **Association** attribut de la **AssociationSet**  élément.

Ensembles d’associations SSDL sont mappés aux ensembles d’associations CSDL à un élément AssociationSetMapping. Toutefois, si l’ensemble d’associations CSDL pour une association CSDL donnée est défini à l’aide d’un élément ReferentialConstraint, aucune correspondant **AssociationSetMapping** élément n’est nécessaire. Dans ce cas, si un **AssociationSetMapping** élément est présent, les mappages, il définit seront remplacées par les **ReferentialConstraint** élément.

Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Fin (zéro, un ou deux)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.

| Nom d'attribut  | Requis | Value                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Name**        | Oui         | Nom de la contrainte de clé étrangère que l'ensemble d'associations représente.                          |
| **Association** | Oui         | Nom de l'association qui définit les colonnes qui participent à la contrainte de clé étrangère. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **AssociationSet** élément qui représente le `FK_CustomerOrders` une contrainte de clé étrangère dans la base de données sous-jacente :

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType, élément (SSDL)

Le **CollectionType** élément de langage de définition de schéma de magasin (SSDL) Spécifie que le type de retour d’une fonction est une collection. Le **CollectionType** élément est un enfant de l’élément ReturnType. Le type de collection est spécifié à l’aide de l’élément enfant de RowType :

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre une fonction qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a>CommandText, élément (SSDL)

Le **CommandText** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui vous permet de définir une instruction SQL qui est exécutée sur la base de données. Le **CommandText** élément vous permet d’ajouter des fonctionnalités sont similaire à une procédure stockée dans la base de données, mais que vous définissez le **CommandText** élément dans le modèle de stockage.

Le **CommandText** élément ne peut pas avoir d’éléments enfants. Le corps de la **CommandText** élément doit être une instruction SQL valide pour la base de données sous-jacente.

Aucun attribut n’est applicable à la **CommandText** élément.

### <a name="example"></a>Exemple

L’exemple suivant montre un **fonction** élément avec un enfant **CommandText** élément. Exposer le **UpdateProductInOrder** fonctionner en tant que méthode sur ObjectContext en l’important dans le modèle conceptuel.  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a>DefiningQuery, élément (SSDL)

Le **DefiningQuery** élément dans le langage de définition de schéma de magasin (SSDL) vous permet d’exécuter une instruction SQL directement dans la base de données sous-jacente. Le **DefiningQuery** élément est couramment utilisé comme une vue de base de données, mais la vue est définie dans le modèle de stockage au lieu de la base de données. La vue définie dans un **DefiningQuery** élément peut être mappé à un type d’entité dans le modèle conceptuel via un élément EntitySetMapping. Ces mappages sont en lecture seule.  

La syntaxe SSDL suivante illustre la déclaration d’un **EntitySet** suivie de la **DefiningQuery** élément qui contient une requête utilisée pour récupérer la vue.

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

Vous pouvez utiliser des procédures stockées dans Entity Framework pour activer des scénarios en lecture-écriture sur des vues. Vous pouvez utiliser une vue de source de données ou une vue Entity SQL en tant que la table de base pour la récupération des données et traitement des modifications par des procédures stockées.

Vous pouvez utiliser la **DefiningQuery** élément à cibler Microsoft SQL Server Compact 3.5. Bien que SQL Server Compact 3.5 ne prend pas en charge les procédures stockées, vous pouvez implémenter des fonctionnalités similaires avec le **DefiningQuery** élément. Cet élément peut s'avérer également utile pour créer des procédures stockées afin de surmonter une incompatibilité entre les types de données utilisés dans le langage de programmation et ceux de la source de données. Vous pouvez écrire un **DefiningQuery** qui prend un certain ensemble de paramètres et appelle ensuite une procédure stockée avec un autre ensemble de paramètres, par exemple, une procédure stockée qui supprime des données.

## <a name="dependent-element-ssdl"></a>Élément Dependent (SSDL)

Le **dépendants** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison dépendante d’une contrainte de clé étrangère (également appelée une contrainte référentielle). Le **dépendants** élément spécifie la (ou les colonnes) dans une table qui font référence à une colonne clé primaire (ou les colonnes). **PropertyRef** éléments spécifient quelles colonnes sont référencées. Le Principal élément spécifie les colonnes de clés primaires référencées par les colonnes qui sont spécifiés dans le **dépendants** élément.

Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   PropertyRef (un ou plusieurs)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **dépendants** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rôle**       | Oui         | La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne de référencement. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte. Le **dépendants** élément spécifie la **CustomerId** colonne de la **ordre** table en tant que la terminaison dépendante de la contrainte.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a>Documentation, élément (SSDL)

Le **Documentation** élément dans le langage de définition de schéma de magasin (SSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent.

Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   **Résumé**: une brève description de l’élément parent. (zéro ou un élément).
-   **LongDescription**: une description détaillée de l’élément parent. (zéro ou un élément).

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a>End, élément (SSDL)

Le **fin** élément de langage de définition de schéma de magasin (SSDL) spécifie la table et le nombre de lignes à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente. Le **fin** élément peut être un enfant de l’élément Association ou de l’élément AssociationSet. Dans chaque cas, les éléments enfants et les attributs applicables possibles sont différents.

### <a name="end-element-as-a-child-of-the-association-element"></a>Élément End comme enfant de l'élément Association

Un **fin** élément (en tant qu’enfant de le **Association** élément) spécifie la table et le nombre de lignes à la fin d’une contrainte de clé étrangère avec la **Type** et **Multiplicité** respectivement. Les terminaisons d'une contrainte de clé étrangère sont définies dans le cadre d'un ensemble d'associations SSDL ; un ensemble d'associations SSDL doit avoir exactement deux terminaisons.

Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   OnDelete (zéro ou un élément)
-   Éléments d’annotation (zéro ou plusieurs éléments)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.

| Nom d'attribut   | Requis | Value                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Oui         | Le nom qualifié complet du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.                                                                                                                                                                                                                                                                                          |
| **Rôle**         | Non          | La valeur de la **rôle** attribut dans un élément Principal ou du dépendant de l’élément ReferentialConstraint correspondant (si utilisé).                                                                                                                                                                                                                                             |
| **Multiplicité** | Oui         | **1**, **valeur 0.. 1**, ou **\*** selon le nombre de lignes qui peuvent être à la fin de la contrainte de clé étrangère. <br/> **1** indique qu’exactement une ligne existe à la fin de la contrainte de clé étrangère. <br/> **valeur 0.. 1** indique que zéro ou une ligne existe à la fin de la contrainte de clé étrangère. <br/> **\*** Indique que zéro, un ou plusieurs lignes existent à la fin de la contrainte de clé étrangère. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

#### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère. Le **multiplicité** valeurs spécifiées sur chaque **fin** élément indiquent que plusieurs lignes dans le **commandes** table peut être associée à une ligne dans le **clients**  table, mais qu’une seule ligne dans le **clients** table peut être associée à une ligne dans le **commandes** table. En outre, le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **clients** table est supprimée.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Élément End comme enfant de l'élément AssociationSet

Le **fin** élément (en tant qu’enfant de le **AssociationSet** élément) spécifie une table à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente.

Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plus)

#### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.

| Nom d'attribut | Requis | Value                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Oui         | Le nom du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.                                      |
| **Rôle**       | Non          | La valeur de l’un de le **rôle** spécifiés pour l’un attributs **fin** élément de l’élément Association correspondant. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

#### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément avec un **AssociationSet** élément avec deux **fin** éléments :

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a>EntityContainer, élément (SSDL)

Un **EntityContainer** élément dans le langage de définition de schéma de magasin (SSDL) décrit la structure de la source de données sous-jacente dans une application Entity Framework : jeux d’entités SSDL (définis dans les éléments EntitySet) représente des tables dans une base de données, les types d’entité SSDL (définis dans les éléments EntityType) représentent des lignes dans une table, et ensembles d’associations (définis dans les éléments de l’AssociationSet) représentent des contraintes de clé étrangère dans une base de données. Un conteneur d’entités de stockage modèle mappe à un conteneur d’entités de modèle conceptuel via l’élément EntityContainerMapping.

Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation. Si un **Documentation** élément est présent, il doit précéder tous les autres éléments enfants.

Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :

-   EntitySet ;
-   AssociationSet ;
-   éléments d'annotation.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityContainer** élément.

| Nom d'attribut | Requis | Value                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du conteneur d'entités. Ce nom ne peut pas contenir de point (.). |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément qui définit deux jeux d’entités et un ensemble d’associations. Notez que les noms de type d'entité et de type d'association sont qualifiés par le nom de l'espace de noms du modèle conceptuel.

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a>EntitySet, élément (SSDL)

Un **EntitySet** élément de langage de définition de schéma de magasin (SSDL) représente une table ou vue dans la base de données sous-jacente. Un élément EntityType en langage SSDL représente une ligne dans la table ou vue. Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL. Le mappage entre un jeu d’entités CSDL et un jeu d’entités SSDL est spécifié dans un élément EntitySetMapping.

Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   DefiningQuery (zéro ou un élément)
-   éléments d'annotation.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.

> [!NOTE]
> Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias. Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.

| Nom d'attribut | Requis | Value                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du jeu d'entités.                                                              |
| **EntityType** | Oui         | Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances. |
| **schéma**     | Non          | Schéma de base de données.                                                                     |
| **Table**      | Non          | Table de base de données.                                                                      |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityContainer** élément qui a deux **EntitySet** éléments et l’autre **AssociationSet** élément :

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a>EntityType, élément (SSDL)

Un **EntityType** élément de langage de définition de schéma de magasin (SSDL) représente une ligne dans une table ou vue de la base de données sous-jacente. Un élément EntitySet en SSDL représente la table ou vue dans laquelle les lignes résultent. Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL. Le mappage entre un type d’entité SSDL et un type d’entité CSDL est spécifié dans un élément EntityTypeMapping.

Le **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Clé (zéro ou un élément)
-   éléments d'annotation.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du type d'entité. Cette valeur est habituellement la même que le nom de la table dans laquelle le type d'entité représente une ligne. Cette valeur ne peut pas contenir de point (.). |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec deux propriétés :

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a>Function, élément (SSDL)

Le **fonction** élément de langage de définition de schéma de magasin (SSDL) spécifie une procédure stockée qui existe dans la base de données sous-jacente.

Le **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Paramètre (zéro ou plus)
-   CommandText (zéro ou 1)
-   ReturnType (zéro ou plus)
-   Éléments d’annotation (zéro ou plus)

Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.

Les procédures stockées spécifiées dans le modèle de stockage peuvent être importées dans le modèle conceptuel d'une application. Pour plus d’informations, consultez [interrogation avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/query.md). Le **fonction** élément peut également être utilisé pour définir des fonctions personnalisées dans le modèle de stockage.  

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fonction** élément.

> [!NOTE]
> Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias. Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.

| Nom d'attribut             | Requis | Value                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                   | Oui         | Nom de la procédure stockée.                                                                                                                                                                                  |
| **ReturnType**             | Non          | Type de retour de la procédure stockée.                                                                                                                                                                           |
| **Aggregate**              | Non          | **True** si la procédure stockée retourne une valeur d’agrégation ; sinon **False**.                                                                                                                                  |
| **BuiltIn**                | Non          | **True** si la fonction est intégré<sup>1</sup> fonction ; sinon **False**.                                                                                                                                  |
| **StoreFunctionName**      | Non          | Nom de la procédure stockée.                                                                                                                                                                                  |
| **NiladicFunction**        | Non          | **True** si la fonction est un niladiques<sup>2</sup> fonction ; **False** dans le cas contraire.                                                                                                                                   |
| **IsComposable**           | Non          | **True** si la fonction est un élément composable<sup>3</sup> fonction ; **False** dans le cas contraire.                                                                                                                                |
| **ParameterTypeSemantics** | Non          | Énumération qui définit la sémantique de type utilisée pour résoudre les surcharges de fonction. L'énumération est définie dans le manifeste du fournisseur par définition de fonction. La valeur par défaut est **AllowImplicitConversion**. |
| **schéma**                 | Non          | Nom du schéma dans lequel une procédure stockée est définie.                                                                                                                                                   |

<sup>1</sup> une fonction intégrée est une fonction qui est définie dans la base de données. Pour plus d’informations sur les fonctions qui sont définis dans le modèle de stockage, consultez l’élément CommandText (SSDL).

<sup>2</sup> une fonction niladique est une fonction qui n’accepte aucun paramètre et, lorsqu’elle est appelée, ne nécessite pas de parenthèses.

<sup>3</sup> deux fonctions sont composables si la sortie d’une fonction peut être l’entrée de l’autre fonction.

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **fonction** élément qui correspond à la **UpdateOrderQuantity** procédure stockée. La procédure stockée accepte deux paramètres et ne retourne pas de valeur.

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a>Key, élément (SSDL)

Le **clé** élément de langage de définition de schéma de magasin (SSDL) représente la clé primaire d’une table dans la base de données sous-jacente. **Clé** est un élément enfant d’un élément EntityType, qui représente une ligne dans une table. La clé primaire est définie dans le **clé** élément en faisant référence à un ou plusieurs éléments de propriété sont définis sur le **EntityType** élément.

Le **clé** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   PropertyRef (un ou plusieurs)
-   éléments d'annotation.

Aucun attribut n’est applicable à la **clé** élément.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec une clé qui fait référence à une seule propriété :

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a>OnDelete, élément (SSDL)

Le **OnDelete** élément dans le langage de définition de schéma de magasin (SSDL) reflète le comportement de la base de données lorsqu’une ligne qui participe à une contrainte de clé étrangère est supprimée. Si l’action est définie sur **Cascade**, puis les lignes qui font référence à une ligne qui est en cours de suppression seront également supprimées. Si l’action est définie sur **aucun**, les lignes qui font référence à une ligne qui est en cours de suppression ne sont pas également supprimées. Un **OnDelete** élément est un élément enfant d’un élément de fin.

Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.

| Nom d'attribut | Requis | Value                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Action**     | Oui         | **Cascade** ou **aucun**. (La valeur **Restricted** est valide mais a le même comportement que **aucun**.) |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **OnDelete** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère. Le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **Clients** table est supprimée.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a>Élément Parameter (SSDL)

Le **paramètre** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui spécifie les paramètres pour une procédure stockée dans la base de données.

Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   Documentation (zéro ou un élément)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **paramètre** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Oui         | Nom du paramètre.                                                                                                                                                                                                      |
| **Type**       | Oui         | Type du paramètre.                                                                                                                                                                                                             |
| **Mode**       | Non          | **Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.                                                                                                                |
| **MaxLength**  | Non          | Longueur maximale du paramètre.                                                                                                                                                                                            |
| **Précision**  | Non          | Précision du paramètre.                                                                                                                                                                                                 |
| **Échelle**      | Non          | Échelle du paramètre.                                                                                                                                                                                                     |
| **SRID**       | Non          | Identificateur de référence spatiale système. Valide uniquement pour les paramètres des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **fonction** élément qui a deux **paramètre** éléments qui spécifient les paramètres d’entrée :

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a>Élément Principal (SSDL)

Le **Principal** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte de clé étrangère (également appelée une contrainte référentielle). Le **Principal** élément spécifie la colonne de clé primaire (ou les colonnes) dans une table qui est référencée par un autre (ou les colonnes). **PropertyRef** éléments spécifient quelles colonnes sont référencées. L’élément dépendant spécifie les colonnes qui font référence les colonnes de clés primaires sont spécifiés dans le **Principal** élément.

Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :

-   PropertyRef (un ou plusieurs)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Principal** élément.

| Nom d'attribut | Requis | Value                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rôle**       | Oui         | La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne référencée. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte. Le **Principal** élément spécifie la **CustomerId** colonne de la **client** table en tant que la terminaison principale de la contrainte.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a>Property, élément (SSDL)

Le **propriété** élément de langage de définition de schéma de magasin (SSDL) représente une colonne dans une table dans la base de données sous-jacente. **Propriété** éléments sont des enfants d’éléments EntityType, qui représentent des lignes dans une table. Chaque **propriété** élément défini sur une **EntityType** élément représente une colonne.

Un **propriété** élément ne peut pas avoir d’éléments enfants.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                  | Oui         | Nom de la colonne correspondante.                                                                                                                                                                                           |
| **Type**                  | Oui         | Type de la colonne correspondante.                                                                                                                                                                                           |
| **Nullable**              | Non          | **True** (la valeur par défaut) ou **False** selon si la colonne correspondante peut avoir une valeur null.                                                                                                                  |
| **DefaultValue**          | Non          | Valeur par défaut de la colonne correspondante.                                                                                                                                                                                  |
| **MaxLength**             | Non          | Longueur maximale de la colonne correspondante.                                                                                                                                                                                 |
| **FixedLength**           | Non          | **True** ou **False** selon si la valeur de colonne correspondante sera être stockée en tant que chaîne de longueur fixe.                                                                                                              |
| **Précision**             | Non          | Précision de la colonne correspondante.                                                                                                                                                                                      |
| **Échelle**                 | Non          | Échelle de la colonne correspondante.                                                                                                                                                                                          |
| **Unicode**               | Non          | **True** ou **False** selon si la valeur de colonne correspondante sera être stockée sous forme de chaîne Unicode.                                                                                                                   |
| **classement**             | Non          | Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.                                                                                                                                                   |
| **SRID**                  | Non          | Identificateur de référence spatiale système. Valide uniquement pour les propriétés des types spatiaux. Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Non          | **Aucun**, **identité** (si la valeur de colonne correspondante est une identité qui est générée dans la base de données), ou **calculé** (si la valeur de colonne correspondante est calculée dans la base de données). Non valide pour les propriétés de RowType. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **EntityType** élément avec deux enfants **propriété** éléments :

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a>Élément PropertyRef (SSDL)

Le **PropertyRef** élément dans le langage de définition de schéma de magasin (SSDL) fait référence à une propriété définie sur un élément EntityType pour indiquer que la propriété effectuera l’un des rôles suivants :

-   Faire partie de la clé primaire de la table qui la **EntityType** représente. Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé primaire. Pour plus d’informations, consultez l’élément Key.
-   Faire partie de la terminaison dépendante ou principale d'une contrainte référentielle. Pour plus d’informations, consultez ReferentialConstraint (élément).

Le **PropertyRef** élément peut avoir uniquement les éléments enfants suivants :

-   Documentation (zéro ou un élément)
-   éléments d'annotation.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.

| Nom d'attribut | Requis | Value                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Oui         | Nom de la propriété référencée. |

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **PropertyRef** élément utilisé pour définir une clé primaire en référençant une propriété qui est définie sur une **EntityType** élément.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint, élément (SSDL)

Le **ReferentialConstraint** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère (également appelée une contrainte d’intégrité référentielle) dans la base de données sous-jacente. Les terminaisons principales et dépendantes de la contrainte sont spécifiées par les éléments enfants principale et dépendante, respectivement. Les colonnes qui participent les terminaisons principale et dépendantes sont référencées avec les éléments PropertyRef.

Le **ReferentialConstraint** élément est un élément enfant facultatif de l’élément Association. Si un **ReferentialConstraint** élément n’est pas utilisé pour mapper la contrainte de clé étrangère est spécifiée dans le **Association** élément AssociationSetMapping élément doit être utilisé pour ce faire.

Le **ReferentialConstraint** élément peut avoir les éléments enfants suivants :

-   Documentation (zéro ou un élément)
-   Principal (exactement un élément)
-   Dépendante (exactement un élément)
-   Éléments d’annotation (zéro ou plus)

### <a name="applicable-attributes"></a>Attributs applicables

N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferentialConstraint** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a>ReturnType, élément (SSDL)

Le **ReturnType** élément dans le langage de définition de schéma de magasin (SSDL) spécifie le type de retour pour une fonction qui est défini dans un **fonction** élément. Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.

Le type de retour d’une fonction est spécifié avec la **Type** attribut ou le **ReturnType** élément.

Le **ReturnType** élément peut avoir les éléments enfants suivants :

- CollectionType (un)  

> [!NOTE]
> N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément. Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL. Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.

### <a name="example"></a>Exemple

L’exemple suivant utilise un **fonction** qui retourne une collection de lignes.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a>RowType, élément (SSDL)

Un **RowType** élément dans le langage de définition de schéma de magasin (SSDL) définit une structure sans nom comme un retour de type pour une fonction définie dans le magasin.

Un **RowType** élément est l’élément enfant de **CollectionType** élément :

Un **RowType** élément peut avoir les éléments enfants suivants :

- Propriété (un ou plusieurs)  

### <a name="example"></a>Exemple

L’exemple suivant montre une fonction de magasin qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a>Schema, élément (SSDL)

Le **schéma** élément dans le langage de définition de schéma de magasin (SSDL) est l’élément racine d’une définition de modèle de stockage. Il contient des définitions pour les objets, les fonctions et les conteneurs qui composent un modèle de stockage.

Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :

-   Association
-   EntityType
-   EntityContainer
-   Fonction

Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour les objets de type et l’association d’entité dans un modèle de stockage. Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.

Un espace de noms de modèle de stockage est différent de l’espace de noms XML de la **schéma** élément. Un espace de noms de modèle de stockage (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités et types d’associations. L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément. Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage SSDL. Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.

### <a name="applicable-attributes"></a>Attributs applicables

Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.

| Nom d'attribut            | Requis | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espace de noms**             | Oui         | Espace de noms du modèle de stockage. La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type. Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms ExampleModel.Store, le nom qualifié complet de le **EntityType** est ExampleModel.Store.Customer. <br/> Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**. La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma CSDL. |
| **Alias**                 | Non          | Identificateur utilisé à la place du nom de l'espace de noms. Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms ExampleModel.Store et la valeur de la **Alias** attribut est *StorageModel*, vous pouvez utiliser Storagemodel.client comme nom qualifié complet de le **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Fournisseur**              | Oui         | Fournisseur de données.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Oui         | Jeton qui indique au fournisseur quel manifeste de fournisseur retourner. Aucun format n'est défini pour le jeton. Les valeurs du jeton sont définies par le fournisseur. Pour plus d’informations sur les jetons du manifeste du fournisseur SQL Server, consultez SqlClient pour Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Exemple

L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a>Attributs d'annotation

Les attributs d'annotation en SSDL (Store Schema Definition Language) sont des attributs XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant les éléments dans le modèle de stockage. En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux attributs d'annotation :

-   Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage SSDL.
-   Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.

Plusieurs attributs d'annotation peuvent être appliqués à un élément SSDL donné. Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.

### <a name="example"></a>Exemple

L’exemple suivant montre un élément EntityType qui a un attribut d’annotation appliqué à la **OrderId** propriété. L’exemple montre également un élément d’annotation ajouté à la **EntityType** élément.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a>Éléments d'annotation (SSDL)

Les éléments d'annotation en SSDL (Store Schema Definition Language) sont des éléments XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant le modèle de stockage. En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux éléments d'annotation :

-   Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé au langage SSDL.
-   Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.
-   Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément SSDL donné.

Plusieurs éléments d'annotation peuvent être des enfants d'un élément SSDL donné. À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.

### <a name="example"></a>Exemple

L’exemple suivant montre un élément EntityType qui a un élément d’annotation (**CustomElement**). L’exemple montre également un attribut d’annotation appliqué à la **OrderId** propriété.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a>Facettes (SSDL)

Les facettes dans le langage de définition de schéma de magasin (SSDL) représentent des contraintes sur les types de colonnes qui sont spécifiés dans les éléments de propriété. Les facettes sont implémentées comme des attributs XML sur **propriété** éléments.

Le tableau ci-dessous décrit les facettes prises en charge dans le langage SSDL :

| Facette           | Description                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **classement**   | Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.                                                                                                             |
| **FixedLength** | Spécifie si la longueur de la valeur de colonne peut varier.                                                                                                                                                                                                  |
| **MaxLength**   | Spécifie la longueur maximale de la valeur de colonne.                                                                                                                                                                                                           |
| **Précision**   | Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir. Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de colonne. |
| **Échelle**       | Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de colonne.                                                                                                                                                                      |
| **Unicode**     | Indique si la valeur de colonne est stockée au format Unicode.                                                                                                                                                                                                    |
