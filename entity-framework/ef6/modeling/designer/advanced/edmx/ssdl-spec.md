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
# <a name="ssdl-specification"></a><span data-ttu-id="cd8e0-102">Spécification SSDL</span><span class="sxs-lookup"><span data-stu-id="cd8e0-102">SSDL Specification</span></span>
<span data-ttu-id="cd8e0-103">SSDL (Store Schema Definition Language) est un langage basé sur XML qui décrit le modèle de stockage d'une application Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="cd8e0-104">Dans une application Entity Framework, les métadonnées de modèle de stockage est chargée à partir d’un fichier .ssdl (écrit en SSDL) dans une instance de la System.Data.Metadata.Edm.StoreItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="cd8e0-105">Entity Framework utilise des métadonnées de modèle de stockage pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques au magasin.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="cd8e0-106">Entity Framework Designer (Concepteur d’EF) stocke des informations de modèle de stockage dans un fichier .edmx au moment du design.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="cd8e0-107">Au moment de la génération du Concepteur d’entités utilise les informations dans un fichier .edmx pour créer le fichier .ssdl qui est nécessaire par Entity Framework lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="cd8e0-108">Les versions de SSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="cd8e0-109">Version du langage SSDL</span><span class="sxs-lookup"><span data-stu-id="cd8e0-109">SSDL Version</span></span> | <span data-ttu-id="cd8e0-110">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="cd8e0-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="cd8e0-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="cd8e0-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="cd8e0-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="cd8e0-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="cd8e0-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="cd8e0-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="cd8e0-114">Association, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-114">Association Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-115">Un **Association** élément de langage de définition de schéma de magasin (SSDL) spécifie les colonnes de table qui participent à dans une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="cd8e0-116">Deux éléments de fin enfant obligatoire spécifient des tables situées aux terminaisons de l’association et la multiplicité à chaque extrémité.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="cd8e0-117">Un élément ReferentialConstraint facultatif spécifie les terminaisons principales et dépendantes de l’association, ainsi que les colonnes participantes.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="cd8e0-118">Si aucun **ReferentialConstraint** élément est présent, un élément AssociationSetMapping doit être utilisé pour spécifier les mappages de colonnes pour l’association.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="cd8e0-119">Le **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-120">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-121">Fin (exactement deux éléments)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-121">End (exactly two)</span></span>
-   <span data-ttu-id="cd8e0-122">ReferentialConstraint (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-123">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-124">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-124">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-125">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="cd8e0-126">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-126">Attribute Name</span></span> | <span data-ttu-id="cd8e0-127">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-127">Is Required</span></span> | <span data-ttu-id="cd8e0-128">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-129">**Name**</span></span>       | <span data-ttu-id="cd8e0-130">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-130">Yes</span></span>         | <span data-ttu-id="cd8e0-131">Nom de la contrainte de clé étrangère correspondante dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-132">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="cd8e0-133">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-134">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-135">Example</span></span>

<span data-ttu-id="cd8e0-136">L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="cd8e0-137">AssociationSet, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-138">Le **AssociationSet** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère entre deux tables dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="cd8e0-139">Les colonnes de table qui participent à la contrainte de clé étrangère sont spécifiées dans un élément de l’Association.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="cd8e0-140">Le **Association** élément qui correspond à une donnée **AssociationSet** élément est spécifié dans le **Association** attribut de la **AssociationSet**  élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="cd8e0-141">Ensembles d’associations SSDL sont mappés aux ensembles d’associations CSDL à un élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="cd8e0-142">Toutefois, si l’ensemble d’associations CSDL pour une association CSDL donnée est défini à l’aide d’un élément ReferentialConstraint, aucune correspondant **AssociationSetMapping** élément n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="cd8e0-143">Dans ce cas, si un **AssociationSetMapping** élément est présent, les mappages, il définit seront remplacées par les **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="cd8e0-144">Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-145">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-146">Fin (zéro, un ou deux)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-146">End (zero or two)</span></span>
-   <span data-ttu-id="cd8e0-147">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-148">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-148">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-149">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="cd8e0-150">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-150">Attribute Name</span></span>  | <span data-ttu-id="cd8e0-151">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-151">Is Required</span></span> | <span data-ttu-id="cd8e0-152">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-153">**Name**</span></span>        | <span data-ttu-id="cd8e0-154">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-154">Yes</span></span>         | <span data-ttu-id="cd8e0-155">Nom de la contrainte de clé étrangère que l'ensemble d'associations représente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="cd8e0-156">**Association**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-156">**Association**</span></span> | <span data-ttu-id="cd8e0-157">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-157">Yes</span></span>         | <span data-ttu-id="cd8e0-158">Nom de l'association qui définit les colonnes qui participent à la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-159">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="cd8e0-160">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-161">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-162">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-162">Example</span></span>

<span data-ttu-id="cd8e0-163">L’exemple suivant montre un **AssociationSet** élément qui représente le `FK_CustomerOrders` une contrainte de clé étrangère dans la base de données sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="cd8e0-164">CollectionType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-165">Le **CollectionType** élément de langage de définition de schéma de magasin (SSDL) Spécifie que le type de retour d’une fonction est une collection.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="cd8e0-166">Le **CollectionType** élément est un enfant de l’élément ReturnType.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="cd8e0-167">Le type de collection est spécifié à l’aide de l’élément enfant de RowType :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e0-168">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="cd8e0-169">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-170">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-171">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-171">Example</span></span>

<span data-ttu-id="cd8e0-172">L’exemple suivant montre une fonction qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="cd8e0-173">CommandText, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-174">Le **CommandText** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui vous permet de définir une instruction SQL qui est exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="cd8e0-175">Le **CommandText** élément vous permet d’ajouter des fonctionnalités sont similaire à une procédure stockée dans la base de données, mais que vous définissez le **CommandText** élément dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="cd8e0-176">Le **CommandText** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="cd8e0-177">Le corps de la **CommandText** élément doit être une instruction SQL valide pour la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="cd8e0-178">Aucun attribut n’est applicable à la **CommandText** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-179">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-179">Example</span></span>

<span data-ttu-id="cd8e0-180">L’exemple suivant montre un **fonction** élément avec un enfant **CommandText** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="cd8e0-181">Exposer le **UpdateProductInOrder** fonctionner en tant que méthode sur ObjectContext en l’important dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="cd8e0-182">DefiningQuery, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-183">Le **DefiningQuery** élément dans le langage de définition de schéma de magasin (SSDL) vous permet d’exécuter une instruction SQL directement dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="cd8e0-184">Le **DefiningQuery** élément est couramment utilisé comme une vue de base de données, mais la vue est définie dans le modèle de stockage au lieu de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="cd8e0-185">La vue définie dans un **DefiningQuery** élément peut être mappé à un type d’entité dans le modèle conceptuel via un élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="cd8e0-186">Ces mappages sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-186">These mappings are read-only.</span></span>  

<span data-ttu-id="cd8e0-187">La syntaxe SSDL suivante illustre la déclaration d’un **EntitySet** suivie de la **DefiningQuery** élément qui contient une requête utilisée pour récupérer la vue.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="cd8e0-188">Vous pouvez utiliser des procédures stockées dans Entity Framework pour activer des scénarios en lecture-écriture sur des vues.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="cd8e0-189">Vous pouvez utiliser une vue de source de données ou une vue Entity SQL en tant que la table de base pour la récupération des données et traitement des modifications par des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="cd8e0-190">Vous pouvez utiliser la **DefiningQuery** élément à cibler Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="cd8e0-191">Bien que SQL Server Compact 3.5 ne prend pas en charge les procédures stockées, vous pouvez implémenter des fonctionnalités similaires avec le **DefiningQuery** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="cd8e0-192">Cet élément peut s'avérer également utile pour créer des procédures stockées afin de surmonter une incompatibilité entre les types de données utilisés dans le langage de programmation et ceux de la source de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="cd8e0-193">Vous pouvez écrire un **DefiningQuery** qui prend un certain ensemble de paramètres et appelle ensuite une procédure stockée avec un autre ensemble de paramètres, par exemple, une procédure stockée qui supprime des données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="cd8e0-194">Élément Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-195">Le **dépendants** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison dépendante d’une contrainte de clé étrangère (également appelée une contrainte référentielle).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="cd8e0-196">Le **dépendants** élément spécifie la (ou les colonnes) dans une table qui font référence à une colonne clé primaire (ou les colonnes).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="cd8e0-197">**PropertyRef** éléments spécifient quelles colonnes sont référencées.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="cd8e0-198">Le Principal élément spécifie les colonnes de clés primaires référencées par les colonnes qui sont spécifiés dans le **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="cd8e0-199">Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-200">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="cd8e0-201">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-202">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-202">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-203">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="cd8e0-204">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-204">Attribute Name</span></span> | <span data-ttu-id="cd8e0-205">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-205">Is Required</span></span> | <span data-ttu-id="cd8e0-206">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-207">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-207">**Role**</span></span>       | <span data-ttu-id="cd8e0-208">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-208">Yes</span></span>         | <span data-ttu-id="cd8e0-209">La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne de référencement.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-210">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="cd8e0-211">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-212">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-213">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-213">Example</span></span>

<span data-ttu-id="cd8e0-214">L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="cd8e0-215">Le **dépendants** élément spécifie la **CustomerId** colonne de la **ordre** table en tant que la terminaison dépendante de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="cd8e0-216">Documentation, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-217">Le **Documentation** élément dans le langage de définition de schéma de magasin (SSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="cd8e0-218">Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-219">**Résumé**: une brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="cd8e0-220">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-220">(zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-221">**LongDescription**: une description détaillée de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="cd8e0-222">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-223">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-223">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-224">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="cd8e0-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-227">Example</span></span>

<span data-ttu-id="cd8e0-228">L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="cd8e0-229">End, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-229">End Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-230">Le **fin** élément de langage de définition de schéma de magasin (SSDL) spécifie la table et le nombre de lignes à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="cd8e0-231">Le **fin** élément peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="cd8e0-232">Dans chaque cas, les éléments enfants et les attributs applicables possibles sont différents.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="cd8e0-233">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="cd8e0-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="cd8e0-234">Un **fin** élément (en tant qu’enfant de le **Association** élément) spécifie la table et le nombre de lignes à la fin d’une contrainte de clé étrangère avec la **Type** et **Multiplicité** respectivement.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="cd8e0-235">Les terminaisons d'une contrainte de clé étrangère sont définies dans le cadre d'un ensemble d'associations SSDL ; un ensemble d'associations SSDL doit avoir exactement deux terminaisons.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="cd8e0-236">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-237">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-238">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-239">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-240">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-240">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-241">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="cd8e0-242">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-242">Attribute Name</span></span>   | <span data-ttu-id="cd8e0-243">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-243">Is Required</span></span> | <span data-ttu-id="cd8e0-244">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-245">**Type**</span></span>         | <span data-ttu-id="cd8e0-246">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-246">Yes</span></span>         | <span data-ttu-id="cd8e0-247">Le nom qualifié complet du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="cd8e0-248">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-248">**Role**</span></span>         | <span data-ttu-id="cd8e0-249">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-249">No</span></span>          | <span data-ttu-id="cd8e0-250">La valeur de la **rôle** attribut dans un élément Principal ou du dépendant de l’élément ReferentialConstraint correspondant (si utilisé).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="cd8e0-251">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-251">**Multiplicity**</span></span> | <span data-ttu-id="cd8e0-252">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-252">Yes</span></span>         | <span data-ttu-id="cd8e0-253">**1**, **valeur 0.. 1**, ou **\*** selon le nombre de lignes qui peuvent être à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="cd8e0-254">**1** indique qu’exactement une ligne existe à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="cd8e0-255">**valeur 0.. 1** indique que zéro ou une ligne existe à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="cd8e0-256">**\*** Indique que zéro, un ou plusieurs lignes existent à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-257">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="cd8e0-258">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-259">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="cd8e0-260">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-260">Example</span></span>

<span data-ttu-id="cd8e0-261">L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="cd8e0-262">Le **multiplicité** valeurs spécifiées sur chaque **fin** élément indiquent que plusieurs lignes dans le **commandes** table peut être associée à une ligne dans le **clients**  table, mais qu’une seule ligne dans le **clients** table peut être associée à une ligne dans le **commandes** table.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="cd8e0-263">En outre, le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **clients** table est supprimée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="cd8e0-264">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="cd8e0-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="cd8e0-265">Le **fin** élément (en tant qu’enfant de le **AssociationSet** élément) spécifie une table à une extrémité d’une contrainte de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="cd8e0-266">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-267">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-268">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-269">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-269">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-270">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="cd8e0-271">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-271">Attribute Name</span></span> | <span data-ttu-id="cd8e0-272">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-272">Is Required</span></span> | <span data-ttu-id="cd8e0-273">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-274">**EntitySet**</span></span>  | <span data-ttu-id="cd8e0-275">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-275">Yes</span></span>         | <span data-ttu-id="cd8e0-276">Le nom du jeu d’entités SSDL qui est à la fin de la contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="cd8e0-277">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-277">**Role**</span></span>       | <span data-ttu-id="cd8e0-278">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-278">No</span></span>          | <span data-ttu-id="cd8e0-279">La valeur de l’un de le **rôle** spécifiés pour l’un attributs **fin** élément de l’élément Association correspondant.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-280">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="cd8e0-281">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-282">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="cd8e0-283">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-283">Example</span></span>

<span data-ttu-id="cd8e0-284">L’exemple suivant montre un **EntityContainer** élément avec un **AssociationSet** élément avec deux **fin** éléments :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="cd8e0-285">EntityContainer, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-286">Un **EntityContainer** élément dans le langage de définition de schéma de magasin (SSDL) décrit la structure de la source de données sous-jacente dans une application Entity Framework : jeux d’entités SSDL (définis dans les éléments EntitySet) représente des tables dans une base de données, les types d’entité SSDL (définis dans les éléments EntityType) représentent des lignes dans une table, et ensembles d’associations (définis dans les éléments de l’AssociationSet) représentent des contraintes de clé étrangère dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="cd8e0-287">Un conteneur d’entités de stockage modèle mappe à un conteneur d’entités de modèle conceptuel via l’élément EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="cd8e0-288">Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="cd8e0-289">Si un **Documentation** élément est présent, il doit précéder tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="cd8e0-290">Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-291">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="cd8e0-291">EntitySet</span></span>
-   <span data-ttu-id="cd8e0-292">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="cd8e0-292">AssociationSet</span></span>
-   <span data-ttu-id="cd8e0-293">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-294">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-294">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-295">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="cd8e0-296">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-296">Attribute Name</span></span> | <span data-ttu-id="cd8e0-297">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-297">Is Required</span></span> | <span data-ttu-id="cd8e0-298">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-299">**Name**</span></span>       | <span data-ttu-id="cd8e0-300">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-300">Yes</span></span>         | <span data-ttu-id="cd8e0-301">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-301">The name of the entity container.</span></span> <span data-ttu-id="cd8e0-302">Ce nom ne peut pas contenir de point (.).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-303">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="cd8e0-304">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-305">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-306">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-306">Example</span></span>

<span data-ttu-id="cd8e0-307">L’exemple suivant montre un **EntityContainer** élément qui définit deux jeux d’entités et un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="cd8e0-308">Notez que les noms de type d'entité et de type d'association sont qualifiés par le nom de l'espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="cd8e0-309">EntitySet, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-310">Un **EntitySet** élément de langage de définition de schéma de magasin (SSDL) représente une table ou vue dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="cd8e0-311">Un élément EntityType en langage SSDL représente une ligne dans la table ou vue.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="cd8e0-312">Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="cd8e0-313">Le mappage entre un jeu d’entités CSDL et un jeu d’entités SSDL est spécifié dans un élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="cd8e0-314">Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-315">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-316">DefiningQuery (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-317">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-318">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-318">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-319">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e0-320">Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="cd8e0-321">Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="cd8e0-322">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-322">Attribute Name</span></span> | <span data-ttu-id="cd8e0-323">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-323">Is Required</span></span> | <span data-ttu-id="cd8e0-324">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-325">**Name**</span></span>       | <span data-ttu-id="cd8e0-326">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-326">Yes</span></span>         | <span data-ttu-id="cd8e0-327">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="cd8e0-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-328">**EntityType**</span></span> | <span data-ttu-id="cd8e0-329">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-329">Yes</span></span>         | <span data-ttu-id="cd8e0-330">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="cd8e0-331">**schéma**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-331">**Schema**</span></span>     | <span data-ttu-id="cd8e0-332">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-332">No</span></span>          | <span data-ttu-id="cd8e0-333">Schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="cd8e0-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-334">**Table**</span></span>      | <span data-ttu-id="cd8e0-335">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-335">No</span></span>          | <span data-ttu-id="cd8e0-336">Table de base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="cd8e0-337">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="cd8e0-338">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-339">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-340">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-340">Example</span></span>

<span data-ttu-id="cd8e0-341">L’exemple suivant montre un **EntityContainer** élément qui a deux **EntitySet** éléments et l’autre **AssociationSet** élément :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="cd8e0-342">EntityType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-343">Un **EntityType** élément de langage de définition de schéma de magasin (SSDL) représente une ligne dans une table ou vue de la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="cd8e0-344">Un élément EntitySet en SSDL représente la table ou vue dans laquelle les lignes résultent.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="cd8e0-345">Le **EntityType** attribut d’un **EntitySet** élément spécifie le type d’entité SSDL particulier qui représente les lignes dans un jeu d’entités SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="cd8e0-346">Le mappage entre un type d’entité SSDL et un type d’entité CSDL est spécifié dans un élément EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="cd8e0-347">Le **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-348">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-349">Clé (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="cd8e0-350">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-351">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-351">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-352">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="cd8e0-353">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-353">Attribute Name</span></span> | <span data-ttu-id="cd8e0-354">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-354">Is Required</span></span> | <span data-ttu-id="cd8e0-355">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-356">**Name**</span></span>       | <span data-ttu-id="cd8e0-357">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-357">Yes</span></span>         | <span data-ttu-id="cd8e0-358">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-358">The name of the entity type.</span></span> <span data-ttu-id="cd8e0-359">Cette valeur est habituellement la même que le nom de la table dans laquelle le type d'entité représente une ligne.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="cd8e0-360">Cette valeur ne peut pas contenir de point (.).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-361">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="cd8e0-362">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-363">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-364">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-364">Example</span></span>

<span data-ttu-id="cd8e0-365">L’exemple suivant montre un **EntityType** élément avec deux propriétés :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="cd8e0-366">Function, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-366">Function Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-367">Le **fonction** élément de langage de définition de schéma de magasin (SSDL) spécifie une procédure stockée qui existe dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="cd8e0-368">Le **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-369">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-370">Paramètre (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="cd8e0-371">CommandText (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-372">ReturnType (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="cd8e0-373">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="cd8e0-374">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="cd8e0-375">Les procédures stockées spécifiées dans le modèle de stockage peuvent être importées dans le modèle conceptuel d'une application.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="cd8e0-376">Pour plus d’informations, consultez [interrogation avec des procédures stockées](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="cd8e0-377">Le **fonction** élément peut également être utilisé pour définir des fonctions personnalisées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-378">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-378">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-379">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e0-380">Certains attributs (non répertoriées ici) peuvent être qualifiés avec le **stocker** alias.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="cd8e0-381">Ces attributs sont utilisés par l’Assistant de modèle de mise à jour lors de la mise à jour d’un modèle.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="cd8e0-382">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-382">Attribute Name</span></span>             | <span data-ttu-id="cd8e0-383">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-383">Is Required</span></span> | <span data-ttu-id="cd8e0-384">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-385">**Name**</span></span>                   | <span data-ttu-id="cd8e0-386">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-386">Yes</span></span>         | <span data-ttu-id="cd8e0-387">Nom de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="cd8e0-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-388">**ReturnType**</span></span>             | <span data-ttu-id="cd8e0-389">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-389">No</span></span>          | <span data-ttu-id="cd8e0-390">Type de retour de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="cd8e0-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-391">**Aggregate**</span></span>              | <span data-ttu-id="cd8e0-392">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-392">No</span></span>          | <span data-ttu-id="cd8e0-393">**True** si la procédure stockée retourne une valeur d’agrégation ; sinon **False**.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="cd8e0-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-394">**BuiltIn**</span></span>                | <span data-ttu-id="cd8e0-395">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-395">No</span></span>          | <span data-ttu-id="cd8e0-396">**True** si la fonction est intégré<sup>1</sup> fonction ; sinon **False**.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="cd8e0-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="cd8e0-398">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-398">No</span></span>          | <span data-ttu-id="cd8e0-399">Nom de la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="cd8e0-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-400">**NiladicFunction**</span></span>        | <span data-ttu-id="cd8e0-401">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-401">No</span></span>          | <span data-ttu-id="cd8e0-402">**True** si la fonction est un niladiques<sup>2</sup> fonction ; **False** dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="cd8e0-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-403">**IsComposable**</span></span>           | <span data-ttu-id="cd8e0-404">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-404">No</span></span>          | <span data-ttu-id="cd8e0-405">**True** si la fonction est un élément composable<sup>3</sup> fonction ; **False** dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="cd8e0-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="cd8e0-407">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-407">No</span></span>          | <span data-ttu-id="cd8e0-408">Énumération qui définit la sémantique de type utilisée pour résoudre les surcharges de fonction.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="cd8e0-409">L'énumération est définie dans le manifeste du fournisseur par définition de fonction.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="cd8e0-410">La valeur par défaut est **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="cd8e0-411">**schéma**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-411">**Schema**</span></span>                 | <span data-ttu-id="cd8e0-412">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-412">No</span></span>          | <span data-ttu-id="cd8e0-413">Nom du schéma dans lequel une procédure stockée est définie.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="cd8e0-414"><sup>1</sup> une fonction intégrée est une fonction qui est définie dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="cd8e0-415">Pour plus d’informations sur les fonctions qui sont définis dans le modèle de stockage, consultez l’élément CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="cd8e0-416"><sup>2</sup> une fonction niladique est une fonction qui n’accepte aucun paramètre et, lorsqu’elle est appelée, ne nécessite pas de parenthèses.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="cd8e0-417"><sup>3</sup> deux fonctions sont composables si la sortie d’une fonction peut être l’entrée de l’autre fonction.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e0-418">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="cd8e0-419">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-420">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-421">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-421">Example</span></span>

<span data-ttu-id="cd8e0-422">L’exemple suivant montre un **fonction** élément qui correspond à la **UpdateOrderQuantity** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="cd8e0-423">La procédure stockée accepte deux paramètres et ne retourne pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="cd8e0-424">Key, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-424">Key Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-425">Le **clé** élément de langage de définition de schéma de magasin (SSDL) représente la clé primaire d’une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="cd8e0-426">**Clé** est un élément enfant d’un élément EntityType, qui représente une ligne dans une table.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="cd8e0-427">La clé primaire est définie dans le **clé** élément en faisant référence à un ou plusieurs éléments de propriété sont définis sur le **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="cd8e0-428">Le **clé** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-429">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="cd8e0-430">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-430">Annotation elements</span></span>

<span data-ttu-id="cd8e0-431">Aucun attribut n’est applicable à la **clé** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-432">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-432">Example</span></span>

<span data-ttu-id="cd8e0-433">L’exemple suivant montre un **EntityType** élément avec une clé qui fait référence à une seule propriété :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="cd8e0-434">OnDelete, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-435">Le **OnDelete** élément dans le langage de définition de schéma de magasin (SSDL) reflète le comportement de la base de données lorsqu’une ligne qui participe à une contrainte de clé étrangère est supprimée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="cd8e0-436">Si l’action est définie sur **Cascade**, puis les lignes qui font référence à une ligne qui est en cours de suppression seront également supprimées.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="cd8e0-437">Si l’action est définie sur **aucun**, les lignes qui font référence à une ligne qui est en cours de suppression ne sont pas également supprimées.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="cd8e0-438">Un **OnDelete** élément est un élément enfant d’un élément de fin.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="cd8e0-439">Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-440">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-441">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-442">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-442">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-443">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="cd8e0-444">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-444">Attribute Name</span></span> | <span data-ttu-id="cd8e0-445">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-445">Is Required</span></span> | <span data-ttu-id="cd8e0-446">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-447">**Action**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-447">**Action**</span></span>     | <span data-ttu-id="cd8e0-448">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-448">Yes</span></span>         | <span data-ttu-id="cd8e0-449">**Cascade** ou **aucun**.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-449">**Cascade** or **None**.</span></span> <span data-ttu-id="cd8e0-450">(La valeur **Restricted** est valide mais a le même comportement que **aucun**.)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-451">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="cd8e0-452">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-453">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-454">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-454">Example</span></span>

<span data-ttu-id="cd8e0-455">L’exemple suivant montre un **Association** élément qui définit le **FK\_CustomerOrders** contrainte de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="cd8e0-456">Le **OnDelete** élément indique que toutes les lignes dans le **commandes** table qui font référence à une ligne particulière dans le **clients** table est supprimée si la ligne dans le **Clients** table est supprimée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="cd8e0-457">Élément Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-458">Le **paramètre** élément dans le langage de définition de schéma de magasin (SSDL) est un enfant de l’élément de la fonction qui spécifie les paramètres pour une procédure stockée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="cd8e0-459">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-460">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-461">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-462">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-462">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-463">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="cd8e0-464">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-464">Attribute Name</span></span> | <span data-ttu-id="cd8e0-465">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-465">Is Required</span></span> | <span data-ttu-id="cd8e0-466">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-467">**Name**</span></span>       | <span data-ttu-id="cd8e0-468">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-468">Yes</span></span>         | <span data-ttu-id="cd8e0-469">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="cd8e0-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-470">**Type**</span></span>       | <span data-ttu-id="cd8e0-471">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-471">Yes</span></span>         | <span data-ttu-id="cd8e0-472">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="cd8e0-473">**Mode**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-473">**Mode**</span></span>       | <span data-ttu-id="cd8e0-474">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-474">No</span></span>          | <span data-ttu-id="cd8e0-475">**Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="cd8e0-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-476">**MaxLength**</span></span>  | <span data-ttu-id="cd8e0-477">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-477">No</span></span>          | <span data-ttu-id="cd8e0-478">Longueur maximale du paramètre.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="cd8e0-479">**Précision**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-479">**Precision**</span></span>  | <span data-ttu-id="cd8e0-480">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-480">No</span></span>          | <span data-ttu-id="cd8e0-481">Précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="cd8e0-482">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-482">**Scale**</span></span>      | <span data-ttu-id="cd8e0-483">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-483">No</span></span>          | <span data-ttu-id="cd8e0-484">Échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="cd8e0-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-485">**SRID**</span></span>       | <span data-ttu-id="cd8e0-486">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-486">No</span></span>          | <span data-ttu-id="cd8e0-487">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cd8e0-488">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="cd8e0-489">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-490">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="cd8e0-491">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-492">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-493">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-493">Example</span></span>

<span data-ttu-id="cd8e0-494">L’exemple suivant montre un **fonction** élément qui a deux **paramètre** éléments qui spécifient les paramètres d’entrée :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="cd8e0-495">Élément Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-496">Le **Principal** dans le langage de définition de schéma de magasin (SSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte de clé étrangère (également appelée une contrainte référentielle).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="cd8e0-497">Le **Principal** élément spécifie la colonne de clé primaire (ou les colonnes) dans une table qui est référencée par un autre (ou les colonnes).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="cd8e0-498">**PropertyRef** éléments spécifient quelles colonnes sont référencées.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="cd8e0-499">L’élément dépendant spécifie les colonnes qui font référence les colonnes de clés primaires sont spécifiés dans le **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="cd8e0-500">Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="cd8e0-501">PropertyRef (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="cd8e0-502">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-503">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-503">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-504">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="cd8e0-505">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-505">Attribute Name</span></span> | <span data-ttu-id="cd8e0-506">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-506">Is Required</span></span> | <span data-ttu-id="cd8e0-507">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-508">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-508">**Role**</span></span>       | <span data-ttu-id="cd8e0-509">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-509">Yes</span></span>         | <span data-ttu-id="cd8e0-510">La même valeur que la **rôle** attribut (le cas échéant) de l’élément de fin correspondant ; sinon, le nom de la table qui contient la colonne référencée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-511">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="cd8e0-512">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-513">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-514">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-514">Example</span></span>

<span data-ttu-id="cd8e0-515">L’exemple suivant montre un élément d’Association qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders** clé étrangère contrainte.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="cd8e0-516">Le **Principal** élément spécifie la **CustomerId** colonne de la **client** table en tant que la terminaison principale de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="cd8e0-517">Property, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-517">Property Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-518">Le **propriété** élément de langage de définition de schéma de magasin (SSDL) représente une colonne dans une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="cd8e0-519">**Propriété** éléments sont des enfants d’éléments EntityType, qui représentent des lignes dans une table.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="cd8e0-520">Chaque **propriété** élément défini sur une **EntityType** élément représente une colonne.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="cd8e0-521">Un **propriété** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-522">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-522">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-523">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="cd8e0-524">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-524">Attribute Name</span></span>            | <span data-ttu-id="cd8e0-525">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-525">Is Required</span></span> | <span data-ttu-id="cd8e0-526">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-527">**Name**</span></span>                  | <span data-ttu-id="cd8e0-528">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-528">Yes</span></span>         | <span data-ttu-id="cd8e0-529">Nom de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="cd8e0-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-530">**Type**</span></span>                  | <span data-ttu-id="cd8e0-531">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-531">Yes</span></span>         | <span data-ttu-id="cd8e0-532">Type de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="cd8e0-533">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-533">**Nullable**</span></span>              | <span data-ttu-id="cd8e0-534">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-534">No</span></span>          | <span data-ttu-id="cd8e0-535">**True** (la valeur par défaut) ou **False** selon si la colonne correspondante peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="cd8e0-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-536">**DefaultValue**</span></span>          | <span data-ttu-id="cd8e0-537">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-537">No</span></span>          | <span data-ttu-id="cd8e0-538">Valeur par défaut de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="cd8e0-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-539">**MaxLength**</span></span>             | <span data-ttu-id="cd8e0-540">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-540">No</span></span>          | <span data-ttu-id="cd8e0-541">Longueur maximale de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="cd8e0-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-542">**FixedLength**</span></span>           | <span data-ttu-id="cd8e0-543">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-543">No</span></span>          | <span data-ttu-id="cd8e0-544">**True** ou **False** selon si la valeur de colonne correspondante sera être stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="cd8e0-545">**Précision**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-545">**Precision**</span></span>             | <span data-ttu-id="cd8e0-546">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-546">No</span></span>          | <span data-ttu-id="cd8e0-547">Précision de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="cd8e0-548">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-548">**Scale**</span></span>                 | <span data-ttu-id="cd8e0-549">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-549">No</span></span>          | <span data-ttu-id="cd8e0-550">Échelle de la colonne correspondante.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="cd8e0-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-551">**Unicode**</span></span>               | <span data-ttu-id="cd8e0-552">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-552">No</span></span>          | <span data-ttu-id="cd8e0-553">**True** ou **False** selon si la valeur de colonne correspondante sera être stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="cd8e0-554">**classement**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-554">**Collation**</span></span>             | <span data-ttu-id="cd8e0-555">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-555">No</span></span>          | <span data-ttu-id="cd8e0-556">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="cd8e0-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-557">**SRID**</span></span>                  | <span data-ttu-id="cd8e0-558">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-558">No</span></span>          | <span data-ttu-id="cd8e0-559">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="cd8e0-560">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="cd8e0-561">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="cd8e0-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="cd8e0-563">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-563">No</span></span>          | <span data-ttu-id="cd8e0-564">**Aucun**, **identité** (si la valeur de colonne correspondante est une identité qui est générée dans la base de données), ou **calculé** (si la valeur de colonne correspondante est calculée dans la base de données).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="cd8e0-565">Non valide pour les propriétés de RowType.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-566">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="cd8e0-567">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-568">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-569">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-569">Example</span></span>

<span data-ttu-id="cd8e0-570">L’exemple suivant montre un **EntityType** élément avec deux enfants **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="cd8e0-571">Élément PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-572">Le **PropertyRef** élément dans le langage de définition de schéma de magasin (SSDL) fait référence à une propriété définie sur un élément EntityType pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="cd8e0-573">Faire partie de la clé primaire de la table qui la **EntityType** représente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="cd8e0-574">Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="cd8e0-575">Pour plus d’informations, consultez l’élément Key.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="cd8e0-576">Faire partie de la terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="cd8e0-577">Pour plus d’informations, consultez ReferentialConstraint (élément).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="cd8e0-578">Le **PropertyRef** élément peut avoir uniquement les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="cd8e0-579">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-580">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-581">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-581">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-582">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="cd8e0-583">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-583">Attribute Name</span></span> | <span data-ttu-id="cd8e0-584">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-584">Is Required</span></span> | <span data-ttu-id="cd8e0-585">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="cd8e0-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-586">**Name**</span></span>       | <span data-ttu-id="cd8e0-587">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-587">Yes</span></span>         | <span data-ttu-id="cd8e0-588">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="cd8e0-589">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="cd8e0-590">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="cd8e0-591">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-592">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-592">Example</span></span>

<span data-ttu-id="cd8e0-593">L’exemple suivant montre un **PropertyRef** élément utilisé pour définir une clé primaire en référençant une propriété qui est définie sur une **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="cd8e0-594">ReferentialConstraint, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-595">Le **ReferentialConstraint** élément de langage de définition de schéma de magasin (SSDL) représente une contrainte de clé étrangère (également appelée une contrainte d’intégrité référentielle) dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="cd8e0-596">Les terminaisons principales et dépendantes de la contrainte sont spécifiées par les éléments enfants principale et dépendante, respectivement.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="cd8e0-597">Les colonnes qui participent les terminaisons principale et dépendantes sont référencées avec les éléments PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="cd8e0-598">Le **ReferentialConstraint** élément est un élément enfant facultatif de l’élément Association.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="cd8e0-599">Si un **ReferentialConstraint** élément n’est pas utilisé pour mapper la contrainte de clé étrangère est spécifiée dans le **Association** élément AssociationSetMapping élément doit être utilisé pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="cd8e0-600">Le **ReferentialConstraint** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="cd8e0-601">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="cd8e0-602">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="cd8e0-603">Dépendante (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="cd8e0-604">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-605">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-605">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-606">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="cd8e0-607">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-608">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-609">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-609">Example</span></span>

<span data-ttu-id="cd8e0-610">L’exemple suivant montre un **Association** élément qui utilise un **ReferentialConstraint** élément pour spécifier les colonnes qui participent à la **FK\_CustomerOrders**  contrainte de clé étrangère :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="cd8e0-611">ReturnType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-612">Le **ReturnType** élément dans le langage de définition de schéma de magasin (SSDL) spécifie le type de retour pour une fonction qui est défini dans un **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="cd8e0-613">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="cd8e0-614">Le type de retour d’une fonction est spécifié avec la **Type** attribut ou le **ReturnType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="cd8e0-615">Le **ReturnType** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="cd8e0-616">CollectionType (un)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="cd8e0-617">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="cd8e0-618">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-619">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-620">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-620">Example</span></span>

<span data-ttu-id="cd8e0-621">L’exemple suivant utilise un **fonction** qui retourne une collection de lignes.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="cd8e0-622">RowType, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-623">Un **RowType** élément dans le langage de définition de schéma de magasin (SSDL) définit une structure sans nom comme un retour de type pour une fonction définie dans le magasin.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="cd8e0-624">Un **RowType** élément est l’élément enfant de **CollectionType** élément :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="cd8e0-625">Un **RowType** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="cd8e0-626">Propriété (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="cd8e0-627">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-627">Example</span></span>

<span data-ttu-id="cd8e0-628">L’exemple suivant montre une fonction de magasin qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="cd8e0-629">Schema, élément (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="cd8e0-630">Le **schéma** élément dans le langage de définition de schéma de magasin (SSDL) est l’élément racine d’une définition de modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="cd8e0-631">Il contient des définitions pour les objets, les fonctions et les conteneurs qui composent un modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="cd8e0-632">Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="cd8e0-633">Association</span><span class="sxs-lookup"><span data-stu-id="cd8e0-633">Association</span></span>
-   <span data-ttu-id="cd8e0-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="cd8e0-634">EntityType</span></span>
-   <span data-ttu-id="cd8e0-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="cd8e0-635">EntityContainer</span></span>
-   <span data-ttu-id="cd8e0-636">Fonction</span><span class="sxs-lookup"><span data-stu-id="cd8e0-636">Function</span></span>

<span data-ttu-id="cd8e0-637">Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour les objets de type et l’association d’entité dans un modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="cd8e0-638">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="cd8e0-639">Un espace de noms de modèle de stockage est différent de l’espace de noms XML de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="cd8e0-640">Un espace de noms de modèle de stockage (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités et types d’associations.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="cd8e0-641">L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="cd8e0-642">Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="cd8e0-643">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="cd8e0-644">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="cd8e0-644">Applicable Attributes</span></span>

<span data-ttu-id="cd8e0-645">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="cd8e0-646">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="cd8e0-646">Attribute Name</span></span>            | <span data-ttu-id="cd8e0-647">Requis</span><span class="sxs-lookup"><span data-stu-id="cd8e0-647">Is Required</span></span> | <span data-ttu-id="cd8e0-648">Value</span><span class="sxs-lookup"><span data-stu-id="cd8e0-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-649">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-649">**Namespace**</span></span>             | <span data-ttu-id="cd8e0-650">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-650">Yes</span></span>         | <span data-ttu-id="cd8e0-651">Espace de noms du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-651">The namespace of the storage model.</span></span> <span data-ttu-id="cd8e0-652">La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="cd8e0-653">Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms ExampleModel.Store, le nom qualifié complet de le **EntityType** est ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="cd8e0-654">Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="cd8e0-655">La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma CSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="cd8e0-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-656">**Alias**</span></span>                 | <span data-ttu-id="cd8e0-657">Non</span><span class="sxs-lookup"><span data-stu-id="cd8e0-657">No</span></span>          | <span data-ttu-id="cd8e0-658">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="cd8e0-659">Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms ExampleModel.Store et la valeur de la **Alias** attribut est *StorageModel*, vous pouvez utiliser Storagemodel.client comme nom qualifié complet de le **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="cd8e0-660">**Fournisseur**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-660">**Provider**</span></span>              | <span data-ttu-id="cd8e0-661">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-661">Yes</span></span>         | <span data-ttu-id="cd8e0-662">Fournisseur de données.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="cd8e0-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="cd8e0-664">Oui</span><span class="sxs-lookup"><span data-stu-id="cd8e0-664">Yes</span></span>         | <span data-ttu-id="cd8e0-665">Jeton qui indique au fournisseur quel manifeste de fournisseur retourner.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="cd8e0-666">Aucun format n'est défini pour le jeton.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-666">No format for the token is defined.</span></span> <span data-ttu-id="cd8e0-667">Les valeurs du jeton sont définies par le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="cd8e0-668">Pour plus d’informations sur les jetons du manifeste du fournisseur SQL Server, consultez SqlClient pour Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="cd8e0-669">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-669">Example</span></span>

<span data-ttu-id="cd8e0-670">L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="cd8e0-671">Attributs d'annotation</span><span class="sxs-lookup"><span data-stu-id="cd8e0-671">Annotation Attributes</span></span>

<span data-ttu-id="cd8e0-672">Les attributs d'annotation en SSDL (Store Schema Definition Language) sont des attributs XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant les éléments dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="cd8e0-673">En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux attributs d'annotation :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="cd8e0-674">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="cd8e0-675">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="cd8e0-676">Plusieurs attributs d'annotation peuvent être appliqués à un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="cd8e0-677">Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-678">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-678">Example</span></span>

<span data-ttu-id="cd8e0-679">L’exemple suivant montre un élément EntityType qui a un attribut d’annotation appliqué à la **OrderId** propriété.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="cd8e0-680">L’exemple montre également un élément d’annotation ajouté à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="cd8e0-681">Éléments d'annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="cd8e0-682">Les éléments d'annotation en SSDL (Store Schema Definition Language) sont des éléments XML personnalisés dans le modèle de stockage qui fournissent des métadonnées supplémentaires concernant le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="cd8e0-683">En plus d'avoir une structure XML valide, les contraintes suivantes s'appliquent aux éléments d'annotation :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="cd8e0-684">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé au langage SSDL.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="cd8e0-685">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="cd8e0-686">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="cd8e0-687">Plusieurs éléments d'annotation peuvent être des enfants d'un élément SSDL donné.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="cd8e0-688">À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="cd8e0-689">Exemple</span><span class="sxs-lookup"><span data-stu-id="cd8e0-689">Example</span></span>

<span data-ttu-id="cd8e0-690">L’exemple suivant montre un élément EntityType qui a un élément d’annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="cd8e0-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="cd8e0-691">L’exemple montre également un attribut d’annotation appliqué à la **OrderId** propriété.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="cd8e0-692">Facettes (SSDL)</span><span class="sxs-lookup"><span data-stu-id="cd8e0-692">Facets (SSDL)</span></span>

<span data-ttu-id="cd8e0-693">Les facettes dans le langage de définition de schéma de magasin (SSDL) représentent des contraintes sur les types de colonnes qui sont spécifiés dans les éléments de propriété.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="cd8e0-694">Les facettes sont implémentées comme des attributs XML sur **propriété** éléments.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="cd8e0-695">Le tableau ci-dessous décrit les facettes prises en charge dans le langage SSDL :</span><span class="sxs-lookup"><span data-stu-id="cd8e0-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="cd8e0-696">Facette</span><span class="sxs-lookup"><span data-stu-id="cd8e0-696">Facet</span></span>           | <span data-ttu-id="cd8e0-697">Description</span><span class="sxs-lookup"><span data-stu-id="cd8e0-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cd8e0-698">**classement**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-698">**Collation**</span></span>   | <span data-ttu-id="cd8e0-699">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="cd8e0-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-700">**FixedLength**</span></span> | <span data-ttu-id="cd8e0-701">Spécifie si la longueur de la valeur de colonne peut varier.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="cd8e0-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-702">**MaxLength**</span></span>   | <span data-ttu-id="cd8e0-703">Spécifie la longueur maximale de la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="cd8e0-704">**Précision**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-704">**Precision**</span></span>   | <span data-ttu-id="cd8e0-705">Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="cd8e0-706">Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="cd8e0-707">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-707">**Scale**</span></span>       | <span data-ttu-id="cd8e0-708">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="cd8e0-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="cd8e0-709">**Unicode**</span></span>     | <span data-ttu-id="cd8e0-710">Indique si la valeur de colonne est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="cd8e0-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
