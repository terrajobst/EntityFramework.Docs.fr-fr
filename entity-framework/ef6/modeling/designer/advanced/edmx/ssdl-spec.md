---
title: Spécification SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121451"
---
# <a name="ssdl-specification"></a><span data-ttu-id="e9e51-102">Spécification SSDL</span><span class="sxs-lookup"><span data-stu-id="e9e51-102">SSDL Specification</span></span>
<span data-ttu-id="e9e51-103">SSDL (Store Schema Definition Language) est un langage basé sur XML qui décrit le modèle de stockage d'une application Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9e51-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="e9e51-104">Dans une application Entity Framework, les métadonnées de modèle de stockage est chargée à partir d’un fichier .ssdl (écrit en SSDL) dans une instance de la System.Data.Metadata.Edm.StoreItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="e9e51-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="e9e51-105">Entity Framework utilise des métadonnées de modèle de stockage pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques au magasin.</span><span class="sxs-lookup"><span data-stu-id="e9e51-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="e9e51-106">Entity Framework Designer (Concepteur d’EF) stocke des informations de modèle de stockage dans un fichier .edmx au moment du design.</span><span class="sxs-lookup"><span data-stu-id="e9e51-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="e9e51-107">Au moment de la génération du Concepteur d’entités utilise les informations dans un fichier .edmx pour créer le fichier .ssdl qui est nécessaire par Entity Framework lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e9e51-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="e9e51-108">Les versions de SSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="e9e51-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="e9e51-109">Version du langage SSDL</span><span class="sxs-lookup"><span data-stu-id="e9e51-109">SSDL Version</span></span> | <span data-ttu-id="e9e51-110">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="e9e51-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="e9e51-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="e9e51-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="e9e51-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="e9e51-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="e9e51-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="e9e51-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="e9e51-114">Association, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-114">Association Element (SSDL)</span></span>

<span data-ttu-id="e9e51-115">Un **Association** élément de langage de définition de schéma de magasin (SSDL) spécifie les colonnes de table qui participent à dans une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="e9e51-116">Deux éléments de fin enfant obligatoire spécifient des tables situées aux terminaisons de l’association et la multiplicité à chaque extrémité.</span><span class="sxs-lookup"><span data-stu-id="e9e51-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="e9e51-117">Un élément ReferentialConstraint facultatif spécifie les terminaisons principales et dépendantes de l’association, ainsi que les colonnes participantes.</span><span class="sxs-lookup"><span data-stu-id="e9e51-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="e9e51-118">Si aucun **ReferentialConstraint** élément est présent, un élément AssociationSetMapping doit être utilisé pour spécifier les mappages de colonnes pour l’association.</span><span class="sxs-lookup"><span data-stu-id="e9e51-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="e9e51-119">Le **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-120">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-121">Fin (exactement deux éléments)</span><span class="sxs-lookup"><span data-stu-id="e9e51-121">End (exactly two)</span></span>
-   <span data-ttu-id="e9e51-122">ReferentialConstraint (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="e9e51-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="e9e51-123">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-124">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-124">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-125">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="e9e51-126">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-126">Attribute Name</span></span> | <span data-ttu-id="e9e51-127">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-127">Is Required</span></span> | <span data-ttu-id="e9e51-128">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-129">**Name**</span></span>       | <span data-ttu-id="e9e51-130">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-130">Yes</span></span>         | <span data-ttu-id="e9e51-131">Nom de la contrainte de clé étrangère correspondante dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-132">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="e9e51-133">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-134">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-135">Example</span></span>

<span data-ttu-id="e9e51-136">L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="e9e51-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="e9e51-137">AssociationSet, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="e9e51-138">Le **AssociationSet** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère entre deux tables dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="e9e51-139">Les colonnes de table qui participent à la contrainte de clé étrangère sont spécifiées dans un élément de l’Association.</span><span class="sxs-lookup"><span data-stu-id="e9e51-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="e9e51-140">Le **Association** élément qui correspond à une donnée **AssociationSet** élément est spécifié dans le **Association** attribut de la **AssociationSet**  élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="e9e51-141">Ensembles d’associations SSDL sont mappés aux ensembles d’associations CSDL à un élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e9e51-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="e9e51-142">Toutefois, si l’ensemble d’associations CSDL pour une association CSDL donnée est défini à l’aide d’un élément ReferentialConstraint, aucune correspondant **AssociationSetMapping** élément n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e9e51-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="e9e51-143">Dans ce cas, si un **AssociationSetMapping** élément est présent, les mappages, il définit seront remplacées par les **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="e9e51-144">Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-145">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-146">Fin (zéro, un ou deux)</span><span class="sxs-lookup"><span data-stu-id="e9e51-146">End (zero or two)</span></span>
-   <span data-ttu-id="e9e51-147">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-148">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-148">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-149">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="e9e51-150">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-150">Attribute Name</span></span>  | <span data-ttu-id="e9e51-151">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-151">Is Required</span></span> | <span data-ttu-id="e9e51-152">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-153">**Name**</span></span>        | <span data-ttu-id="e9e51-154">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-154">Yes</span></span>         | <span data-ttu-id="e9e51-155">Nom de la contrainte de clé étrangère que l'ensemble d'associations représente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="e9e51-156">**Association**</span><span class="sxs-lookup"><span data-stu-id="e9e51-156">**Association**</span></span> | <span data-ttu-id="e9e51-157">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-157">Yes</span></span>         | <span data-ttu-id="e9e51-158">Nom de l'association qui définit les colonnes qui participent à la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-159">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="e9e51-160">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-161">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-162">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-162">Example</span></span>

<span data-ttu-id="e9e51-163">L’exemple suivant montre un **AssociationSet** élément qui représente le `FK_CustomerOrders` une contrainte de clé étrangère dans la base de données sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="e9e51-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="e9e51-164">CollectionType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="e9e51-165">Le **CollectionType** élément de langage de définition de schéma de magasin (SSDL) Spécifie que le type de retour d’une fonction est une collection.</span><span class="sxs-lookup"><span data-stu-id="e9e51-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="e9e51-166">Le **CollectionType** élément est un enfant de l’élément ReturnType.</span><span class="sxs-lookup"><span data-stu-id="e9e51-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="e9e51-167">Le type de collection est spécifié à l’aide de l’élément enfant de RowType :</span><span class="sxs-lookup"><span data-stu-id="e9e51-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="e9e51-168">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="e9e51-169">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-170">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-171">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-171">Example</span></span>

<span data-ttu-id="e9e51-172">L’exemple suivant montre une fonction qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes.</span><span class="sxs-lookup"><span data-stu-id="e9e51-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="e9e51-173">CommandText, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="e9e51-174">Le **CommandText** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui vous permet de définir une instruction SQL qui est exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="e9e51-175">Le **CommandText** élément vous permet d’ajouter des fonctionnalités sont similaire à une procédure stockée dans la base de données, mais que vous définissez le **CommandText** élément dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="e9e51-176">Le **CommandText** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="e9e51-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="e9e51-177">Le corps de la **CommandText** élément doit être une instruction SQL valide pour la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="e9e51-178">Aucun attribut n’est applicable à la **CommandText** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-179">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-179">Example</span></span>

<span data-ttu-id="e9e51-180">L’exemple suivant montre un **fonction** élément avec un enfant **CommandText** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="e9e51-181">Exposer le **UpdateProductInOrder** fonctionner en tant que méthode sur ObjectContext en l’important dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="e9e51-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="e9e51-182">DefiningQuery, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="e9e51-183">Le **DefiningQuery** élément dans le langage de définition de schéma de magasin (SSDL) vous permet d’exécuter une instruction SQL directement dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="e9e51-184">Le **DefiningQuery** élément est couramment utilisé comme une vue de base de données, mais la vue est définie dans le modèle de stockage au lieu de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="e9e51-185">La vue définie dans un **DefiningQuery** élément peut être mappé à un type d’entité dans le modèle conceptuel via un élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="e9e51-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="e9e51-186">Ces mappages sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="e9e51-186">These mappings are read-only.</span></span>  

<span data-ttu-id="e9e51-187">La syntaxe SSDL suivante illustre la déclaration d’un **EntitySet** suivie de la **DefiningQuery** élément qui contient une requête utilisée pour récupérer la vue.</span><span class="sxs-lookup"><span data-stu-id="e9e51-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="e9e51-188">Vous pouvez utiliser des procédures stockées dans Entity Framework pour activer des scénarios en lecture-écriture sur des vues.</span><span class="sxs-lookup"><span data-stu-id="e9e51-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="e9e51-189">Vous pouvez utiliser une vue de source de données ou une vue Entity SQL en tant que la table de base pour la récupération des données et traitement des modifications par des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="e9e51-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="e9e51-190">Vous pouvez utiliser la **DefiningQuery** élément à cibler Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="e9e51-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="e9e51-191">Bien que SQL Server Compact 3.5 ne prend pas en charge les procédures stockées, vous pouvez implémenter des fonctionnalités similaires avec le **DefiningQuery** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="e9e51-192">Cet élément peut s'avérer également utile pour créer des procédures stockées afin de surmonter une incompatibilité entre les types de données utilisés dans le langage de programmation et ceux de la source de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="e9e51-193">Vous pouvez écrire un **DefiningQuery** qui prend un certain ensemble de paramètres et appelle ensuite une procédure stockée avec un autre ensemble de paramètres, par exemple, une procédure stockée qui supprime des données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="e9e51-194">Élément Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="e9e51-195">Le **dépendants** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison dépendante d’une contrainte de clé étrangère (également appelée une contrainte référentielle).</span><span class="sxs-lookup"><span data-stu-id="e9e51-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="e9e51-196">Le **dépendants** élément spécifie la (ou les colonnes) dans une table qui font référence à une colonne clé primaire (ou les colonnes).</span><span class="sxs-lookup"><span data-stu-id="e9e51-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="e9e51-197">**PropertyRef** éléments spécifient quelles colonnes sont référencées.</span><span class="sxs-lookup"><span data-stu-id="e9e51-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="e9e51-198">Le Principal élément spécifie les colonnes de clés primaires référencées par les colonnes qui sont spécifiés dans le **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="e9e51-199">Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-200">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="e9e51-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e9e51-201">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-202">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-202">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-203">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="e9e51-204">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-204">Attribute Name</span></span> | <span data-ttu-id="e9e51-205">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-205">Is Required</span></span> | <span data-ttu-id="e9e51-206">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-207">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-207">**Role**</span></span>       | <span data-ttu-id="e9e51-208">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-208">Yes</span></span>         | <span data-ttu-id="e9e51-209">La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne de référencement.</span><span class="sxs-lookup"><span data-stu-id="e9e51-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-210">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="e9e51-211">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-212">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-213">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-213">Example</span></span>

<span data-ttu-id="e9e51-214">L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte.</span><span class="sxs-lookup"><span data-stu-id="e9e51-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e9e51-215">Le **dépendants** élément spécifie la **CustomerId** colonne de la **ordre** table en tant que la terminaison dépendante de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="e9e51-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="e9e51-216">Documentation, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="e9e51-217">Le **Documentation** élément dans le langage de définition de schéma de magasin (SSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="e9e51-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="e9e51-218">Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-219">**Résumé**: une brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="e9e51-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="e9e51-220">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="e9e51-220">(zero or one element)</span></span>
-   <span data-ttu-id="e9e51-221">**LongDescription**: une description détaillée de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="e9e51-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="e9e51-222">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="e9e51-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-223">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-223">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-224">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="e9e51-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-227">Example</span></span>

<span data-ttu-id="e9e51-228">L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="e9e51-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="e9e51-229">End, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-229">End Element (SSDL)</span></span>

<span data-ttu-id="e9e51-230">Le **fin** élément de langage de définition de schéma de magasin (SSDL) spécifie la table et le nombre de lignes à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="e9e51-231">Le **fin** élément peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="e9e51-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="e9e51-232">Dans chaque cas, les éléments enfants et les attributs applicables possibles sont différents.</span><span class="sxs-lookup"><span data-stu-id="e9e51-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="e9e51-233">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="e9e51-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="e9e51-234">Un **fin** élément (en tant qu’enfant de le **Association** élément) spécifie la table et le nombre de lignes à la fin d’une contrainte de clé étrangère avec la **Type** et **Multiplicité** respectivement.</span><span class="sxs-lookup"><span data-stu-id="e9e51-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="e9e51-235">Les terminaisons d'une contrainte de clé étrangère sont définies dans le cadre d'un ensemble d'associations SSDL ; un ensemble d'associations SSDL doit avoir exactement deux terminaisons.</span><span class="sxs-lookup"><span data-stu-id="e9e51-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="e9e51-236">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-237">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-238">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-239">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="e9e51-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-240">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-240">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-241">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="e9e51-242">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-242">Attribute Name</span></span>   | <span data-ttu-id="e9e51-243">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-243">Is Required</span></span> | <span data-ttu-id="e9e51-244">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="e9e51-245">**Type**</span></span>         | <span data-ttu-id="e9e51-246">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-246">Yes</span></span>         | <span data-ttu-id="e9e51-247">Le nom qualifié complet du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="e9e51-248">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-248">**Role**</span></span>         | <span data-ttu-id="e9e51-249">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-249">No</span></span>          | <span data-ttu-id="e9e51-250">La valeur de la **rôle** attribut dans un élément Principal ou du dépendant de l’élément ReferentialConstraint correspondant (si utilisé).</span><span class="sxs-lookup"><span data-stu-id="e9e51-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="e9e51-251">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="e9e51-251">**Multiplicity**</span></span> | <span data-ttu-id="e9e51-252">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-252">Yes</span></span>         | <span data-ttu-id="e9e51-253">**1**, **valeur 0.. 1**, ou **\*** selon le nombre de lignes qui peuvent être à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="e9e51-254">**1** indique qu’exactement une ligne existe à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="e9e51-255">**valeur 0.. 1** indique que zéro ou une ligne existe à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="e9e51-256">**\*** Indique que zéro, un ou plusieurs lignes existent à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-257">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e9e51-258">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-259">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="e9e51-260">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-260">Example</span></span>

<span data-ttu-id="e9e51-261">L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e9e51-262">Le **multiplicité** valeurs spécifiées sur chaque **fin** élément indiquent que plusieurs lignes dans le **commandes** table peut être associée à une ligne dans le **clients**  table, mais qu’une seule ligne dans le **clients** table peut être associée à une ligne dans le **commandes** table.</span><span class="sxs-lookup"><span data-stu-id="e9e51-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="e9e51-263">En outre, le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **clients** table est supprimée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="e9e51-264">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="e9e51-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="e9e51-265">Le **fin** élément (en tant qu’enfant de le **AssociationSet** élément) spécifie une table à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="e9e51-266">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-267">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-268">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-269">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-269">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-270">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="e9e51-271">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-271">Attribute Name</span></span> | <span data-ttu-id="e9e51-272">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-272">Is Required</span></span> | <span data-ttu-id="e9e51-273">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="e9e51-274">**EntitySet**</span></span>  | <span data-ttu-id="e9e51-275">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-275">Yes</span></span>         | <span data-ttu-id="e9e51-276">Le nom du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="e9e51-277">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-277">**Role**</span></span>       | <span data-ttu-id="e9e51-278">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-278">No</span></span>          | <span data-ttu-id="e9e51-279">La valeur de l’un de le **rôle** spécifiés pour l’un attributs **fin** élément de l’élément Association correspondant.</span><span class="sxs-lookup"><span data-stu-id="e9e51-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-280">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="e9e51-281">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-282">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="e9e51-283">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-283">Example</span></span>

<span data-ttu-id="e9e51-284">L’exemple suivant montre un **EntityContainer** élément avec un **AssociationSet** élément avec deux **fin** éléments :</span><span class="sxs-lookup"><span data-stu-id="e9e51-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="e9e51-285">EntityContainer, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="e9e51-286">Un **EntityContainer** élément dans le langage de définition de schéma de magasin (SSDL) décrit la structure de la source de données sous-jacente dans une application Entity Framework : jeux d’entités SSDL (définis dans les éléments EntitySet) représente des tables dans une base de données, les types d’entité SSDL (définis dans les éléments EntityType) représentent des lignes dans une table, et ensembles d’associations (définis dans les éléments de l’AssociationSet) représentent des contraintes de clé étrangère dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="e9e51-287">Un conteneur d’entités de stockage modèle mappe à un conteneur d’entités de modèle conceptuel via l’élément EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="e9e51-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="e9e51-288">Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="e9e51-289">Si un **Documentation** élément est présent, il doit précéder tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="e9e51-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="e9e51-290">Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-291">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="e9e51-291">EntitySet</span></span>
-   <span data-ttu-id="e9e51-292">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="e9e51-292">AssociationSet</span></span>
-   <span data-ttu-id="e9e51-293">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-294">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-294">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-295">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="e9e51-296">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-296">Attribute Name</span></span> | <span data-ttu-id="e9e51-297">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-297">Is Required</span></span> | <span data-ttu-id="e9e51-298">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-299">**Name**</span></span>       | <span data-ttu-id="e9e51-300">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-300">Yes</span></span>         | <span data-ttu-id="e9e51-301">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="e9e51-301">The name of the entity container.</span></span> <span data-ttu-id="e9e51-302">Ce nom ne peut pas contenir de point (.).</span><span class="sxs-lookup"><span data-stu-id="e9e51-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-303">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="e9e51-304">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-305">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-306">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-306">Example</span></span>

<span data-ttu-id="e9e51-307">L’exemple suivant montre un **EntityContainer** élément qui définit deux jeux d’entités et un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="e9e51-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="e9e51-308">Notez que les noms de type d'entité et de type d'association sont qualifiés par le nom de l'espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="e9e51-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="e9e51-309">EntitySet, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="e9e51-310">Un **EntitySet** élément de langage de définition de schéma de magasin (SSDL) représente une table ou vue dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="e9e51-311">Un élément EntityType en langage SSDL représente une ligne dans la table ou vue.</span><span class="sxs-lookup"><span data-stu-id="e9e51-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="e9e51-312">Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="e9e51-313">Le mappage entre un jeu d’entités CSDL et un jeu d’entités SSDL est spécifié dans un élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="e9e51-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="e9e51-314">Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-315">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-316">DefiningQuery (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-317">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-318">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-318">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-319">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e9e51-320">Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias.</span><span class="sxs-lookup"><span data-stu-id="e9e51-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="e9e51-321">Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="e9e51-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="e9e51-322">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-322">Attribute Name</span></span> | <span data-ttu-id="e9e51-323">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-323">Is Required</span></span> | <span data-ttu-id="e9e51-324">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-325">**Name**</span></span>       | <span data-ttu-id="e9e51-326">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-326">Yes</span></span>         | <span data-ttu-id="e9e51-327">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="e9e51-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="e9e51-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="e9e51-328">**EntityType**</span></span> | <span data-ttu-id="e9e51-329">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-329">Yes</span></span>         | <span data-ttu-id="e9e51-330">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="e9e51-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="e9e51-331">**Schéma**</span><span class="sxs-lookup"><span data-stu-id="e9e51-331">**Schema**</span></span>     | <span data-ttu-id="e9e51-332">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-332">No</span></span>          | <span data-ttu-id="e9e51-333">Schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="e9e51-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="e9e51-334">**Table**</span></span>      | <span data-ttu-id="e9e51-335">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-335">No</span></span>          | <span data-ttu-id="e9e51-336">Table de base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="e9e51-337">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="e9e51-338">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-339">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-340">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-340">Example</span></span>

<span data-ttu-id="e9e51-341">L’exemple suivant montre un **EntityContainer** élément qui a deux **EntitySet** éléments et l’autre **AssociationSet** élément :</span><span class="sxs-lookup"><span data-stu-id="e9e51-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="e9e51-342">EntityType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="e9e51-343">Un **EntityType** élément de langage de définition de schéma de magasin (SSDL) représente une ligne dans une table ou vue de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="e9e51-344">Un élément EntitySet en SSDL représente la table ou vue dans laquelle les lignes résultent.</span><span class="sxs-lookup"><span data-stu-id="e9e51-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="e9e51-345">Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="e9e51-346">Le mappage entre un type d’entité SSDL et un type d’entité CSDL est spécifié dans un élément EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="e9e51-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="e9e51-347">Le **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-348">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-349">Clé (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="e9e51-350">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-351">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-351">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-352">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="e9e51-353">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-353">Attribute Name</span></span> | <span data-ttu-id="e9e51-354">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-354">Is Required</span></span> | <span data-ttu-id="e9e51-355">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-356">**Name**</span></span>       | <span data-ttu-id="e9e51-357">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-357">Yes</span></span>         | <span data-ttu-id="e9e51-358">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="e9e51-358">The name of the entity type.</span></span> <span data-ttu-id="e9e51-359">Cette valeur est habituellement la même que le nom de la table dans laquelle le type d'entité représente une ligne.</span><span class="sxs-lookup"><span data-stu-id="e9e51-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="e9e51-360">Cette valeur ne peut pas contenir de point (.).</span><span class="sxs-lookup"><span data-stu-id="e9e51-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-361">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="e9e51-362">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-363">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-364">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-364">Example</span></span>

<span data-ttu-id="e9e51-365">L’exemple suivant montre un **EntityType** élément avec deux propriétés :</span><span class="sxs-lookup"><span data-stu-id="e9e51-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="e9e51-366">Function, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-366">Function Element (SSDL)</span></span>

<span data-ttu-id="e9e51-367">Le **fonction** élément de langage de définition de schéma de magasin (SSDL) spécifie une procédure stockée qui existe dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="e9e51-368">Le **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-369">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-370">Paramètre (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="e9e51-371">CommandText (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="e9e51-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="e9e51-372">ReturnType (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="e9e51-373">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="e9e51-374">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="e9e51-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="e9e51-375">Les procédures stockées spécifiées dans le modèle de stockage peuvent être importées dans le modèle conceptuel d'une application.</span><span class="sxs-lookup"><span data-stu-id="e9e51-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="e9e51-376">Pour plus d’informations, consultez [interrogation avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="e9e51-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="e9e51-377">Le **fonction** élément peut également être utilisé pour définir des fonctions personnalisées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-378">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-378">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-379">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e9e51-380">Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias.</span><span class="sxs-lookup"><span data-stu-id="e9e51-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="e9e51-381">Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="e9e51-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="e9e51-382">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-382">Attribute Name</span></span>             | <span data-ttu-id="e9e51-383">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-383">Is Required</span></span> | <span data-ttu-id="e9e51-384">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-385">**Name**</span></span>                   | <span data-ttu-id="e9e51-386">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-386">Yes</span></span>         | <span data-ttu-id="e9e51-387">Nom de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e9e51-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="e9e51-388">**ReturnType**</span></span>             | <span data-ttu-id="e9e51-389">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-389">No</span></span>          | <span data-ttu-id="e9e51-390">Type de retour de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="e9e51-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="e9e51-391">**Aggregate**</span></span>              | <span data-ttu-id="e9e51-392">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-392">No</span></span>          | <span data-ttu-id="e9e51-393">**True** si la procédure stockée retourne une valeur d’agrégation ; sinon **False**.</span><span class="sxs-lookup"><span data-stu-id="e9e51-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="e9e51-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="e9e51-394">**BuiltIn**</span></span>                | <span data-ttu-id="e9e51-395">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-395">No</span></span>          | <span data-ttu-id="e9e51-396">**True** si la fonction est intégré<sup>1</sup> fonction ; sinon **False**.</span><span class="sxs-lookup"><span data-stu-id="e9e51-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="e9e51-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="e9e51-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="e9e51-398">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-398">No</span></span>          | <span data-ttu-id="e9e51-399">Nom de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e9e51-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="e9e51-400">**NiladicFunction**</span></span>        | <span data-ttu-id="e9e51-401">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-401">No</span></span>          | <span data-ttu-id="e9e51-402">**True** si la fonction est un niladiques<sup>2</sup> fonction ; **False** dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="e9e51-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="e9e51-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="e9e51-403">**IsComposable**</span></span>           | <span data-ttu-id="e9e51-404">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-404">No</span></span>          | <span data-ttu-id="e9e51-405">**True** si la fonction est un élément composable<sup>3</sup> fonction ; **False** dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="e9e51-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="e9e51-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="e9e51-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="e9e51-407">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-407">No</span></span>          | <span data-ttu-id="e9e51-408">Énumération qui définit la sémantique de type utilisée pour résoudre les surcharges de fonction.</span><span class="sxs-lookup"><span data-stu-id="e9e51-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="e9e51-409">L'énumération est définie dans le manifeste du fournisseur par définition de fonction.</span><span class="sxs-lookup"><span data-stu-id="e9e51-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="e9e51-410">La valeur par défaut est **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="e9e51-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="e9e51-411">**Schéma**</span><span class="sxs-lookup"><span data-stu-id="e9e51-411">**Schema**</span></span>                 | <span data-ttu-id="e9e51-412">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-412">No</span></span>          | <span data-ttu-id="e9e51-413">Nom du schéma dans lequel une procédure stockée est définie.</span><span class="sxs-lookup"><span data-stu-id="e9e51-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="e9e51-414"><sup>1</sup> une fonction intégrée est une fonction qui est définie dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="e9e51-415">Pour plus d’informations sur les fonctions qui sont définis dans le modèle de stockage, consultez l’élément CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e9e51-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="e9e51-416"><sup>2</sup> une fonction niladique est une fonction qui n’accepte aucun paramètre et, lorsqu’elle est appelée, ne nécessite pas de parenthèses.</span><span class="sxs-lookup"><span data-stu-id="e9e51-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="e9e51-417"><sup>3</sup> deux fonctions sont composables si la sortie d’une fonction peut être l’entrée de l’autre fonction.</span><span class="sxs-lookup"><span data-stu-id="e9e51-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="e9e51-418">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="e9e51-419">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-420">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-421">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-421">Example</span></span>

<span data-ttu-id="e9e51-422">L’exemple suivant montre un **fonction** élément qui correspond à la **UpdateOrderQuantity** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="e9e51-423">La procédure stockée accepte deux paramètres et ne retourne pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="e9e51-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="e9e51-424">Key, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-424">Key Element (SSDL)</span></span>

<span data-ttu-id="e9e51-425">Le **clé** élément de langage de définition de schéma de magasin (SSDL) représente la clé primaire d’une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="e9e51-426">**Clé** est un élément enfant d’un élément EntityType, qui représente une ligne dans une table.</span><span class="sxs-lookup"><span data-stu-id="e9e51-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="e9e51-427">La clé primaire est définie dans le **clé** élément en faisant référence à un ou plusieurs éléments de propriété sont définis sur le **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="e9e51-428">Le **clé** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-429">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="e9e51-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e9e51-430">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-430">Annotation elements</span></span>

<span data-ttu-id="e9e51-431">Aucun attribut n’est applicable à la **clé** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-432">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-432">Example</span></span>

<span data-ttu-id="e9e51-433">L’exemple suivant montre un **EntityType** élément avec une clé qui fait référence à une seule propriété :</span><span class="sxs-lookup"><span data-stu-id="e9e51-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="e9e51-434">OnDelete, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="e9e51-435">Le **OnDelete** élément dans le langage de définition de schéma de magasin (SSDL) reflète le comportement de la base de données lorsqu’une ligne qui participe à une contrainte de clé étrangère est supprimée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="e9e51-436">Si l’action est définie sur **Cascade**, puis les lignes qui font référence à une ligne qui est en cours de suppression seront également supprimées.</span><span class="sxs-lookup"><span data-stu-id="e9e51-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="e9e51-437">Si l’action est définie sur **aucun**, les lignes qui font référence à une ligne qui est en cours de suppression ne sont pas également supprimées.</span><span class="sxs-lookup"><span data-stu-id="e9e51-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="e9e51-438">Un **OnDelete** élément est un élément enfant d’un élément de fin.</span><span class="sxs-lookup"><span data-stu-id="e9e51-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="e9e51-439">Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-440">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-441">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-442">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-442">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-443">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="e9e51-444">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-444">Attribute Name</span></span> | <span data-ttu-id="e9e51-445">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-445">Is Required</span></span> | <span data-ttu-id="e9e51-446">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-447">**Action**</span><span class="sxs-lookup"><span data-stu-id="e9e51-447">**Action**</span></span>     | <span data-ttu-id="e9e51-448">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-448">Yes</span></span>         | <span data-ttu-id="e9e51-449">**Cascade** ou **aucun**.</span><span class="sxs-lookup"><span data-stu-id="e9e51-449">**Cascade** or **None**.</span></span> <span data-ttu-id="e9e51-450">(La valeur **Restricted** est valide mais a le même comportement que **aucun**.)</span><span class="sxs-lookup"><span data-stu-id="e9e51-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-451">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="e9e51-452">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-453">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-454">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-454">Example</span></span>

<span data-ttu-id="e9e51-455">L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="e9e51-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e9e51-456">Le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **Clients** table est supprimée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="e9e51-457">Élément Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="e9e51-458">Le **paramètre** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui spécifie les paramètres pour une procédure stockée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="e9e51-459">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-460">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-461">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-462">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-462">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-463">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="e9e51-464">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-464">Attribute Name</span></span> | <span data-ttu-id="e9e51-465">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-465">Is Required</span></span> | <span data-ttu-id="e9e51-466">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-467">**Name**</span></span>       | <span data-ttu-id="e9e51-468">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-468">Yes</span></span>         | <span data-ttu-id="e9e51-469">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="e9e51-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="e9e51-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="e9e51-470">**Type**</span></span>       | <span data-ttu-id="e9e51-471">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-471">Yes</span></span>         | <span data-ttu-id="e9e51-472">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="e9e51-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="e9e51-473">**Mode**</span><span class="sxs-lookup"><span data-stu-id="e9e51-473">**Mode**</span></span>       | <span data-ttu-id="e9e51-474">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-474">No</span></span>          | <span data-ttu-id="e9e51-475">**Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="e9e51-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="e9e51-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e9e51-476">**MaxLength**</span></span>  | <span data-ttu-id="e9e51-477">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-477">No</span></span>          | <span data-ttu-id="e9e51-478">Longueur maximale du paramètre.</span><span class="sxs-lookup"><span data-stu-id="e9e51-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="e9e51-479">**Précision**</span><span class="sxs-lookup"><span data-stu-id="e9e51-479">**Precision**</span></span>  | <span data-ttu-id="e9e51-480">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-480">No</span></span>          | <span data-ttu-id="e9e51-481">Précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="e9e51-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="e9e51-482">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-482">**Scale**</span></span>      | <span data-ttu-id="e9e51-483">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-483">No</span></span>          | <span data-ttu-id="e9e51-484">Échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="e9e51-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="e9e51-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e9e51-485">**SRID**</span></span>       | <span data-ttu-id="e9e51-486">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-486">No</span></span>          | <span data-ttu-id="e9e51-487">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="e9e51-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e9e51-488">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="e9e51-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="e9e51-489">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9e51-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-490">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="e9e51-491">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-492">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-493">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-493">Example</span></span>

<span data-ttu-id="e9e51-494">L’exemple suivant montre un **fonction** élément qui a deux **paramètre** éléments qui spécifient les paramètres d’entrée :</span><span class="sxs-lookup"><span data-stu-id="e9e51-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="e9e51-495">Élément Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="e9e51-496">Le **Principal** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte de clé étrangère (également appelée une contrainte référentielle).</span><span class="sxs-lookup"><span data-stu-id="e9e51-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="e9e51-497">Le **Principal** élément spécifie la colonne de clé primaire (ou les colonnes) dans une table qui est référencée par un autre (ou les colonnes).</span><span class="sxs-lookup"><span data-stu-id="e9e51-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="e9e51-498">**PropertyRef** éléments spécifient quelles colonnes sont référencées.</span><span class="sxs-lookup"><span data-stu-id="e9e51-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="e9e51-499">L’élément dépendant spécifie les colonnes qui font référence les colonnes de clés primaires sont spécifiés dans le **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="e9e51-500">Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="e9e51-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e9e51-501">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="e9e51-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="e9e51-502">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-503">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-503">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-504">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="e9e51-505">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-505">Attribute Name</span></span> | <span data-ttu-id="e9e51-506">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-506">Is Required</span></span> | <span data-ttu-id="e9e51-507">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-508">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-508">**Role**</span></span>       | <span data-ttu-id="e9e51-509">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-509">Yes</span></span>         | <span data-ttu-id="e9e51-510">La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne référencée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-511">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="e9e51-512">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-513">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-514">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-514">Example</span></span>

<span data-ttu-id="e9e51-515">L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte.</span><span class="sxs-lookup"><span data-stu-id="e9e51-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="e9e51-516">Le **Principal** élément spécifie la **CustomerId** colonne de la **client** table en tant que la terminaison principale de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="e9e51-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="e9e51-517">Property, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-517">Property Element (SSDL)</span></span>

<span data-ttu-id="e9e51-518">Le **propriété** élément de langage de définition de schéma de magasin (SSDL) représente une colonne dans une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="e9e51-519">**Propriété** éléments sont des enfants d’éléments EntityType, qui représentent des lignes dans une table.</span><span class="sxs-lookup"><span data-stu-id="e9e51-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="e9e51-520">Chaque **propriété** élément défini sur une **EntityType** élément représente une colonne.</span><span class="sxs-lookup"><span data-stu-id="e9e51-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="e9e51-521">Un **propriété** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="e9e51-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-522">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-522">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-523">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="e9e51-524">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-524">Attribute Name</span></span>            | <span data-ttu-id="e9e51-525">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-525">Is Required</span></span> | <span data-ttu-id="e9e51-526">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-527">**Name**</span></span>                  | <span data-ttu-id="e9e51-528">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-528">Yes</span></span>         | <span data-ttu-id="e9e51-529">Nom de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="e9e51-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="e9e51-530">**Type**</span></span>                  | <span data-ttu-id="e9e51-531">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-531">Yes</span></span>         | <span data-ttu-id="e9e51-532">Type de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="e9e51-533">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="e9e51-533">**Nullable**</span></span>              | <span data-ttu-id="e9e51-534">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-534">No</span></span>          | <span data-ttu-id="e9e51-535">**True** (la valeur par défaut) ou **False** selon si la colonne correspondante peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="e9e51-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="e9e51-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="e9e51-536">**DefaultValue**</span></span>          | <span data-ttu-id="e9e51-537">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-537">No</span></span>          | <span data-ttu-id="e9e51-538">Valeur par défaut de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e9e51-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e9e51-539">**MaxLength**</span></span>             | <span data-ttu-id="e9e51-540">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-540">No</span></span>          | <span data-ttu-id="e9e51-541">Longueur maximale de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="e9e51-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e9e51-542">**FixedLength**</span></span>           | <span data-ttu-id="e9e51-543">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-543">No</span></span>          | <span data-ttu-id="e9e51-544">**True** ou **False** selon si la valeur de colonne correspondante sera être stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="e9e51-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="e9e51-545">**Précision**</span><span class="sxs-lookup"><span data-stu-id="e9e51-545">**Precision**</span></span>             | <span data-ttu-id="e9e51-546">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-546">No</span></span>          | <span data-ttu-id="e9e51-547">Précision de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="e9e51-548">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-548">**Scale**</span></span>                 | <span data-ttu-id="e9e51-549">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-549">No</span></span>          | <span data-ttu-id="e9e51-550">Échelle de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="e9e51-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="e9e51-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="e9e51-551">**Unicode**</span></span>               | <span data-ttu-id="e9e51-552">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-552">No</span></span>          | <span data-ttu-id="e9e51-553">**True** ou **False** selon si la valeur de colonne correspondante sera être stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="e9e51-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="e9e51-554">**classement**</span><span class="sxs-lookup"><span data-stu-id="e9e51-554">**Collation**</span></span>             | <span data-ttu-id="e9e51-555">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-555">No</span></span>          | <span data-ttu-id="e9e51-556">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="e9e51-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="e9e51-557">**SRID**</span></span>                  | <span data-ttu-id="e9e51-558">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-558">No</span></span>          | <span data-ttu-id="e9e51-559">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="e9e51-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="e9e51-560">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="e9e51-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="e9e51-561">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9e51-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="e9e51-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="e9e51-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="e9e51-563">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-563">No</span></span>          | <span data-ttu-id="e9e51-564">**Aucun**, **identité** (si la valeur de colonne correspondante est une identité qui est générée dans la base de données), ou **calculé** (si la valeur de colonne correspondante est calculée dans la base de données).</span><span class="sxs-lookup"><span data-stu-id="e9e51-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="e9e51-565">Non valide pour les propriétés de RowType.</span><span class="sxs-lookup"><span data-stu-id="e9e51-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-566">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="e9e51-567">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-568">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-569">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-569">Example</span></span>

<span data-ttu-id="e9e51-570">L’exemple suivant montre un **EntityType** élément avec deux enfants **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="e9e51-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="e9e51-571">Élément PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="e9e51-572">Le **PropertyRef** élément dans le langage de définition de schéma de magasin (SSDL) fait référence à une propriété définie sur un élément EntityType pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="e9e51-573">Faire partie de la clé primaire de la table qui la **EntityType** représente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="e9e51-574">Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="e9e51-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="e9e51-575">Pour plus d’informations, consultez l’élément Key.</span><span class="sxs-lookup"><span data-stu-id="e9e51-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="e9e51-576">Faire partie de la terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="e9e51-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="e9e51-577">Pour plus d’informations, consultez ReferentialConstraint (élément).</span><span class="sxs-lookup"><span data-stu-id="e9e51-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="e9e51-578">Le **PropertyRef** élément peut avoir uniquement les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="e9e51-579">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-580">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="e9e51-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-581">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-581">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-582">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="e9e51-583">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-583">Attribute Name</span></span> | <span data-ttu-id="e9e51-584">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-584">Is Required</span></span> | <span data-ttu-id="e9e51-585">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="e9e51-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="e9e51-586">**Name**</span></span>       | <span data-ttu-id="e9e51-587">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-587">Yes</span></span>         | <span data-ttu-id="e9e51-588">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="e9e51-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="e9e51-589">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="e9e51-590">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="e9e51-591">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-592">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-592">Example</span></span>

<span data-ttu-id="e9e51-593">L’exemple suivant montre un **PropertyRef** élément utilisé pour définir une clé primaire en référençant une propriété qui est définie sur une **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="e9e51-594">ReferentialConstraint, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="e9e51-595">Le **ReferentialConstraint** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère (également appelée une contrainte d’intégrité référentielle) dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="e9e51-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="e9e51-596">Les terminaisons principales et dépendantes de la contrainte sont spécifiées par les éléments enfants principale et dépendante, respectivement.</span><span class="sxs-lookup"><span data-stu-id="e9e51-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="e9e51-597">Les colonnes qui participent les terminaisons principale et dépendantes sont référencées avec les éléments PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="e9e51-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="e9e51-598">Le **ReferentialConstraint** élément est un élément enfant facultatif de l’élément Association.</span><span class="sxs-lookup"><span data-stu-id="e9e51-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="e9e51-599">Si un **ReferentialConstraint** élément n’est pas utilisé pour mapper la contrainte de clé étrangère est spécifiée dans le **Association** élément AssociationSetMapping élément doit être utilisé pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="e9e51-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="e9e51-600">Le **ReferentialConstraint** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="e9e51-601">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="e9e51-602">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="e9e51-603">Dépendante (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="e9e51-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="e9e51-604">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="e9e51-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-605">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-605">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-606">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="e9e51-607">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-608">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-609">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-609">Example</span></span>

<span data-ttu-id="e9e51-610">L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="e9e51-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="e9e51-611">ReturnType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="e9e51-612">Le **ReturnType** élément dans le langage de définition de schéma de magasin (SSDL) spécifie le type de retour pour une fonction qui est défini dans un **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="e9e51-613">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="e9e51-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="e9e51-614">Le type de retour d’une fonction est spécifié avec la **Type** attribut ou le **ReturnType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="e9e51-615">Le **ReturnType** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="e9e51-616">CollectionType (un)</span><span class="sxs-lookup"><span data-stu-id="e9e51-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="e9e51-617">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="e9e51-618">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="e9e51-619">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-620">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-620">Example</span></span>

<span data-ttu-id="e9e51-621">L’exemple suivant utilise un **fonction** qui retourne une collection de lignes.</span><span class="sxs-lookup"><span data-stu-id="e9e51-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="e9e51-622">RowType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="e9e51-623">Un **RowType** élément dans le langage de définition de schéma de magasin (SSDL) définit une structure sans nom comme un retour de type pour une fonction définie dans le magasin.</span><span class="sxs-lookup"><span data-stu-id="e9e51-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="e9e51-624">Un **RowType** élément est l’élément enfant de **CollectionType** élément :</span><span class="sxs-lookup"><span data-stu-id="e9e51-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="e9e51-625">Un **RowType** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="e9e51-626">Propriété (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="e9e51-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="e9e51-627">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-627">Example</span></span>

<span data-ttu-id="e9e51-628">L’exemple suivant montre une fonction de magasin qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="e9e51-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="e9e51-629">Schema, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="e9e51-630">Le **schéma** élément dans le langage de définition de schéma de magasin (SSDL) est l’élément racine d’une définition de modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="e9e51-631">Il contient des définitions pour les objets, les fonctions et les conteneurs qui composent un modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="e9e51-632">Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="e9e51-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="e9e51-633">Association</span><span class="sxs-lookup"><span data-stu-id="e9e51-633">Association</span></span>
-   <span data-ttu-id="e9e51-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="e9e51-634">EntityType</span></span>
-   <span data-ttu-id="e9e51-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="e9e51-635">EntityContainer</span></span>
-   <span data-ttu-id="e9e51-636">Fonction</span><span class="sxs-lookup"><span data-stu-id="e9e51-636">Function</span></span>

<span data-ttu-id="e9e51-637">Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour les objets de type et l’association d’entité dans un modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="e9e51-638">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="e9e51-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="e9e51-639">Un espace de noms de modèle de stockage est différent de l’espace de noms XML de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="e9e51-640">Un espace de noms de modèle de stockage (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités et types d’associations.</span><span class="sxs-lookup"><span data-stu-id="e9e51-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="e9e51-641">L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="e9e51-642">Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="e9e51-643">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="e9e51-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e9e51-644">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="e9e51-644">Applicable Attributes</span></span>

<span data-ttu-id="e9e51-645">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="e9e51-646">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="e9e51-646">Attribute Name</span></span>            | <span data-ttu-id="e9e51-647">Requis</span><span class="sxs-lookup"><span data-stu-id="e9e51-647">Is Required</span></span> | <span data-ttu-id="e9e51-648">Value</span><span class="sxs-lookup"><span data-stu-id="e9e51-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-649">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="e9e51-649">**Namespace**</span></span>             | <span data-ttu-id="e9e51-650">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-650">Yes</span></span>         | <span data-ttu-id="e9e51-651">Espace de noms du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-651">The namespace of the storage model.</span></span> <span data-ttu-id="e9e51-652">La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="e9e51-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="e9e51-653">Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms ExampleModel.Store, le nom qualifié complet de le **EntityType** est ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="e9e51-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="e9e51-654">Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="e9e51-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="e9e51-655">La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma CSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="e9e51-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="e9e51-656">**Alias**</span></span>                 | <span data-ttu-id="e9e51-657">Non</span><span class="sxs-lookup"><span data-stu-id="e9e51-657">No</span></span>          | <span data-ttu-id="e9e51-658">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="e9e51-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="e9e51-659">Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms ExampleModel.Store et la valeur de la **Alias** attribut est *StorageModel*, vous pouvez utiliser Storagemodel.client comme nom qualifié complet de le **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="e9e51-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="e9e51-660">**Fournisseur**</span><span class="sxs-lookup"><span data-stu-id="e9e51-660">**Provider**</span></span>              | <span data-ttu-id="e9e51-661">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-661">Yes</span></span>         | <span data-ttu-id="e9e51-662">Fournisseur de données.</span><span class="sxs-lookup"><span data-stu-id="e9e51-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="e9e51-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="e9e51-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="e9e51-664">Oui</span><span class="sxs-lookup"><span data-stu-id="e9e51-664">Yes</span></span>         | <span data-ttu-id="e9e51-665">Jeton qui indique au fournisseur quel manifeste de fournisseur retourner.</span><span class="sxs-lookup"><span data-stu-id="e9e51-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="e9e51-666">Aucun format n'est défini pour le jeton.</span><span class="sxs-lookup"><span data-stu-id="e9e51-666">No format for the token is defined.</span></span> <span data-ttu-id="e9e51-667">Les valeurs du jeton sont définies par le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="e9e51-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="e9e51-668">Pour plus d’informations sur les jetons du manifeste du fournisseur SQL Server, consultez SqlClient pour Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9e51-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="e9e51-669">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-669">Example</span></span>

<span data-ttu-id="e9e51-670">L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="e9e51-671">Attributs d'annotation</span><span class="sxs-lookup"><span data-stu-id="e9e51-671">Annotation Attributes</span></span>

<span data-ttu-id="e9e51-672">Les attributs d'annotation en SSDL (Store Schema Definition Language) sont des attributs XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant les éléments dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="e9e51-673">En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux attributs d'annotation :</span><span class="sxs-lookup"><span data-stu-id="e9e51-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="e9e51-674">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="e9e51-675">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="e9e51-676">Plusieurs attributs d'annotation peuvent être appliqués à un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="e9e51-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="e9e51-677">Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="e9e51-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-678">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-678">Example</span></span>

<span data-ttu-id="e9e51-679">L’exemple suivant montre un élément EntityType qui a un attribut d’annotation appliqué à la **OrderId** propriété.</span><span class="sxs-lookup"><span data-stu-id="e9e51-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="e9e51-680">L’exemple montre également un élément d’annotation ajouté à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="e9e51-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="e9e51-681">Éléments d'annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="e9e51-682">Les éléments d'annotation en SSDL (Store Schema Definition Language) sont des éléments XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="e9e51-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="e9e51-683">En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux éléments d'annotation :</span><span class="sxs-lookup"><span data-stu-id="e9e51-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="e9e51-684">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé au langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="e9e51-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="e9e51-685">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="e9e51-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="e9e51-686">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="e9e51-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="e9e51-687">Plusieurs éléments d'annotation peuvent être des enfants d'un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="e9e51-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="e9e51-688">À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="e9e51-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="e9e51-689">Exemple</span><span class="sxs-lookup"><span data-stu-id="e9e51-689">Example</span></span>

<span data-ttu-id="e9e51-690">L’exemple suivant montre un élément EntityType qui a un élément d’annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="e9e51-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="e9e51-691">L’exemple montre également un attribut d’annotation appliqué à la **OrderId** propriété.</span><span class="sxs-lookup"><span data-stu-id="e9e51-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="e9e51-692">Facettes (SSDL)</span><span class="sxs-lookup"><span data-stu-id="e9e51-692">Facets (SSDL)</span></span>

<span data-ttu-id="e9e51-693">Les facettes dans le langage de définition de schéma de magasin (SSDL) représentent des contraintes sur les types de colonnes qui sont spécifiés dans les éléments de propriété.</span><span class="sxs-lookup"><span data-stu-id="e9e51-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="e9e51-694">Les facettes sont implémentées comme des attributs XML sur **propriété** éléments.</span><span class="sxs-lookup"><span data-stu-id="e9e51-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="e9e51-695">Le tableau ci-dessous décrit les facettes prises en charge dans le langage SSDL :</span><span class="sxs-lookup"><span data-stu-id="e9e51-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="e9e51-696">Facette</span><span class="sxs-lookup"><span data-stu-id="e9e51-696">Facet</span></span>           | <span data-ttu-id="e9e51-697">Description</span><span class="sxs-lookup"><span data-stu-id="e9e51-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e9e51-698">**classement**</span><span class="sxs-lookup"><span data-stu-id="e9e51-698">**Collation**</span></span>   | <span data-ttu-id="e9e51-699">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="e9e51-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="e9e51-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="e9e51-700">**FixedLength**</span></span> | <span data-ttu-id="e9e51-701">Spécifie si la longueur de la valeur de colonne peut varier.</span><span class="sxs-lookup"><span data-stu-id="e9e51-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="e9e51-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="e9e51-702">**MaxLength**</span></span>   | <span data-ttu-id="e9e51-703">Spécifie la longueur maximale de la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="e9e51-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="e9e51-704">**Précision**</span><span class="sxs-lookup"><span data-stu-id="e9e51-704">**Precision**</span></span>   | <span data-ttu-id="e9e51-705">Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="e9e51-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="e9e51-706">Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="e9e51-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="e9e51-707">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="e9e51-707">**Scale**</span></span>       | <span data-ttu-id="e9e51-708">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="e9e51-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="e9e51-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="e9e51-709">**Unicode**</span></span>     | <span data-ttu-id="e9e51-710">Indique si la valeur de colonne est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="e9e51-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
