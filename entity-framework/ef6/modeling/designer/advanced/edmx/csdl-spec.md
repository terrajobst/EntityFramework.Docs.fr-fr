---
title: Spécification CSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418776"
---
# <a name="csdl-specification"></a><span data-ttu-id="6fc08-102">Spécification CSDL</span><span class="sxs-lookup"><span data-stu-id="6fc08-102">CSDL Specification</span></span>
<span data-ttu-id="6fc08-103">Le langage CSDL (Conceptual Schema Definition Language) est un langage basé sur XML qui décrit les entités, relations et fonctions qui composent un modèle conceptuel d'une application pilotée par les données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="6fc08-104">Ce modèle conceptuel peut être utilisé par le Entity Framework ou WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="6fc08-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="6fc08-105">Les métadonnées qui sont décrites avec le langage CSDL sont utilisées par le Entity Framework pour mapper les entités et les relations définies dans un modèle conceptuel à une source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="6fc08-106">Pour plus d’informations, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) et [spécification MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="6fc08-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="6fc08-107">Le langage CSDL est l’implémentation de la Entity Framework du Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="6fc08-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="6fc08-108">Dans une application Entity Framework, les métadonnées de modèle conceptuel sont chargées à partir d’un fichier. CSDL (écrit en CSDL) dans une instance de System. Data. Metadata. Edm. EdmItemCollection et sont accessibles à l’aide des méthodes de la Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="6fc08-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="6fc08-109">Entity Framework utilise les métadonnées du modèle conceptuel pour traduire les requêtes sur le modèle conceptuel en commandes spécifiques à la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="6fc08-110">Le concepteur EF stocke les informations de modèle conceptuel dans un fichier. edmx au moment de la conception.</span><span class="sxs-lookup"><span data-stu-id="6fc08-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="6fc08-111">Au moment de la génération, le concepteur EF utilise les informations d’un fichier. edmx pour créer le fichier. csdl requis par Entity Framework au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="6fc08-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="6fc08-112">Les versions de CSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="6fc08-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="6fc08-113">Version CSDL</span><span class="sxs-lookup"><span data-stu-id="6fc08-113">CSDL Version</span></span> | <span data-ttu-id="6fc08-114">Espace de noms XML</span><span class="sxs-lookup"><span data-stu-id="6fc08-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="6fc08-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="6fc08-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="6fc08-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="6fc08-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="6fc08-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="6fc08-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="6fc08-118">Élément Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-118">Association Element (CSDL)</span></span>

<span data-ttu-id="6fc08-119">Un élément **Association** définit une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="6fc08-120">Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ».</span><span class="sxs-lookup"><span data-stu-id="6fc08-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="6fc08-121">La multiplicité d’une terminaison d’association peut avoir la valeur un (1), zéro ou un (0.. 1), ou plusieurs (\*).</span><span class="sxs-lookup"><span data-stu-id="6fc08-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="6fc08-122">Ces informations sont spécifiées dans deux éléments End enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="6fc08-123">Il est possible d'accéder aux instances de type d'entité au niveau d'une terminaison d'une association via les propriétés de navigation ou les clés étrangères, si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="6fc08-124">Dans une application, une instance d'une association représente une association spécifique entre des instances de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="6fc08-125">Les instances d'association sont regroupées logiquement dans un ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="6fc08-126">Un élément **Association** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-127">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-128">End (exactement 2 éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="6fc08-129">ReferentialConstraint (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-130">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-131">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-131">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-132">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="6fc08-133">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-133">Attribute Name</span></span> | <span data-ttu-id="6fc08-134">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-134">Is Required</span></span> | <span data-ttu-id="6fc08-135">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="6fc08-136">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-136">**Name**</span></span>       | <span data-ttu-id="6fc08-137">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-137">Yes</span></span>         | <span data-ttu-id="6fc08-138">Nom de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-139">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6fc08-140">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-141">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-142">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-142">Example</span></span>

<span data-ttu-id="6fc08-143">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** lorsque les clés étrangères n’ont pas été exposées sur les types d’entités **Customer** et **Order** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6fc08-144">Les valeurs de **multiplicité** pour chaque **terminaison** de l’Association indiquent que de nombreuses **commandes** peuvent être associées à un **client**, mais qu’un seul **client** peut être associé à une **commande**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6fc08-145">En outre, l’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext seront supprimées si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="6fc08-146">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** lorsque des clés étrangères ont été exposées sur les types d’entités **Customer** et **Order** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="6fc08-147">Avec les clés étrangères exposées, la relation entre les entités est gérée à l’aide d’un élément **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="6fc08-148">Un élément AssociationSetMapping correspondant n'est pas nécessaire pour mapper cette association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="6fc08-149">AssociationSet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="6fc08-150">L’élément **AssociationSet** en Conceptual Schema Definition Language (CSDL) est un conteneur logique pour les instances d’association du même type.</span><span class="sxs-lookup"><span data-stu-id="6fc08-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="6fc08-151">Un ensemble d'associations fournit une définition pour le regroupement d'instances d'association afin qu'elles puissent être mappées à une source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="6fc08-152">L’élément **AssociationSet** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-153">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-154">End (exactement deux éléments requis)</span><span class="sxs-lookup"><span data-stu-id="6fc08-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="6fc08-155">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6fc08-156">L’attribut **Association** spécifie le type d’association contenu dans un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="6fc08-157">Les jeux d’entités qui composent les terminaisons d’un ensemble d’associations sont spécifiés avec exactement deux éléments **finaux** enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-158">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-158">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-159">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="6fc08-160">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-160">Attribute Name</span></span>  | <span data-ttu-id="6fc08-161">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-161">Is Required</span></span> | <span data-ttu-id="6fc08-162">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-163">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-163">**Name**</span></span>        | <span data-ttu-id="6fc08-164">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-164">Yes</span></span>         | <span data-ttu-id="6fc08-165">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-165">The name of the entity set.</span></span> <span data-ttu-id="6fc08-166">La valeur de l’attribut **Name** ne peut pas être la même que la valeur de l’attribut **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="6fc08-167">**Association**</span><span class="sxs-lookup"><span data-stu-id="6fc08-167">**Association**</span></span> | <span data-ttu-id="6fc08-168">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-168">Yes</span></span>         | <span data-ttu-id="6fc08-169">Nom qualifié complet de l'association dont l'ensemble d'associations contient des instances.</span><span class="sxs-lookup"><span data-stu-id="6fc08-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="6fc08-170">L'association doit être dans le même espace de noms que l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-171">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="6fc08-172">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-173">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-174">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-174">Example</span></span>

<span data-ttu-id="6fc08-175">L’exemple suivant montre un élément **EntityContainer** avec deux éléments **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="6fc08-176">Élément CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-177">L’élément **CollectionType** en Conceptual Schema Definition Language (CSDL) spécifie qu’un paramètre de fonction ou un type de retour de fonction est une collection.</span><span class="sxs-lookup"><span data-stu-id="6fc08-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="6fc08-178">L’élément **CollectionType** peut être un enfant de l’élément parameter ou de l’élément ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="6fc08-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="6fc08-179">Le type de collection peut être spécifié à l’aide de l’attribut **type** ou de l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="6fc08-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-180">**CollectionType**</span></span>
-   <span data-ttu-id="6fc08-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6fc08-181">ReferenceType</span></span>
-   <span data-ttu-id="6fc08-182">RowType</span><span class="sxs-lookup"><span data-stu-id="6fc08-182">RowType</span></span>
-   <span data-ttu-id="6fc08-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6fc08-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-184">Un modèle ne sera pas validé si le type d’une collection est spécifié avec l’attribut **type** et un élément enfant.</span><span class="sxs-lookup"><span data-stu-id="6fc08-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-185">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-185">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-186">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6fc08-187">Notez que les attributs **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**et **collation** sont uniquement applicables aux collections de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6fc08-188">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-188">Attribute Name</span></span>                                                          | <span data-ttu-id="6fc08-189">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-189">Is Required</span></span> | <span data-ttu-id="6fc08-190">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-191">**Type**</span></span>                                                                | <span data-ttu-id="6fc08-192">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-192">No</span></span>          | <span data-ttu-id="6fc08-193">Type de la collection.</span><span class="sxs-lookup"><span data-stu-id="6fc08-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6fc08-194">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-194">**Nullable**</span></span>                                                            | <span data-ttu-id="6fc08-195">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-195">No</span></span>          | <span data-ttu-id="6fc08-196">**True** (valeur par défaut) ou **False** selon que la propriété peut avoir ou non une valeur null.</span><span class="sxs-lookup"><span data-stu-id="6fc08-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="6fc08-197">> Dans le CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="6fc08-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6fc08-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="6fc08-199">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-199">No</span></span>          | <span data-ttu-id="6fc08-200">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="6fc08-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="6fc08-202">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-202">No</span></span>          | <span data-ttu-id="6fc08-203">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="6fc08-204">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="6fc08-205">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-205">No</span></span>          | <span data-ttu-id="6fc08-206">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="6fc08-207">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-207">**Precision**</span></span>                                                           | <span data-ttu-id="6fc08-208">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-208">No</span></span>          | <span data-ttu-id="6fc08-209">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6fc08-210">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-210">**Scale**</span></span>                                                               | <span data-ttu-id="6fc08-211">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-211">No</span></span>          | <span data-ttu-id="6fc08-212">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-213">**SRID**</span></span>                                                                | <span data-ttu-id="6fc08-214">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-214">No</span></span>          | <span data-ttu-id="6fc08-215">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-216">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="6fc08-217">   Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="6fc08-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="6fc08-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-218">**Unicode**</span></span>                                                             | <span data-ttu-id="6fc08-219">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-219">No</span></span>          | <span data-ttu-id="6fc08-220">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="6fc08-221">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-221">**Collation**</span></span>                                                           | <span data-ttu-id="6fc08-222">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-222">No</span></span>          | <span data-ttu-id="6fc08-223">Chaîne qui spécifie l’ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="6fc08-224">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6fc08-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-227">Example</span></span>

<span data-ttu-id="6fc08-228">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de types d’entité **Person** (comme spécifié avec l’attribut **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="6fc08-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="6fc08-229">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de lignes (comme spécifié dans l’élément **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="6fc08-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="6fc08-230">L’exemple suivant montre une fonction définie par modèle qui utilise l’élément **CollectionType** pour spécifier que la fonction accepte comme paramètre une collection de types d’entités **Department** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="6fc08-231">ComplexType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-232">Un élément **complexType** définit une structure de données composée de propriétés **type EDMSimpleType** ou d’autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="6fc08-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="6fc08-233">  Un type complexe peut être une propriété d’un type d’entité ou d’un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="6fc08-234">Un type complexe est semblable à un type d'entité du fait qu'un type complexe définit des données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="6fc08-235">Toutefois, il existe des différences clés entre les types complexes et les types d'entités :</span><span class="sxs-lookup"><span data-stu-id="6fc08-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="6fc08-236">Les types complexes n'ont pas d'identité (ni de clés), par conséquent, ils ne peuvent pas exister indépendamment.</span><span class="sxs-lookup"><span data-stu-id="6fc08-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="6fc08-237">Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="6fc08-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="6fc08-238">Les types complexes ne peuvent pas participer à des associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="6fc08-239">Aucune terminaison d'association ne peut être un type complexe ; par conséquent, il n'est pas possible de définir des propriétés de navigation pour des types complexes.</span><span class="sxs-lookup"><span data-stu-id="6fc08-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="6fc08-240">Une propriété de type complexe ne peut pas avoir de valeur NULL, bien que les propriétés scalaires d'un type complexe puissent chacune être définie sur NULL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="6fc08-241">Un élément **complexType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-242">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-243">Property (zéro, un ou plusieurs éléments) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6fc08-244">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6fc08-245">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **complexType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="6fc08-246">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="6fc08-247">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-247">Is Required</span></span> | <span data-ttu-id="6fc08-248">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-249">Name</span><span class="sxs-lookup"><span data-stu-id="6fc08-249">Name</span></span>                                                                                                           | <span data-ttu-id="6fc08-250">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-250">Yes</span></span>         | <span data-ttu-id="6fc08-251">Nom du type complexe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-251">The name of the complex type.</span></span> <span data-ttu-id="6fc08-252">Le nom d'un type complexe ne peut pas être identique au nom d'un autre type complexe, d'un type d'entité ou d'une association qui figure dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="6fc08-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="6fc08-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="6fc08-254">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-254">No</span></span>          | <span data-ttu-id="6fc08-255">Nom d'un autre type complexe qui est le type de base du type complexe en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="6fc08-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="6fc08-256">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6fc08-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6fc08-257">L'héritage pour les types complexes n'est pas pris en charge dans cette version.</span><span class="sxs-lookup"><span data-stu-id="6fc08-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="6fc08-258">Résumé</span><span class="sxs-lookup"><span data-stu-id="6fc08-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="6fc08-259">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-259">No</span></span>          | <span data-ttu-id="6fc08-260">**True** ou **false** (valeur par défaut), selon que le type complexe est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="6fc08-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="6fc08-261">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="6fc08-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="6fc08-262">Les types complexes dans cette version ne peuvent pas être des types abstraits.</span><span class="sxs-lookup"><span data-stu-id="6fc08-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6fc08-263">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **complexType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="6fc08-264">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-265">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-266">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-266">Example</span></span>

<span data-ttu-id="6fc08-267">L’exemple suivant montre un type complexe, **Address**, avec les propriétés type EDMSimpleType **StreetAddress**, **City**, **StateOrProvince**, **Country**et **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="6fc08-268">Pour définir l' **adresse** de type complexe (ci-dessus) en tant que propriété d’un type d’entité, vous devez déclarer le type de propriété dans la définition du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="6fc08-269">L’exemple suivant illustre la propriété **Address** en tant que type complexe sur un type d’entité (**éditeur**) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="6fc08-270">DefiningExpression, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="6fc08-271">L’élément **DefiningExpression** en Conceptual Schema Definition Language (CSDL) contient une expression Entity SQL qui définit une fonction dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="6fc08-272">À des fins de validation, un élément **DefiningExpression** peut contenir du contenu arbitraire.</span><span class="sxs-lookup"><span data-stu-id="6fc08-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="6fc08-273">Toutefois, Entity Framework lèvera une exception au moment de l’exécution si un élément **DefiningExpression** ne contient pas d’Entity SQL valide.</span><span class="sxs-lookup"><span data-stu-id="6fc08-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-274">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-274">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-275">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **DefiningExpression** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="6fc08-276">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-277">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-278">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-278">Example</span></span>

<span data-ttu-id="6fc08-279">L’exemple suivant utilise un élément **DefiningExpression** pour définir une fonction qui retourne le nombre d’années écoulées depuis la publication d’un livre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="6fc08-280">Le contenu de l’élément **DefiningExpression** est écrit en Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="6fc08-281">Dependent, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="6fc08-282">L’élément **dépendant** en Conceptual Schema Definition Language (CSDL) est un élément enfant de l’élément ReferentialConstraint et définit la terminaison dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="6fc08-283">Un élément **ReferentialConstraint** définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6fc08-284">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6fc08-285">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6fc08-286">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6fc08-287">Les éléments **PropertyRef** sont utilisés pour spécifier les clés qui référencent la terminaison principale.</span><span class="sxs-lookup"><span data-stu-id="6fc08-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="6fc08-288">L’élément **dépendant** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-289">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6fc08-290">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-291">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-291">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-292">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **dépendant** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="6fc08-293">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-293">Attribute Name</span></span> | <span data-ttu-id="6fc08-294">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-294">Is Required</span></span> | <span data-ttu-id="6fc08-295">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6fc08-296">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-296">**Role**</span></span>       | <span data-ttu-id="6fc08-297">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-297">Yes</span></span>         | <span data-ttu-id="6fc08-298">Nom du type d'entité au niveau de la terminaison dépendante de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-299">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **dépendant** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="6fc08-300">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-301">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-302">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-302">Example</span></span>

<span data-ttu-id="6fc08-303">L’exemple suivant montre un élément **ReferentialConstraint** utilisé dans le cadre de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6fc08-304">La propriété **PublisherId** du type **d’entité Book** compose la terminaison dépendante de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="6fc08-305">Documentation, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="6fc08-306">L’élément **documentation** en Conceptual Schema Definition Language (CSDL) peut être utilisé pour fournir des informations sur un objet défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="6fc08-307">Dans un fichier. edmx, lorsque l’élément **documentation** est un enfant d’un élément qui apparaît en tant qu’objet sur l’aire de conception du concepteur EF (tel qu’une entité, une association ou une propriété), le contenu de l’élément **documentation** s’affiche dans la fenêtre **Propriétés** de Visual Studio pour l’objet.</span><span class="sxs-lookup"><span data-stu-id="6fc08-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="6fc08-308">L’élément **documentation** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-309">**Summary**: brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="6fc08-310">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="6fc08-310">(zero or one element)</span></span>
-   <span data-ttu-id="6fc08-311">**LongDescription**: description complète de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="6fc08-312">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="6fc08-312">(zero or one element)</span></span>
-   <span data-ttu-id="6fc08-313">éléments d'annotation</span><span class="sxs-lookup"><span data-stu-id="6fc08-313">Annotation elements.</span></span> <span data-ttu-id="6fc08-314">(zéro, un ou plusieurs éléments).</span><span class="sxs-lookup"><span data-stu-id="6fc08-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-315">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-315">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-316">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **documentation** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="6fc08-317">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-318">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-319">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-319">Example</span></span>

<span data-ttu-id="6fc08-320">L’exemple suivant montre l’élément de **documentation** en tant qu’élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="6fc08-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="6fc08-321">Si l’extrait de code ci-dessous se trouve dans le contenu CSDL d’un fichier. edmx, le contenu des éléments **Summary** et **LongDescription** apparaît dans la fenêtre **Propriétés** de Visual Studio lorsque vous cliquez sur le type d’entité `Customer`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="6fc08-322">End, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-322">End Element (CSDL)</span></span>

<span data-ttu-id="6fc08-323">L’élément **end** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="6fc08-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="6fc08-324">Dans chaque cas, le rôle de l’élément de **fin** est différent et les attributs applicables sont différents.</span><span class="sxs-lookup"><span data-stu-id="6fc08-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="6fc08-325">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="6fc08-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="6fc08-326">Un élément **end** (en tant qu’enfant de l’élément **Association** ) identifie le type d’entité à une terminaison d’une association et le nombre d’instances de type d’entité qui peuvent exister à cette terminaison d’une association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="6fc08-327">Les terminaisons d'association sont définies dans le cadre d'une association ; une association doit avoir exactement deux terminaisons d'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="6fc08-328">Il est possible d'accéder aux instances de type d'entité au niveau de la terminaison d'une association via les propriétés de navigation ou les clés étrangères si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="6fc08-329">Un élément **end** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-330">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-331">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-332">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-333">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-333">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-334">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **final** lorsqu’il est l’enfant d’un élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="6fc08-335">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-335">Attribute Name</span></span>   | <span data-ttu-id="6fc08-336">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-336">Is Required</span></span> | <span data-ttu-id="6fc08-337">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-338">**Type**</span></span>         | <span data-ttu-id="6fc08-339">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-339">Yes</span></span>         | <span data-ttu-id="6fc08-340">Nom du type d'entité au niveau de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="6fc08-341">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-341">**Role**</span></span>         | <span data-ttu-id="6fc08-342">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-342">No</span></span>          | <span data-ttu-id="6fc08-343">Nom de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-343">A name for the association end.</span></span> <span data-ttu-id="6fc08-344">Si aucun nom n'est fourni, le nom du type d'entité au niveau de la terminaison de l'association sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="6fc08-345">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="6fc08-345">**Multiplicity**</span></span> | <span data-ttu-id="6fc08-346">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-346">Yes</span></span>         | <span data-ttu-id="6fc08-347">**1**, **0.. 1**ou **\*** selon le nombre d’instances de type d’entité qui peuvent être à la fin de l’Association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="6fc08-348">**1** indique qu’il existe exactement une instance de type d’entité au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="6fc08-349">**0.. 1** indique que zéro ou une instance de type d’entité existe au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="6fc08-350">**\*** indique qu’il existe zéro, une ou plusieurs instances de type d’entité au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-351">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fin** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6fc08-352">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-353">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-354">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-354">Example</span></span>

<span data-ttu-id="6fc08-355">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6fc08-356">Les valeurs de **multiplicité** pour chaque **terminaison** de l’Association indiquent que de nombreuses **commandes** peuvent être associées à un **client**, mais qu’un seul **client** peut être associé à une **commande**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="6fc08-357">En outre, l’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext sont supprimées si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="6fc08-358">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="6fc08-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="6fc08-359">L’élément **end** spécifie une extrémité d’un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="6fc08-360">L’élément **AssociationSet** doit contenir deux éléments **end** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="6fc08-361">Les informations contenues dans un élément **end** sont utilisées dans le mappage d’un ensemble d’associations à une source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="6fc08-362">Un élément **end** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-363">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-364">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-365">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6fc08-366">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6fc08-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-367">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-367">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-368">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **end** lorsqu’il est l’enfant d’un élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="6fc08-369">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-369">Attribute Name</span></span> | <span data-ttu-id="6fc08-370">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-370">Is Required</span></span> | <span data-ttu-id="6fc08-371">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6fc08-372">**EntitySet**</span></span>  | <span data-ttu-id="6fc08-373">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-373">Yes</span></span>         | <span data-ttu-id="6fc08-374">Nom de l’élément **EntitySet** qui définit une extrémité de l’élément **AssociationSet** parent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="6fc08-375">L’élément **EntitySet** doit être défini dans le même conteneur d’entités que l’élément **AssociationSet** parent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="6fc08-376">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-376">**Role**</span></span>       | <span data-ttu-id="6fc08-377">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-377">No</span></span>          | <span data-ttu-id="6fc08-378">Nom de la terminaison de l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-378">The name of the association set end.</span></span> <span data-ttu-id="6fc08-379">Si l’attribut **role** n’est pas utilisé, le nom de la terminaison de l’ensemble d’associations sera le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6fc08-380">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fin** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="6fc08-381">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-382">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-383">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-383">Example</span></span>

<span data-ttu-id="6fc08-384">L’exemple suivant montre un élément **EntityContainer** avec deux éléments **AssociationSet** , chacun avec deux éléments **end** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="6fc08-385">Élément EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="6fc08-386">L’élément **EntityContainer** en Conceptual Schema Definition Language (CSDL) est un conteneur logique pour les jeux d’entités, les ensembles d’associations et les importations de fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="6fc08-387">Un conteneur d'entités de modèle conceptuel est mappé à un conteneur d'entités de modèle de stockage via l'élément EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="6fc08-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="6fc08-388">Un conteneur d'entités de modèle de stockage décrit la structure de la base de données : les jeux d'entités décrivent les tables, les ensembles d'associations décrivent les contraintes de clé étrangère et les importations de fonction décrivent les procédures stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="6fc08-389">Un élément **EntityContainer** peut avoir zéro ou un élément documentation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="6fc08-390">Si un élément de **documentation** est présent, il doit précéder tous les éléments **EntitySet**, **AssociationSet**et **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="6fc08-391">Un élément **EntityContainer** peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-392">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-392">EntitySet</span></span>
-   <span data-ttu-id="6fc08-393">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-393">AssociationSet</span></span>
-   <span data-ttu-id="6fc08-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="6fc08-394">FunctionImport</span></span>
-   <span data-ttu-id="6fc08-395">Éléments Annotation</span><span class="sxs-lookup"><span data-stu-id="6fc08-395">Annotation elements</span></span>

<span data-ttu-id="6fc08-396">Vous pouvez étendre un élément **EntityContainer** pour inclure le contenu d’un autre **EntityContainer** qui se trouve dans le même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6fc08-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="6fc08-397">Pour inclure le contenu d’un autre **EntityContainer**, dans l’élément **EntityContainer** de référence, définissez la valeur de l’attribut **extends** sur le nom de l’élément **EntityContainer** que vous souhaitez inclure.</span><span class="sxs-lookup"><span data-stu-id="6fc08-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="6fc08-398">Tous les éléments enfants de l’élément **EntityContainer** inclus sont traités comme des éléments enfants de l’élément **EntityContainer** de référence.</span><span class="sxs-lookup"><span data-stu-id="6fc08-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-399">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-399">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-400">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6fc08-401">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-401">Attribute Name</span></span> | <span data-ttu-id="6fc08-402">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-402">Is Required</span></span> | <span data-ttu-id="6fc08-403">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="6fc08-404">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-404">**Name**</span></span>       | <span data-ttu-id="6fc08-405">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-405">Yes</span></span>         | <span data-ttu-id="6fc08-406">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="6fc08-407">**Dure**</span><span class="sxs-lookup"><span data-stu-id="6fc08-407">**Extends**</span></span>    | <span data-ttu-id="6fc08-408">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-408">No</span></span>          | <span data-ttu-id="6fc08-409">Le nom d'un autre conteneur d'entités au sein du même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6fc08-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-410">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="6fc08-411">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-412">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-413">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-413">Example</span></span>

<span data-ttu-id="6fc08-414">L’exemple suivant montre un élément **EntityContainer** qui définit trois jeux d’entités et deux ensembles d’associations.</span><span class="sxs-lookup"><span data-stu-id="6fc08-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="6fc08-415">EntitySet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="6fc08-416">L’élément **EntitySet** dans Conceptual Schema Definition Language est un conteneur logique pour les instances d’un type d’entité et les instances de tout type dérivé de ce type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="6fc08-417">La relation entre un type d'entité et un jeu d'entités est analogue à la relation entre une ligne et une table dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="6fc08-418">Comme une ligne, un type d'entité définit un jeu de données connexes et, comme une table, un jeu d'entités contient des instances de cette définition.</span><span class="sxs-lookup"><span data-stu-id="6fc08-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="6fc08-419">Un jeu d'entités fournit une construction pour le regroupement d'instances du type d'entité, afin qu'elles puissent être mappées aux structures de données associées dans une source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="6fc08-420">Plusieurs jeux d'entités peuvent être définis pour un type d'entité particulier.</span><span class="sxs-lookup"><span data-stu-id="6fc08-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-421">Le concepteur EF ne prend pas en charge les modèles conceptuels qui contiennent plusieurs jeux d’entités par type.</span><span class="sxs-lookup"><span data-stu-id="6fc08-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="6fc08-422">L’élément **EntitySet** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-423">Élément documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-424">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-425">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-426">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="6fc08-427">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-427">Attribute Name</span></span> | <span data-ttu-id="6fc08-428">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-428">Is Required</span></span> | <span data-ttu-id="6fc08-429">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-430">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-430">**Name**</span></span>       | <span data-ttu-id="6fc08-431">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-431">Yes</span></span>         | <span data-ttu-id="6fc08-432">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="6fc08-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-433">**EntityType**</span></span> | <span data-ttu-id="6fc08-434">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-434">Yes</span></span>         | <span data-ttu-id="6fc08-435">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="6fc08-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-436">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="6fc08-437">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-438">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-439">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-439">Example</span></span>

<span data-ttu-id="6fc08-440">L’exemple suivant montre un élément **EntityContainer** avec trois éléments **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="6fc08-441">Il est possible de définir des jeux d'entités multiples par type (MEST).</span><span class="sxs-lookup"><span data-stu-id="6fc08-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="6fc08-442">L’exemple suivant définit un conteneur d’entités avec deux jeux d’entités pour le type **d’entité Book** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="6fc08-443">Élément EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-444">L’élément **EntityType** représente la structure d’un concept de niveau supérieur, tel qu’un client ou une commande, dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="6fc08-445">Un type d'entité est un modèle pour des instances de types d'entités dans une application.</span><span class="sxs-lookup"><span data-stu-id="6fc08-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="6fc08-446">Chaque modèle contient les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fc08-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="6fc08-447">Nom unique.</span><span class="sxs-lookup"><span data-stu-id="6fc08-447">A unique name.</span></span> <span data-ttu-id="6fc08-448">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="6fc08-448">(Required.)</span></span>
-   <span data-ttu-id="6fc08-449">Clé d'entité définie par une ou plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="6fc08-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="6fc08-450">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="6fc08-450">(Required.)</span></span>
-   <span data-ttu-id="6fc08-451">Propriétés pour contenir des données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-451">Properties for containing data.</span></span> <span data-ttu-id="6fc08-452">(Facultatif.)</span><span class="sxs-lookup"><span data-stu-id="6fc08-452">(Optional.)</span></span>
-   <span data-ttu-id="6fc08-453">Propriétés de navigation qui permettent de naviguer d'une terminaison d'une association à l'autre terminaison.</span><span class="sxs-lookup"><span data-stu-id="6fc08-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="6fc08-454">(Facultatif.)</span><span class="sxs-lookup"><span data-stu-id="6fc08-454">(Optional.)</span></span>

<span data-ttu-id="6fc08-455">Dans une application, une instance d'un type d'entité représente un objet spécifique (tel qu'un client ou une commande spécifique).</span><span class="sxs-lookup"><span data-stu-id="6fc08-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="6fc08-456">Chaque instance d'un type d'entité doit avoir une clé d'entité unique dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="6fc08-457">Deux instances de type d'entité sont considérées égales seulement si elles sont du même type et si les valeurs de leurs clés d'entité sont identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="6fc08-458">Un élément **EntityType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-459">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-460">Key (zéro ou un élément) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-461">Property (zéro, un ou plusieurs éléments) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="6fc08-462">NavigationProperty (zéro, un ou plusieurs éléments) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="6fc08-463">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-464">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-464">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-465">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="6fc08-466">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="6fc08-467">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-467">Is Required</span></span> | <span data-ttu-id="6fc08-468">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-469">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="6fc08-470">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-470">Yes</span></span>         | <span data-ttu-id="6fc08-471">Le nom du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="6fc08-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="6fc08-473">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-473">No</span></span>          | <span data-ttu-id="6fc08-474">Le nom d’un autre type d’entité qui est le type de base du type d’entité défini.</span><span class="sxs-lookup"><span data-stu-id="6fc08-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="6fc08-475">**Abstraction**</span><span class="sxs-lookup"><span data-stu-id="6fc08-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="6fc08-476">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-476">No</span></span>          | <span data-ttu-id="6fc08-477">**True** ou **false**, selon que le type d’entité est un type abstrait ou non.</span><span class="sxs-lookup"><span data-stu-id="6fc08-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="6fc08-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="6fc08-479">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-479">No</span></span>          | <span data-ttu-id="6fc08-480">**True** ou **false** selon que le type d’entité est un type d’entité ouvert.</span><span class="sxs-lookup"><span data-stu-id="6fc08-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="6fc08-481">> L’attribut **OpenType** s’applique uniquement aux types d’entité définis dans les modèles conceptuels utilisés avec ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="6fc08-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6fc08-482">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="6fc08-483">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-484">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-485">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-485">Example</span></span>

<span data-ttu-id="6fc08-486">L’exemple suivant montre un élément **EntityType** avec trois éléments de **propriété** et deux éléments **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="6fc08-487">EnumType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-488">L’élément **enumType** représente un type énuméré.</span><span class="sxs-lookup"><span data-stu-id="6fc08-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="6fc08-489">Un élément **enumType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-490">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-491">Membre (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="6fc08-492">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-493">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-493">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-494">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **enumType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="6fc08-495">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-495">Attribute Name</span></span>     | <span data-ttu-id="6fc08-496">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-496">Is Required</span></span> | <span data-ttu-id="6fc08-497">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-498">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-498">**Name**</span></span>           | <span data-ttu-id="6fc08-499">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-499">Yes</span></span>         | <span data-ttu-id="6fc08-500">Le nom du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6fc08-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="6fc08-501">**IsFlags**</span></span>        | <span data-ttu-id="6fc08-502">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-502">No</span></span>          | <span data-ttu-id="6fc08-503">**True** ou **false**, selon que le type enum peut être utilisé comme un ensemble d’indicateurs.</span><span class="sxs-lookup"><span data-stu-id="6fc08-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="6fc08-504">La valeur par défaut est **false.**</span><span class="sxs-lookup"><span data-stu-id="6fc08-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="6fc08-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-505">**UnderlyingType**</span></span> | <span data-ttu-id="6fc08-506">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-506">No</span></span>          | <span data-ttu-id="6fc08-507">**Edm. Byte**, **Edm. Int16**, **Edm. Int32**, **Edm. Int64** ou **Edm. SByte** définissant la plage de valeurs du type.</span><span class="sxs-lookup"><span data-stu-id="6fc08-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="6fc08-508">  Le type sous-jacent par défaut des éléments d’énumération est **Edm. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-509">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **enumType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="6fc08-510">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-511">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-512">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-512">Example</span></span>

<span data-ttu-id="6fc08-513">L’exemple suivant montre un élément **enumType** avec trois éléments **membres** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="6fc08-514">Function, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-514">Function Element (CSDL)</span></span>

<span data-ttu-id="6fc08-515">L’élément de **fonction** en Conceptual Schema Definition Language (CSDL) est utilisé pour définir ou déclarer des fonctions dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="6fc08-516">Une fonction est définie à l'aide d'un élément DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="6fc08-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="6fc08-517">Un élément **Function** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-518">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-519">Parameter (zéro, un ou plusieurs éléments) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="6fc08-520">DefiningExpression (zéro ou un élément) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-521">ReturnType (Function) (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-522">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="6fc08-523">Un type de retour pour une fonction doit être spécifié avec l’élément **ReturnType** (Function) ou **ReturnType** (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6fc08-524">Les types de retour possibles correspondent à tout type EdmSimpleType, type d'entité, type complexe, type de ligne ou type REF (ou à une collection de l'un de ces types).</span><span class="sxs-lookup"><span data-stu-id="6fc08-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-525">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-525">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-526">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Function** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="6fc08-527">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-527">Attribute Name</span></span> | <span data-ttu-id="6fc08-528">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-528">Is Required</span></span> | <span data-ttu-id="6fc08-529">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6fc08-530">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-530">**Name**</span></span>       | <span data-ttu-id="6fc08-531">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-531">Yes</span></span>         | <span data-ttu-id="6fc08-532">Nom de la fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-532">The name of the function.</span></span>          |
| <span data-ttu-id="6fc08-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-533">**ReturnType**</span></span> | <span data-ttu-id="6fc08-534">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-534">No</span></span>          | <span data-ttu-id="6fc08-535">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-536">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fonction** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="6fc08-537">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-538">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-539">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-539">Example</span></span>

<span data-ttu-id="6fc08-540">L’exemple suivant utilise un élément **Function** pour définir une fonction qui retourne le nombre d’années écoulées depuis l’embauche d’un formateur.</span><span class="sxs-lookup"><span data-stu-id="6fc08-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="6fc08-541">FunctionImport, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="6fc08-542">L’élément **FunctionImport** en Conceptual Schema Definition Language (CSDL) représente une fonction qui est définie dans la source de données, mais qui est disponible pour les objets via le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="6fc08-543">Par exemple, un élément Function dans le modèle de stockage peut être utilisé pour représenter une procédure stockée dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="6fc08-544">Un élément **FunctionImport** du modèle conceptuel représente la fonction correspondante dans une Entity Framework application et est mappée à la fonction de modèle de stockage à l’aide de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="6fc08-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="6fc08-545">Lorsque la fonction est appelée dans l'application, la procédure stockée correspondante est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="6fc08-546">L’élément **FunctionImport** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-547">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-548">Parameter (zéro, un ou plusieurs éléments autorisés) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6fc08-549">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="6fc08-550">ReturnType (FunctionImport) (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="6fc08-551">Un élément de **paramètre** doit être défini pour chaque paramètre que la fonction accepte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="6fc08-552">Un type de retour pour une fonction doit être spécifié avec l’élément **ReturnType** (FunctionImport) ou **ReturnType** (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="6fc08-553">La valeur du type de retour doit être une collection de type EDMSimpleType, d’EntityType ou de ComplexType.</span><span class="sxs-lookup"><span data-stu-id="6fc08-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-554">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-554">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-555">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6fc08-556">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-556">Attribute Name</span></span>   | <span data-ttu-id="6fc08-557">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-557">Is Required</span></span> | <span data-ttu-id="6fc08-558">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-559">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-559">**Name**</span></span>         | <span data-ttu-id="6fc08-560">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-560">Yes</span></span>         | <span data-ttu-id="6fc08-561">Nom de la fonction importée.</span><span class="sxs-lookup"><span data-stu-id="6fc08-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="6fc08-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-562">**ReturnType**</span></span>   | <span data-ttu-id="6fc08-563">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-563">No</span></span>          | <span data-ttu-id="6fc08-564">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-564">The type that the function returns.</span></span> <span data-ttu-id="6fc08-565">N’utilisez pas cet attribut si la fonction ne renvoie pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="6fc08-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="6fc08-566">Dans le cas contraire, la valeur doit être une collection de ComplexType, EntityType ou type EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="6fc08-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="6fc08-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6fc08-567">**EntitySet**</span></span>    | <span data-ttu-id="6fc08-568">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-568">No</span></span>          | <span data-ttu-id="6fc08-569">Si la fonction retourne une collection de types d’entités, la valeur de l' **EntitySet** doit être le jeu d’entités auquel la collection appartient.</span><span class="sxs-lookup"><span data-stu-id="6fc08-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6fc08-570">Dans le cas contraire, l’attribut **EntitySet** ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="6fc08-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-571">**IsComposable**</span></span> | <span data-ttu-id="6fc08-572">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-572">No</span></span>          | <span data-ttu-id="6fc08-573">Si la valeur est définie sur true, la fonction est composable (fonction table) et peut être utilisée dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="6fc08-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="6fc08-574">  La valeur par défaut est **false**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="6fc08-575">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6fc08-576">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-577">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-578">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-578">Example</span></span>

<span data-ttu-id="6fc08-579">L’exemple suivant montre un élément **FunctionImport** qui accepte un paramètre et retourne une collection de types d’entités :</span><span class="sxs-lookup"><span data-stu-id="6fc08-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="6fc08-580">Key, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-580">Key Element (CSDL)</span></span>

<span data-ttu-id="6fc08-581">L’élément **Key** est un élément enfant de l’élément EntityType et définit une *clé d’entité* (une propriété ou un ensemble de propriétés d’un type d’entité qui détermine l’identité).</span><span class="sxs-lookup"><span data-stu-id="6fc08-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6fc08-582">Les propriétés qui composent une clé d'entité sont choisies au moment du design.</span><span class="sxs-lookup"><span data-stu-id="6fc08-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="6fc08-583">Les valeurs des propriétés de clé d'entité doivent identifier de façon unique une instance de type d'entité dans un jeu d'entités au moment de l'exécution.</span><span class="sxs-lookup"><span data-stu-id="6fc08-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="6fc08-584">Les propriétés qui composent une clé d'entité doivent être choisies pour garantir l'unicité des instances dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="6fc08-585">L’élément **Key** définit une clé d’entité en référençant une ou plusieurs des propriétés d’un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="6fc08-586">L’élément **Key** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="6fc08-587">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6fc08-588">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-589">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-589">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-590">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Key** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="6fc08-591">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-592">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-593">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-593">Example</span></span>

<span data-ttu-id="6fc08-594">L’exemple ci-dessous définit un type **d'** entité nommé Book.</span><span class="sxs-lookup"><span data-stu-id="6fc08-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="6fc08-595">La clé d’entité est définie en référençant la propriété **ISBN** du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="6fc08-596">La propriété **ISBN** est un bon choix pour la clé d’entité, car un numéro ISBN (International Standard Book Number) identifie de manière unique un livre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="6fc08-597">L’exemple suivant montre un type d’entité (**auteur**) qui a une clé d’entité composée de deux propriétés, **nom** et **adresse**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="6fc08-598">L’utilisation du **nom** et de l' **adresse** pour la clé d’entité est un choix raisonnable, car il est peu probable que deux auteurs du même nom vivent à la même adresse.</span><span class="sxs-lookup"><span data-stu-id="6fc08-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="6fc08-599">Toutefois, ce choix pour une clé d'entité ne garantit pas vraiment l'unicité des clés d'entité dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="6fc08-600">L’ajout d’une **propriété, telle**que la réutilisation, qui pourrait être utilisée pour identifier un auteur de manière unique, est recommandé dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="6fc08-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="6fc08-601">Member, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-601">Member Element (CSDL)</span></span>

<span data-ttu-id="6fc08-602">L’élément **member** est un élément enfant de l’élément enumType et définit un membre du type énuméré.</span><span class="sxs-lookup"><span data-stu-id="6fc08-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-603">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-603">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-604">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="6fc08-605">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-605">Attribute Name</span></span> | <span data-ttu-id="6fc08-606">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-606">Is Required</span></span> | <span data-ttu-id="6fc08-607">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-608">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-608">**Name**</span></span>       | <span data-ttu-id="6fc08-609">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-609">Yes</span></span>         | <span data-ttu-id="6fc08-610">Nom du membre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="6fc08-611">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="6fc08-611">**Value**</span></span>      | <span data-ttu-id="6fc08-612">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-612">No</span></span>          | <span data-ttu-id="6fc08-613">Valeur du membre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-613">The value of the member.</span></span> <span data-ttu-id="6fc08-614">Par défaut, le premier membre a la valeur 0, et la valeur de chaque énumérateur successif est incrémentée de 1.</span><span class="sxs-lookup"><span data-stu-id="6fc08-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="6fc08-615">Plusieurs membres ayant les mêmes valeurs peuvent exister.</span><span class="sxs-lookup"><span data-stu-id="6fc08-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-616">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="6fc08-617">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-618">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-619">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-619">Example</span></span>

<span data-ttu-id="6fc08-620">L’exemple suivant montre un élément **enumType** avec trois éléments **membres** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="6fc08-621">Élément NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="6fc08-622">Un élément **NavigationProperty** définit une propriété de navigation, qui fournit une référence à l’autre terminaison d’une association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="6fc08-623">Contrairement aux propriétés définies à l'aide de l'élément Property, les propriétés de navigation ne définissent pas la forme et les caractéristiques des données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="6fc08-624">Elles fournissent un moyen d'explorer une association entre deux types d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="6fc08-625">Notez que les propriétés de navigation sont facultatives sur les deux types d'entités au niveau des terminaisons d'une association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="6fc08-626">Si vous définissez une propriété de navigation sur un type d'entité au niveau de la terminaison d'une association, il n'est pas nécessaire de définir une propriété de navigation sur le type d'entité à l'autre terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="6fc08-627">Le type de données retourné par une propriété de navigation est déterminé par la multiplicité de sa terminaison d'association distante.</span><span class="sxs-lookup"><span data-stu-id="6fc08-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="6fc08-628">Par exemple, supposons qu’une propriété de navigation, **OrdersNavProp**, existe sur un type d’entité **Customer** et navigue vers une association un-à-plusieurs entre **Customer** et **Order**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="6fc08-629">Étant donné que la terminaison d’association distante pour la propriété de navigation a une multiplicité de plusieurs (\*), son type de données est une collection ( **ordre**).</span><span class="sxs-lookup"><span data-stu-id="6fc08-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="6fc08-630">De même, si une propriété de navigation, **CustomerNavProp**, existe sur le type d’entité **Order** , son type de données est **Customer** , car la multiplicité de la terminaison distante est un (1).</span><span class="sxs-lookup"><span data-stu-id="6fc08-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="6fc08-631">Un élément **NavigationProperty** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-632">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-633">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-634">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-634">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-635">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="6fc08-636">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-636">Attribute Name</span></span>   | <span data-ttu-id="6fc08-637">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-637">Is Required</span></span> | <span data-ttu-id="6fc08-638">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-639">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-639">**Name**</span></span>         | <span data-ttu-id="6fc08-640">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-640">Yes</span></span>         | <span data-ttu-id="6fc08-641">Nom de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="6fc08-642">**Relation**</span><span class="sxs-lookup"><span data-stu-id="6fc08-642">**Relationship**</span></span> | <span data-ttu-id="6fc08-643">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-643">Yes</span></span>         | <span data-ttu-id="6fc08-644">Nom d'une association figurant dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="6fc08-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="6fc08-645">**ToRole**</span></span>       | <span data-ttu-id="6fc08-646">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-646">Yes</span></span>         | <span data-ttu-id="6fc08-647">Terminaison de l'association à laquelle la navigation prend fin.</span><span class="sxs-lookup"><span data-stu-id="6fc08-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="6fc08-648">La valeur de l’attribut **ToRole** doit être identique à la valeur de l’un des attributs de **rôle** définis sur l’une des terminaisons d’Association (définie dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6fc08-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="6fc08-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="6fc08-649">**FromRole**</span></span>     | <span data-ttu-id="6fc08-650">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-650">Yes</span></span>         | <span data-ttu-id="6fc08-651">Terminaison de l'association où la navigation commence.</span><span class="sxs-lookup"><span data-stu-id="6fc08-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="6fc08-652">La valeur de l’attribut **FromRole** doit être identique à la valeur de l’un des attributs de **rôle** définis sur l’une des terminaisons d’Association (définie dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="6fc08-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-653">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="6fc08-654">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-655">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-656">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-656">Example</span></span>

<span data-ttu-id="6fc08-657">L’exemple suivant définit un type d’entité (**Book**) avec deux propriétés de navigation (**PublishedBy** et **WrittenBy**) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="6fc08-658">OnDelete, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="6fc08-659">L’élément **OnDelete** en Conceptual Schema Definition Language (CSDL) définit le comportement qui est connecté avec une association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="6fc08-660">Si l’attribut **action** est défini sur **cascade** à une terminaison d’une association, les types d’entités associés à l’autre terminaison de l’Association sont supprimés lorsque le type d’entité de la première terminaison est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="6fc08-661">Si l’association entre deux types d’entités est une relation clé primaire-clé primaire, un objet dépendant chargé est supprimé lorsque l’objet principal de l’autre terminaison de l’Association est supprimé, quelle que soit la spécification de **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="6fc08-662">L’élément **OnDelete** affecte uniquement le comportement au moment de l’exécution d’une application ; elle n’affecte pas le comportement dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="6fc08-663">Le comportement défini dans la source de données doit être le même que le comportement défini dans l'application.</span><span class="sxs-lookup"><span data-stu-id="6fc08-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="6fc08-664">Un élément **OnDelete** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-665">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-666">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-667">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-667">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-668">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="6fc08-669">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-669">Attribute Name</span></span> | <span data-ttu-id="6fc08-670">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-670">Is Required</span></span> | <span data-ttu-id="6fc08-671">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-672">**Action**</span><span class="sxs-lookup"><span data-stu-id="6fc08-672">**Action**</span></span>     | <span data-ttu-id="6fc08-673">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-673">Yes</span></span>         | <span data-ttu-id="6fc08-674">**Cascade** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-674">**Cascade** or **None**.</span></span> <span data-ttu-id="6fc08-675">Si vous disposez d’une **cascade**, les types d’entités dépendants sont supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="6fc08-676">Si **aucune**valeur n’est, les types d’entités dépendants ne sont pas supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-677">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="6fc08-678">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-679">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-680">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-680">Example</span></span>

<span data-ttu-id="6fc08-681">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="6fc08-682">L’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext seront supprimées lors de la suppression du **client** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="6fc08-683">Parameter, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="6fc08-684">L’élément **Parameter** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément FunctionImport ou de l’élément Function.</span><span class="sxs-lookup"><span data-stu-id="6fc08-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="6fc08-685">Application de l'élément FunctionImport</span><span class="sxs-lookup"><span data-stu-id="6fc08-685">FunctionImport Element Application</span></span>

<span data-ttu-id="6fc08-686">Un élément **Parameter** (en tant qu’enfant de l’élément **FunctionImport** ) est utilisé pour définir les paramètres d’entrée et de sortie des importations de fonctions déclarées dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="6fc08-687">L’élément **Parameter** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-688">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-689">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-690">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-690">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-691">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6fc08-692">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-692">Attribute Name</span></span> | <span data-ttu-id="6fc08-693">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-693">Is Required</span></span> | <span data-ttu-id="6fc08-694">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-695">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-695">**Name**</span></span>       | <span data-ttu-id="6fc08-696">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-696">Yes</span></span>         | <span data-ttu-id="6fc08-697">Le nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6fc08-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-698">**Type**</span></span>       | <span data-ttu-id="6fc08-699">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-699">Yes</span></span>         | <span data-ttu-id="6fc08-700">Le type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-700">The parameter type.</span></span> <span data-ttu-id="6fc08-701">La valeur doit être de type **EDMSimpleType** ou de type complexe, dans la portée du modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="6fc08-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-702">**Mode**</span></span>       | <span data-ttu-id="6fc08-703">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-703">No</span></span>          | <span data-ttu-id="6fc08-704">**In**, **out**ou **INOUT** selon que le paramètre est un paramètre d’entrée, de sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="6fc08-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="6fc08-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-705">**MaxLength**</span></span>  | <span data-ttu-id="6fc08-706">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-706">No</span></span>          | <span data-ttu-id="6fc08-707">La longueur maximale autorisée du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="6fc08-708">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-708">**Precision**</span></span>  | <span data-ttu-id="6fc08-709">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-709">No</span></span>          | <span data-ttu-id="6fc08-710">La précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-711">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-711">**Scale**</span></span>      | <span data-ttu-id="6fc08-712">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-712">No</span></span>          | <span data-ttu-id="6fc08-713">L’échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="6fc08-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-714">**SRID**</span></span>       | <span data-ttu-id="6fc08-715">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-715">No</span></span>          | <span data-ttu-id="6fc08-716">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-717">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="6fc08-718">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-719">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6fc08-720">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-721">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-722">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-722">Example</span></span>

<span data-ttu-id="6fc08-723">L’exemple suivant montre un élément **FunctionImport** avec un élément enfant **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="6fc08-724">La fonction accepte un paramètre d'entrée et retourne une collection de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="6fc08-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="6fc08-725">Application de l'élément Function</span><span class="sxs-lookup"><span data-stu-id="6fc08-725">Function Element Application</span></span>

<span data-ttu-id="6fc08-726">Un élément **Parameter** (en tant qu’enfant de l’élément **Function** ) définit des paramètres pour les fonctions qui sont définies ou déclarées dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="6fc08-727">L’élément **Parameter** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-728">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="6fc08-729">CollectionType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="6fc08-730">ReferenceType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="6fc08-731">RowType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-732">Seul l’un des éléments **CollectionType**, **ReferenceType**ou **RowType** peut être un élément enfant d’un élément de **propriété** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="6fc08-733">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-734">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="6fc08-735">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6fc08-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-736">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-736">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-737">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="6fc08-738">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-738">Attribute Name</span></span>   | <span data-ttu-id="6fc08-739">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-739">Is Required</span></span> | <span data-ttu-id="6fc08-740">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-741">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-741">**Name**</span></span>         | <span data-ttu-id="6fc08-742">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-742">Yes</span></span>         | <span data-ttu-id="6fc08-743">Le nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="6fc08-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-744">**Type**</span></span>         | <span data-ttu-id="6fc08-745">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-745">No</span></span>          | <span data-ttu-id="6fc08-746">Le type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="6fc08-746">The parameter type.</span></span> <span data-ttu-id="6fc08-747">Un paramètre peut correspondre à l'un quelconque des types suivants (ou à des collections de ces types) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="6fc08-748">**Type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="6fc08-749">type d'entité</span><span class="sxs-lookup"><span data-stu-id="6fc08-749">entity type</span></span> <br/> <span data-ttu-id="6fc08-750">type complexe</span><span class="sxs-lookup"><span data-stu-id="6fc08-750">complex type</span></span> <br/> <span data-ttu-id="6fc08-751">type de ligne</span><span class="sxs-lookup"><span data-stu-id="6fc08-751">row type</span></span> <br/> <span data-ttu-id="6fc08-752">type référence</span><span class="sxs-lookup"><span data-stu-id="6fc08-752">reference type</span></span>                             |
| <span data-ttu-id="6fc08-753">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-753">**Nullable**</span></span>     | <span data-ttu-id="6fc08-754">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-754">No</span></span>          | <span data-ttu-id="6fc08-755">**True** (valeur par défaut) ou **false** selon que la propriété peut avoir une valeur **null** ou non.</span><span class="sxs-lookup"><span data-stu-id="6fc08-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="6fc08-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6fc08-756">**DefaultValue**</span></span> | <span data-ttu-id="6fc08-757">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-757">No</span></span>          | <span data-ttu-id="6fc08-758">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6fc08-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-759">**MaxLength**</span></span>    | <span data-ttu-id="6fc08-760">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-760">No</span></span>          | <span data-ttu-id="6fc08-761">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-762">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-762">**FixedLength**</span></span>  | <span data-ttu-id="6fc08-763">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-763">No</span></span>          | <span data-ttu-id="6fc08-764">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6fc08-765">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-765">**Precision**</span></span>    | <span data-ttu-id="6fc08-766">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-766">No</span></span>          | <span data-ttu-id="6fc08-767">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6fc08-768">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-768">**Scale**</span></span>        | <span data-ttu-id="6fc08-769">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-769">No</span></span>          | <span data-ttu-id="6fc08-770">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6fc08-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-771">**SRID**</span></span>         | <span data-ttu-id="6fc08-772">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-772">No</span></span>          | <span data-ttu-id="6fc08-773">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-774">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6fc08-775">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6fc08-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-776">**Unicode**</span></span>      | <span data-ttu-id="6fc08-777">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-777">No</span></span>          | <span data-ttu-id="6fc08-778">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6fc08-779">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-779">**Collation**</span></span>    | <span data-ttu-id="6fc08-780">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-780">No</span></span>          | <span data-ttu-id="6fc08-781">Chaîne qui spécifie l’ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6fc08-782">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="6fc08-783">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-784">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-785">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-785">Example</span></span>

<span data-ttu-id="6fc08-786">L’exemple suivant montre un élément **Function** qui utilise un élément enfant **Parameter** pour définir un paramètre de fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="6fc08-787">Principal, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="6fc08-788">L’élément **principal** en Conceptual Schema Definition Language (CSDL) est un élément enfant de l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="6fc08-789">Un élément **ReferentialConstraint** définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6fc08-790">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6fc08-791">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6fc08-792">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="6fc08-793">Les éléments **PropertyRef** sont utilisés pour spécifier les clés qui sont référencées par la terminaison dépendante.</span><span class="sxs-lookup"><span data-stu-id="6fc08-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="6fc08-794">L’élément **principal** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-795">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="6fc08-796">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-797">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-797">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-798">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **principal** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="6fc08-799">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-799">Attribute Name</span></span> | <span data-ttu-id="6fc08-800">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-800">Is Required</span></span> | <span data-ttu-id="6fc08-801">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="6fc08-802">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-802">**Role**</span></span>       | <span data-ttu-id="6fc08-803">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-803">Yes</span></span>         | <span data-ttu-id="6fc08-804">Nom du type d'entité au niveau de la terminaison principale de l'association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-805">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **principal** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="6fc08-806">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-807">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-808">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-808">Example</span></span>

<span data-ttu-id="6fc08-809">L’exemple suivant montre un élément **ReferentialConstraint** qui fait partie de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="6fc08-810">La propriété **ID** du type d’entité du serveur de **publication** compose la terminaison principale de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="6fc08-811">Property, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-811">Property Element (CSDL)</span></span>

<span data-ttu-id="6fc08-812">L’élément de **propriété** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément EntityType, de l’élément complexType ou de l’élément RowType.</span><span class="sxs-lookup"><span data-stu-id="6fc08-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="6fc08-813">Applications des éléments EntityType et ComplexType</span><span class="sxs-lookup"><span data-stu-id="6fc08-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="6fc08-814">Les éléments de **propriété** (en tant qu’enfants des éléments **EntityType** ou **complexType** ) définissent la forme et les caractéristiques des données qu’une instance de type d’entité ou une instance de type complexe contiendra.</span><span class="sxs-lookup"><span data-stu-id="6fc08-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="6fc08-815">Les propriétés dans un modèle conceptuel sont analogues aux propriétés qui sont définies sur une classe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="6fc08-816">De même que les propriétés sur une classe définissent la forme de la classe et acheminent des informations sur les objets, les propriétés dans un modèle conceptuel définissent la forme d'un type d'entité et acheminent des informations sur les instances de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="6fc08-817">L’élément **Property** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-818">Élément documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-819">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="6fc08-820">Les facettes suivantes peuvent être appliquées à un élément **Property** : **Nullable**, **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**, **collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="6fc08-821">Les facettes sont des attributs XML qui fournissent des informations sur la manière dont les valeurs de propriété sont stockées dans la banque de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-822">Les facettes ne peuvent être appliquées qu’à des propriétés de type **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-823">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-823">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-824">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6fc08-825">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-825">Attribute Name</span></span>                                                         | <span data-ttu-id="6fc08-826">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-826">Is Required</span></span> | <span data-ttu-id="6fc08-827">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-828">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-828">**Name**</span></span>                                                               | <span data-ttu-id="6fc08-829">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-829">Yes</span></span>         | <span data-ttu-id="6fc08-830">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-831">**Type**</span></span>                                                               | <span data-ttu-id="6fc08-832">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-832">Yes</span></span>         | <span data-ttu-id="6fc08-833">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-833">The type of the property value.</span></span> <span data-ttu-id="6fc08-834">Le type de la valeur de propriété doit être un type **EDMSimpleType** ou un type complexe (indiqué par un nom qualifié complet) qui se trouve dans la portée du modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="6fc08-835">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-835">**Nullable**</span></span>                                                           | <span data-ttu-id="6fc08-836">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-836">No</span></span>          | <span data-ttu-id="6fc08-837">**True** (valeur par défaut) ou <strong>False</strong> selon que la propriété peut avoir ou non une valeur null.</span><span class="sxs-lookup"><span data-stu-id="6fc08-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="6fc08-838">> Dans le langage CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6fc08-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="6fc08-840">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-840">No</span></span>          | <span data-ttu-id="6fc08-841">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6fc08-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="6fc08-843">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-843">No</span></span>          | <span data-ttu-id="6fc08-844">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-845">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="6fc08-846">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-846">No</span></span>          | <span data-ttu-id="6fc08-847">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6fc08-848">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-848">**Precision**</span></span>                                                          | <span data-ttu-id="6fc08-849">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-849">No</span></span>          | <span data-ttu-id="6fc08-850">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6fc08-851">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-851">**Scale**</span></span>                                                              | <span data-ttu-id="6fc08-852">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-852">No</span></span>          | <span data-ttu-id="6fc08-853">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6fc08-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-854">**SRID**</span></span>                                                               | <span data-ttu-id="6fc08-855">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-855">No</span></span>          | <span data-ttu-id="6fc08-856">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-857">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6fc08-858">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6fc08-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-859">**Unicode**</span></span>                                                            | <span data-ttu-id="6fc08-860">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-860">No</span></span>          | <span data-ttu-id="6fc08-861">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6fc08-862">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-862">**Collation**</span></span>                                                          | <span data-ttu-id="6fc08-863">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-863">No</span></span>          | <span data-ttu-id="6fc08-864">Chaîne qui spécifie l’ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="6fc08-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="6fc08-866">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-866">No</span></span>          | <span data-ttu-id="6fc08-867">**None** (valeur par défaut) ou **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="6fc08-868">Si la valeur est définie sur **Fixed**, la valeur de propriété sera utilisée dans les contrôles d’accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="6fc08-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="6fc08-869">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6fc08-870">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-871">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-872">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-872">Example</span></span>

<span data-ttu-id="6fc08-873">L’exemple suivant montre un élément **EntityType** avec trois éléments **Property** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="6fc08-874">L’exemple suivant montre un élément **complexType** avec cinq éléments de **propriété** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="6fc08-875">Application de l'élément RowType</span><span class="sxs-lookup"><span data-stu-id="6fc08-875">RowType Element Application</span></span>

<span data-ttu-id="6fc08-876">Les éléments de **propriété** (comme les enfants d’un élément **RowType** ) définissent la forme et les caractéristiques des données qui peuvent être passées ou retournées à partir d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="6fc08-877">L’élément **Property** peut avoir exactement l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="6fc08-878">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-878">CollectionType</span></span>
-   <span data-ttu-id="6fc08-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6fc08-879">ReferenceType</span></span>
-   <span data-ttu-id="6fc08-880">RowType</span><span class="sxs-lookup"><span data-stu-id="6fc08-880">RowType</span></span>

<span data-ttu-id="6fc08-881">L’élément **Property** peut avoir n’importe quel nombre d’éléments d’annotation enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-882">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6fc08-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-883">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-883">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-884">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="6fc08-885">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-885">Attribute Name</span></span>                                                     | <span data-ttu-id="6fc08-886">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-886">Is Required</span></span> | <span data-ttu-id="6fc08-887">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-888">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-888">**Name**</span></span>                                                           | <span data-ttu-id="6fc08-889">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-889">Yes</span></span>         | <span data-ttu-id="6fc08-890">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-891">**Type**</span></span>                                                           | <span data-ttu-id="6fc08-892">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-892">Yes</span></span>         | <span data-ttu-id="6fc08-893">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-894">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-894">**Nullable**</span></span>                                                       | <span data-ttu-id="6fc08-895">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-895">No</span></span>          | <span data-ttu-id="6fc08-896">**True** (valeur par défaut) ou **False** selon que la propriété peut avoir ou non une valeur null.</span><span class="sxs-lookup"><span data-stu-id="6fc08-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6fc08-897">> Dans CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6fc08-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6fc08-899">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-899">No</span></span>          | <span data-ttu-id="6fc08-900">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6fc08-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="6fc08-902">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-902">No</span></span>          | <span data-ttu-id="6fc08-903">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-904">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="6fc08-905">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-905">No</span></span>          | <span data-ttu-id="6fc08-906">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6fc08-907">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-907">**Precision**</span></span>                                                      | <span data-ttu-id="6fc08-908">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-908">No</span></span>          | <span data-ttu-id="6fc08-909">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6fc08-910">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-910">**Scale**</span></span>                                                          | <span data-ttu-id="6fc08-911">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-911">No</span></span>          | <span data-ttu-id="6fc08-912">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6fc08-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-913">**SRID**</span></span>                                                           | <span data-ttu-id="6fc08-914">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-914">No</span></span>          | <span data-ttu-id="6fc08-915">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-916">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6fc08-917">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6fc08-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-918">**Unicode**</span></span>                                                        | <span data-ttu-id="6fc08-919">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-919">No</span></span>          | <span data-ttu-id="6fc08-920">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6fc08-921">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-921">**Collation**</span></span>                                                      | <span data-ttu-id="6fc08-922">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-922">No</span></span>          | <span data-ttu-id="6fc08-923">Chaîne qui spécifie l’ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6fc08-924">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="6fc08-925">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-926">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="6fc08-927">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-927">Example</span></span>

<span data-ttu-id="6fc08-928">L’exemple suivant montre les éléments de **propriété** utilisés pour définir la forme du type de retour d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="6fc08-929">PropertyRef, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="6fc08-930">L’élément **PropertyRef** en Conceptual Schema Definition Language (CSDL) fait référence à une propriété d’un type d’entité pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="6fc08-931">Partie de la clé de l'entité (une propriété ou un jeu de propriétés d'un type d'entité qui détermine l'identité).</span><span class="sxs-lookup"><span data-stu-id="6fc08-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="6fc08-932">Un ou plusieurs éléments **PropertyRef** peuvent être utilisés pour définir une clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="6fc08-933">Terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="6fc08-934">L’élément **PropertyRef** peut uniquement comporter des éléments d’annotation (zéro ou plus) en tant qu’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-935">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6fc08-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-936">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-936">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-937">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="6fc08-938">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-938">Attribute Name</span></span> | <span data-ttu-id="6fc08-939">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-939">Is Required</span></span> | <span data-ttu-id="6fc08-940">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="6fc08-941">**Nom**</span><span class="sxs-lookup"><span data-stu-id="6fc08-941">**Name**</span></span>       | <span data-ttu-id="6fc08-942">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-942">Yes</span></span>         | <span data-ttu-id="6fc08-943">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="6fc08-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-944">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="6fc08-945">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-946">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-947">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-947">Example</span></span>

<span data-ttu-id="6fc08-948">L’exemple ci-dessous définit un type**d'** entité (Book).</span><span class="sxs-lookup"><span data-stu-id="6fc08-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="6fc08-949">La clé d’entité est définie en référençant la propriété **ISBN** du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="6fc08-950">Dans l’exemple suivant, deux éléments **PropertyRef** sont utilisés pour indiquer que deux propriétés (**ID** et **PublisherId**) sont les terminaisons principale et dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="6fc08-951">Élément ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-952">L’élément **ReferenceType** en Conceptual Schema Definition Language (CSDL) spécifie une référence à un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="6fc08-953">L’élément **ReferenceType** peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="6fc08-954">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="6fc08-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="6fc08-955">Paramètre</span><span class="sxs-lookup"><span data-stu-id="6fc08-955">Parameter</span></span>
-   <span data-ttu-id="6fc08-956">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-956">CollectionType</span></span>

<span data-ttu-id="6fc08-957">L’élément **ReferenceType** est utilisé lors de la définition d’un paramètre ou d’un type de retour pour une fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="6fc08-958">Un élément **ReferenceType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-959">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-960">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-961">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-961">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-962">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="6fc08-963">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-963">Attribute Name</span></span> | <span data-ttu-id="6fc08-964">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-964">Is Required</span></span> | <span data-ttu-id="6fc08-965">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="6fc08-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-966">**Type**</span></span>       | <span data-ttu-id="6fc08-967">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-967">Yes</span></span>         | <span data-ttu-id="6fc08-968">Nom du type d'entité référencé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-969">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="6fc08-970">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-971">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-972">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-972">Example</span></span>

<span data-ttu-id="6fc08-973">L’exemple suivant montre l’élément **ReferenceType** utilisé comme enfant d’un élément **Parameter** dans une fonction définie par modèle qui accepte une référence à un type d’entité **Person** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="6fc08-974">L’exemple suivant montre l’élément **ReferenceType** utilisé comme enfant d’un élément **ReturnType** (Function) dans une fonction définie par modèle qui retourne une référence à un type d’entité **Person** :</span><span class="sxs-lookup"><span data-stu-id="6fc08-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="6fc08-975">ReferentialConstraint, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="6fc08-976">Un élément **ReferentialConstraint** en Conceptual Schema Definition Language (CSDL) définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6fc08-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="6fc08-977">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="6fc08-978">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="6fc08-979">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="6fc08-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="6fc08-980">Si une clé étrangère exposée sur un type d’entité fait référence à une propriété sur un autre type d’entité, l’élément **ReferentialConstraint** définit une association entre les deux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="6fc08-981">Étant donné que l’élément **ReferentialConstraint** fournit des informations sur la façon dont deux types d’entité sont associés, aucun élément AssociationSetMapping correspondant n’est nécessaire dans le Mapping Specification Language (MSL).</span><span class="sxs-lookup"><span data-stu-id="6fc08-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="6fc08-982">Une association entre deux types d’entités qui n’ont pas de clés étrangères exposées doit avoir un élément **AssociationSetMapping** correspondant pour mapper les informations d’association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="6fc08-983">Si une clé étrangère n’est pas exposée sur un type d’entité, l’élément **ReferentialConstraint** ne peut définir qu’une contrainte de clé primaire-clé primaire entre le type d’entité et un autre type d’entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="6fc08-984">Un élément **ReferentialConstraint** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-985">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-986">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="6fc08-987">Dépendent (exactement un élément) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="6fc08-988">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-989">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-989">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-990">L’élément **ReferentialConstraint** peut avoir n’importe quel nombre d’attributs d’annotation (attributs XML personnalisés).</span><span class="sxs-lookup"><span data-stu-id="6fc08-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="6fc08-991">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-992">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-993">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-993">Example</span></span>

<span data-ttu-id="6fc08-994">L’exemple suivant montre un élément **ReferentialConstraint** utilisé dans le cadre de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="6fc08-995">ReturnType (Function), élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="6fc08-996">L’élément **ReturnType** (Function) en Conceptual Schema Definition Language (CSDL) spécifie le type de retour pour une fonction définie dans un élément Function.</span><span class="sxs-lookup"><span data-stu-id="6fc08-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="6fc08-997">Un type de retour de fonction peut également être spécifié avec un attribut **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6fc08-998">Les types de retour peuvent être n’importe quel **type EDMSimpleType**, type d’entité, type complexe, type de ligne, type ref ou une collection de l’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="6fc08-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="6fc08-999">Le type de retour d’une fonction peut être spécifié avec l’attribut de **type** de l’élément **ReturnType** (Function), ou avec l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="6fc08-1000">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-1000">CollectionType</span></span>
-   <span data-ttu-id="6fc08-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1001">ReferenceType</span></span>
-   <span data-ttu-id="6fc08-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-1003">Un modèle ne sera pas validé si vous spécifiez un type de retour de fonction avec à la fois l’attribut de **type** de l’élément **ReturnType** (Function) et l’un des éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1004">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1004">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1005">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="6fc08-1006">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-1006">Attribute Name</span></span> | <span data-ttu-id="6fc08-1007">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-1007">Is Required</span></span> | <span data-ttu-id="6fc08-1008">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="6fc08-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1009">**ReturnType**</span></span> | <span data-ttu-id="6fc08-1010">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1010">No</span></span>          | <span data-ttu-id="6fc08-1011">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-1012">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="6fc08-1013">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1014">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-1015">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1015">Example</span></span>

<span data-ttu-id="6fc08-1016">L’exemple suivant utilise un élément **Function** pour définir une fonction qui retourne le nombre d’années pendant lequel un livre a été imprimé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="6fc08-1017">Notez que le type de retour est spécifié par l’attribut **type** d’un élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="6fc08-1018">ReturnType (FunctionImport), élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="6fc08-1019">L’élément **ReturnType** (FunctionImport) de Conceptual Schema Definition Language (CSDL) spécifie le type de retour pour une fonction définie dans un élément FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="6fc08-1020">Un type de retour de fonction peut également être spécifié avec un attribut **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="6fc08-1021">Les types de retour peuvent être n’importe quelle collection de type d’entité, de type complexe ou de **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="6fc08-1022">Le type de retour d’une fonction est spécifié avec l’attribut de **type** de l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1023">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1023">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1024">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="6fc08-1025">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-1025">Attribute Name</span></span> | <span data-ttu-id="6fc08-1026">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-1026">Is Required</span></span> | <span data-ttu-id="6fc08-1027">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1028">**Type**</span></span>       | <span data-ttu-id="6fc08-1029">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1029">No</span></span>          | <span data-ttu-id="6fc08-1030">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1030">The type that the function returns.</span></span> <span data-ttu-id="6fc08-1031">La valeur doit être une collection de ComplexType, EntityType ou type EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="6fc08-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1032">**EntitySet**</span></span>  | <span data-ttu-id="6fc08-1033">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1033">No</span></span>          | <span data-ttu-id="6fc08-1034">Si la fonction retourne une collection de types d’entités, la valeur de l' **EntitySet** doit être le jeu d’entités auquel la collection appartient.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="6fc08-1035">Dans le cas contraire, l’attribut **EntitySet** ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-1036">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="6fc08-1037">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1038">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-1039">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1039">Example</span></span>

<span data-ttu-id="6fc08-1040">L’exemple suivant utilise un **FunctionImport** qui retourne des livres et des serveurs de publication.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="6fc08-1041">Notez que la fonction retourne deux jeux de résultats et, par conséquent, deux éléments **ReturnType** (FunctionImport) sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="6fc08-1042">Élément RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="6fc08-1043">Un élément **RowType** en Conceptual Schema Definition Language (CSDL) définit une structure sans nom en tant que paramètre ou type de retour pour une fonction définie dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="6fc08-1044">Un élément **RowType** peut être l’enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="6fc08-1045">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-1045">CollectionType</span></span>
-   <span data-ttu-id="6fc08-1046">Paramètre</span><span class="sxs-lookup"><span data-stu-id="6fc08-1046">Parameter</span></span>
-   <span data-ttu-id="6fc08-1047">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1047">ReturnType (Function)</span></span>

<span data-ttu-id="6fc08-1048">Un élément **RowType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-1049">Property (un ou plusieurs) ;</span><span class="sxs-lookup"><span data-stu-id="6fc08-1049">Property (one or more)</span></span>
-   <span data-ttu-id="6fc08-1050">éléments d'annotation (zéro, un ou plusieurs).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1051">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1051">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1052">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **RowType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="6fc08-1053">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1054">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-1055">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1055">Example</span></span>

<span data-ttu-id="6fc08-1056">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de lignes (comme spécifié dans l’élément **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="6fc08-1057">Schema, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="6fc08-1058">L’élément **Schema** est l’élément racine d’une définition de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="6fc08-1059">Il contient des définitions pour les objets, fonctions et conteneurs qui composent un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="6fc08-1060">L’élément **Schema** peut contenir zéro, un ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="6fc08-1061">Utilisation</span><span class="sxs-lookup"><span data-stu-id="6fc08-1061">Using</span></span>
-   <span data-ttu-id="6fc08-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="6fc08-1062">EntityContainer</span></span>
-   <span data-ttu-id="6fc08-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1063">EntityType</span></span>
-   <span data-ttu-id="6fc08-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1064">EnumType</span></span>
-   <span data-ttu-id="6fc08-1065">Association</span><span class="sxs-lookup"><span data-stu-id="6fc08-1065">Association</span></span>
-   <span data-ttu-id="6fc08-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1066">ComplexType</span></span>
-   <span data-ttu-id="6fc08-1067">Fonction</span><span class="sxs-lookup"><span data-stu-id="6fc08-1067">Function</span></span>

<span data-ttu-id="6fc08-1068">Un élément de **schéma** peut contenir zéro ou un élément d’annotation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-1069">L’élément de **fonction** et les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="6fc08-1070">L’élément **Schema** utilise l’attribut **namespace** pour définir l’espace de noms pour le type d’entité, le type complexe et les objets Association dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="6fc08-1071">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="6fc08-1072">Les espaces de noms peuvent s’étendre sur plusieurs éléments de **schéma** et plusieurs fichiers. csdl.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="6fc08-1073">Un espace de noms de modèle conceptuel est différent de l’espace de noms XML de l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="6fc08-1074">Un espace de noms de modèle conceptuel (tel que défini par l’attribut **namespace** ) est un conteneur logique pour les types d’entité, les types complexes et les types d’association.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="6fc08-1075">L’espace de noms XML (indiqué par l’attribut **xmlns** ) d’un élément de **schéma** est l’espace de noms par défaut pour les éléments enfants et les attributs de l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="6fc08-1076">Les espaces de noms XML de la forme https://schemas.microsoft.com/ado/YYYY/MM/edm (où aaaa et MM représentent respectivement une année et un mois) sont réservés au langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1077">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1078">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1078">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1079">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Schema** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="6fc08-1080">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-1080">Attribute Name</span></span> | <span data-ttu-id="6fc08-1081">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-1081">Is Required</span></span> | <span data-ttu-id="6fc08-1082">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-1083">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1083">**Namespace**</span></span>  | <span data-ttu-id="6fc08-1084">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1084">Yes</span></span>         | <span data-ttu-id="6fc08-1085">Espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="6fc08-1086">La valeur de l’attribut d' **espace de noms** est utilisée pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="6fc08-1087">Par exemple, si un **EntityType** nommé *Customer* est dans l’espace de noms simple. example. Model, le nom qualifié complet de l' **EntityType** est SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="6fc08-1088">Les chaînes suivantes ne peuvent pas être utilisées comme valeur pour l’attribut d' **espace de noms** : **System**, **transient**ou **EDM**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="6fc08-1089">La valeur de l’attribut d' **espace de noms** ne peut pas être la même que la valeur de l’attribut d' **espace de noms** dans l’élément de schéma SSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="6fc08-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1090">**Alias**</span></span>      | <span data-ttu-id="6fc08-1091">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1091">No</span></span>          | <span data-ttu-id="6fc08-1092">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6fc08-1093">Par exemple, si un **EntityType** nommé *Customer* est dans l’espace de noms simple. example. Model et que la valeur de l’attribut **alias** est *Model*, vous pouvez utiliser Model. Customer comme nom qualifié complet de l' **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="6fc08-1094">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="6fc08-1095">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1096">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-1097">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1097">Example</span></span>

<span data-ttu-id="6fc08-1098">L’exemple suivant illustre un élément de **schéma** qui contient un élément **EntityContainer** , deux éléments **EntityType** et un élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="6fc08-1099">Élément TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="6fc08-1100">L’élément **TypeRef** en Conceptual Schema Definition Language (CSDL) fournit une référence à un type nommé existant.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="6fc08-1101">L’élément **TypeRef** peut être un enfant de l’élément CollectionType, qui est utilisé pour spécifier qu’une fonction a une collection en tant que paramètre ou type de retour.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="6fc08-1102">Un élément **TypeRef** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="6fc08-1103">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="6fc08-1104">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1105">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1105">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1106">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="6fc08-1107">Notez que les attributs **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**et **collation** sont uniquement applicables à **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="6fc08-1108">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="6fc08-1109">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-1109">Is Required</span></span> | <span data-ttu-id="6fc08-1110">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1111">**Type**</span></span>                                                           | <span data-ttu-id="6fc08-1112">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1112">No</span></span>          | <span data-ttu-id="6fc08-1113">Nom du type référencé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="6fc08-1114">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="6fc08-1115">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1115">No</span></span>          | <span data-ttu-id="6fc08-1116">**True** (valeur par défaut) ou **False** selon que la propriété peut avoir ou non une valeur null.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="6fc08-1117">> Dans CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="6fc08-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="6fc08-1119">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1119">No</span></span>          | <span data-ttu-id="6fc08-1120">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="6fc08-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="6fc08-1122">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1122">No</span></span>          | <span data-ttu-id="6fc08-1123">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="6fc08-1124">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="6fc08-1125">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1125">No</span></span>          | <span data-ttu-id="6fc08-1126">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="6fc08-1127">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1127">**Precision**</span></span>                                                      | <span data-ttu-id="6fc08-1128">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1128">No</span></span>          | <span data-ttu-id="6fc08-1129">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="6fc08-1130">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1130">**Scale**</span></span>                                                          | <span data-ttu-id="6fc08-1131">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1131">No</span></span>          | <span data-ttu-id="6fc08-1132">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="6fc08-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1133">**SRID**</span></span>                                                           | <span data-ttu-id="6fc08-1134">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1134">No</span></span>          | <span data-ttu-id="6fc08-1135">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="6fc08-1136">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="6fc08-1137">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="6fc08-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="6fc08-1139">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1139">No</span></span>          | <span data-ttu-id="6fc08-1140">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="6fc08-1141">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1141">**Collation**</span></span>                                                      | <span data-ttu-id="6fc08-1142">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1142">No</span></span>          | <span data-ttu-id="6fc08-1143">Chaîne qui spécifie l’ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="6fc08-1144">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="6fc08-1145">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1146">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-1147">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1147">Example</span></span>

<span data-ttu-id="6fc08-1148">L’exemple suivant montre une fonction définie par modèle qui utilise l’élément **TypeRef** (en tant qu’enfant d’un élément **CollectionType** ) pour spécifier que la fonction accepte une collection de types d’entités **Department** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="6fc08-1149">Using, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="6fc08-1150">L’élément **using** de Conceptual Schema Definition Language (CSDL) importe le contenu d’un modèle conceptuel qui existe dans un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="6fc08-1151">En définissant la valeur de l’attribut d' **espace de noms** , vous pouvez faire référence à des types d’entités, des types complexes et des types d’associations définis dans un autre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="6fc08-1152">Plus d’un élément **using** peut être un enfant d’un élément **Schema** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-1153">L’élément **using** dans CSDL ne fonctionne pas exactement comme une instruction **using** dans un langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="6fc08-1154">En important un espace de noms avec une instruction **using** dans un langage de programmation, vous n’affectez pas les objets de l’espace de noms d’origine.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="6fc08-1155">Dans le langage CSDL, un espace de noms importé peut contenir un type d'entité dérivé d'un type d'entité figurant dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="6fc08-1156">Cela peut affecter les jeux d'entités déclarés dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="6fc08-1157">L’élément **using** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="6fc08-1158">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="6fc08-1159">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="6fc08-1160">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1160">Applicable Attributes</span></span>

<span data-ttu-id="6fc08-1161">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="6fc08-1162">Nom de l'attribut</span><span class="sxs-lookup"><span data-stu-id="6fc08-1162">Attribute Name</span></span> | <span data-ttu-id="6fc08-1163">Est obligatoire</span><span class="sxs-lookup"><span data-stu-id="6fc08-1163">Is Required</span></span> | <span data-ttu-id="6fc08-1164">Valeur</span><span class="sxs-lookup"><span data-stu-id="6fc08-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-1165">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1165">**Namespace**</span></span>  | <span data-ttu-id="6fc08-1166">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1166">Yes</span></span>         | <span data-ttu-id="6fc08-1167">Nom de l'espace de noms importé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="6fc08-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1168">**Alias**</span></span>      | <span data-ttu-id="6fc08-1169">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1169">Yes</span></span>         | <span data-ttu-id="6fc08-1170">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="6fc08-1171">Bien que cet attribut soit obligatoire, il n'est pas nécessaire qu'il soit utilisé à la place du nom de l'espace de noms pour qualifier les noms d'objets.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="6fc08-1172">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="6fc08-1173">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="6fc08-1174">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="6fc08-1175">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1175">Example</span></span>

<span data-ttu-id="6fc08-1176">L’exemple suivant illustre l' **utilisation** de l’élément Using pour importer un espace de noms défini ailleurs.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="6fc08-1177">Notez que l’espace de noms de l’élément de **schéma** indiqué est `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="6fc08-1178">La propriété `Address` sur le `Publisher`**EntityType** est un type complexe défini dans l’espace de noms `ExtendedBooksModel` (importé avec l’élément **using** ).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="6fc08-1179">Attributs d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="6fc08-1180">Les attributs d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des attributs XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="6fc08-1181">En plus d'avoir une structure XML valide, les attributs d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="6fc08-1182">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6fc08-1183">Plusieurs attributs d'annotation peuvent être appliqués à un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="6fc08-1184">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="6fc08-1185">Les attributs d'annotation peuvent être utilisés pour fournir des métadonnées supplémentaires sur des éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6fc08-1186">Vous pouvez accéder aux métadonnées contenues dans les éléments d’annotation au moment de l’exécution à l’aide des classes de l’espace de noms System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-1187">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1187">Example</span></span>

<span data-ttu-id="6fc08-1188">L’exemple suivant montre un élément **EntityType** avec un attribut d’annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="6fc08-1189">L'exemple fait également apparaître un élément d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="6fc08-1190">Le code suivant récupère les métadonnées dans l'attribut d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="6fc08-1191">Le code ci-dessus suppose que le fichier `School.csdl` se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="6fc08-1192">Éléments d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="6fc08-1193">Les éléments d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des éléments XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="6fc08-1194">En plus d'avoir une structure XML valide, les éléments d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="6fc08-1195">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="6fc08-1196">Plusieurs éléments d'annotation peuvent être des enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="6fc08-1197">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="6fc08-1198">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="6fc08-1199">Les éléments d'annotation permettent de fournir des métadonnées supplémentaires sur les éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="6fc08-1200">À partir de la .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles au moment de l’exécution à l’aide des classes de l’espace de noms System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-1201">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1201">Example</span></span>

<span data-ttu-id="6fc08-1202">L’exemple suivant montre un élément **EntityType** avec un élément annotation (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="6fc08-1203">L'exemple fait également apparaître un attribut d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="6fc08-1204">Le code suivant récupère les métadonnées dans l'élément d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="6fc08-1205">Le code ci-dessus suppose que le fichier School.csdl se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="6fc08-1206">Types de modèles conceptuels (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="6fc08-1207">Le langage CSDL (Conceptual Schema Definition Language) prend en charge un ensemble de types de données primitifs abstraits, appelés **EDMSimpleTypes**, qui définissent des propriétés dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="6fc08-1208">Les **EDMSimpleTypes** sont des proxys pour les types de données primitifs pris en charge dans l’environnement de stockage ou d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="6fc08-1209">Le tableau suivant répertorie les types de données primitifs pris en charge par CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="6fc08-1210">Le tableau répertorie également les facettes qui peuvent être appliquées à chaque **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="6fc08-1211">Type EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="6fc08-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="6fc08-1212">Description</span><span class="sxs-lookup"><span data-stu-id="6fc08-1212">Description</span></span>                                                | <span data-ttu-id="6fc08-1213">Facettes applicables</span><span class="sxs-lookup"><span data-stu-id="6fc08-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="6fc08-1214">**EDM. binaire**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="6fc08-1215">Contient des données binaires.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="6fc08-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="6fc08-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="6fc08-1218">Contient la valeur **true** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="6fc08-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="6fc08-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="6fc08-1221">Contient une valeur d'entier 8 bits non signé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="6fc08-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="6fc08-1224">Représente une date et une heure.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="6fc08-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="6fc08-1227">Contient une date et heure exprimant un décalage en minutes par rapport à l'heure GMT.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="6fc08-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="6fc08-1230">Contient une valeur numérique avec une précision et une échelle fixes.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="6fc08-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="6fc08-1233">Contient un nombre à virgule flottante avec une précision de 15 chiffres</span><span class="sxs-lookup"><span data-stu-id="6fc08-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="6fc08-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="6fc08-1236">Contient un nombre à virgule flottante avec une précision de 7 chiffres.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="6fc08-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1238">**EDM. Guid**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="6fc08-1239">Contient un identificateur unique de 16 octets.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="6fc08-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="6fc08-1242">Contient une valeur d'entier 16 bits signé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="6fc08-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="6fc08-1245">Contient une valeur d'entier 32 bits signé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="6fc08-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="6fc08-1248">Contient une valeur d'entier 64 bits signé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="6fc08-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="6fc08-1251">Contient une valeur d'entier 8 bits signé.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="6fc08-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1253">**Edm.String**</span></span>                   | <span data-ttu-id="6fc08-1254">Contient des données caractères.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1254">Contains character data.</span></span>                                   | <span data-ttu-id="6fc08-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="6fc08-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="6fc08-1257">Contient une heure.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="6fc08-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="6fc08-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="6fc08-1259">**EDM. Geography**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="6fc08-1260">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="6fc08-1262">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="6fc08-1264">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="6fc08-1266">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="6fc08-1268">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="6fc08-1270">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="6fc08-1272">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1273">**EDM. GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="6fc08-1274">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="6fc08-1276">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="6fc08-1278">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="6fc08-1280">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="6fc08-1282">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="6fc08-1284">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="6fc08-1286">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="6fc08-1288">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="6fc08-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="6fc08-1290">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="6fc08-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="6fc08-1291">Facettes (CSDL)</span><span class="sxs-lookup"><span data-stu-id="6fc08-1291">Facets (CSDL)</span></span>

<span data-ttu-id="6fc08-1292">Les facettes dans le langage CSDL (Conceptual Schema Definition Language) représentent des contraintes sur les propriétés de types d'entités et de types complexes.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="6fc08-1293">Les facettes apparaissent comme des attributs XML sur les éléments CSDL suivants :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="6fc08-1294">Propriété</span><span class="sxs-lookup"><span data-stu-id="6fc08-1294">Property</span></span>
-   <span data-ttu-id="6fc08-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="6fc08-1295">TypeRef</span></span>
-   <span data-ttu-id="6fc08-1296">Paramètre</span><span class="sxs-lookup"><span data-stu-id="6fc08-1296">Parameter</span></span>

<span data-ttu-id="6fc08-1297">Le tableau ci-dessous décrit les facettes prises en charge dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="6fc08-1298">Toutes les facettes sont facultatives.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1298">All facets are optional.</span></span> <span data-ttu-id="6fc08-1299">Certaines facettes répertoriées ci-dessous sont utilisées par la Entity Framework lors de la génération d’une base de données à partir d’un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="6fc08-1300">Pour plus d’informations sur les types de données dans un modèle conceptuel, consultez types de modèles conceptuels (CSDL).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="6fc08-1301">Facette</span><span class="sxs-lookup"><span data-stu-id="6fc08-1301">Facet</span></span>               | <span data-ttu-id="6fc08-1302">Description</span><span class="sxs-lookup"><span data-stu-id="6fc08-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="6fc08-1303">S’applique à</span><span class="sxs-lookup"><span data-stu-id="6fc08-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="6fc08-1304">Utilisée pour la génération de base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1304">Used for the database generation</span></span> | <span data-ttu-id="6fc08-1305">Utilisée par le runtime.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="6fc08-1306">**Classement**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1306">**Collation**</span></span>       | <span data-ttu-id="6fc08-1307">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="6fc08-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6fc08-1309">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1309">Yes</span></span>                              | <span data-ttu-id="6fc08-1310">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1310">No</span></span>                  |
| <span data-ttu-id="6fc08-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="6fc08-1312">Indique que la valeur de la propriété doit être utilisée pour des contrôles d'accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="6fc08-1313">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6fc08-1314">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1314">No</span></span>                               | <span data-ttu-id="6fc08-1315">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1315">Yes</span></span>                 |
| <span data-ttu-id="6fc08-1316">**Par défaut**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1316">**Default**</span></span>         | <span data-ttu-id="6fc08-1317">Spécifie la valeur par défaut de la propriété si aucune valeur n'est fournie en cas d'instanciation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="6fc08-1318">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6fc08-1319">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1319">Yes</span></span>                              | <span data-ttu-id="6fc08-1320">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1320">Yes</span></span>                 |
| <span data-ttu-id="6fc08-1321">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1321">**FixedLength**</span></span>     | <span data-ttu-id="6fc08-1322">Spécifie si la longueur de la valeur de propriété peut varier.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="6fc08-1323">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6fc08-1324">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1324">Yes</span></span>                              | <span data-ttu-id="6fc08-1325">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1325">No</span></span>                  |
| <span data-ttu-id="6fc08-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1326">**MaxLength**</span></span>       | <span data-ttu-id="6fc08-1327">Spécifie la longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="6fc08-1328">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6fc08-1329">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1329">Yes</span></span>                              | <span data-ttu-id="6fc08-1330">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1330">No</span></span>                  |
| <span data-ttu-id="6fc08-1331">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1331">**Nullable**</span></span>        | <span data-ttu-id="6fc08-1332">Spécifie si la propriété peut avoir une valeur **null** .</span><span class="sxs-lookup"><span data-stu-id="6fc08-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="6fc08-1333">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="6fc08-1334">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1334">Yes</span></span>                              | <span data-ttu-id="6fc08-1335">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1335">Yes</span></span>                 |
| <span data-ttu-id="6fc08-1336">**Précision**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1336">**Precision**</span></span>       | <span data-ttu-id="6fc08-1337">Pour les propriétés de type **Decimal**, spécifie le nombre de chiffres qu’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="6fc08-1338">Pour les propriétés de type **Time**, **DateTime**et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="6fc08-1339">**Edm. DateTime**, **Edm. DateTimeOffset**, **Edm. Decimal**, **Edm. Time**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="6fc08-1340">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1340">Yes</span></span>                              | <span data-ttu-id="6fc08-1341">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1341">No</span></span>                  |
| <span data-ttu-id="6fc08-1342">**Mettre à l'échelle**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1342">**Scale**</span></span>           | <span data-ttu-id="6fc08-1343">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="6fc08-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="6fc08-1345">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1345">Yes</span></span>                              | <span data-ttu-id="6fc08-1346">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1346">No</span></span>                  |
| <span data-ttu-id="6fc08-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1347">**SRID**</span></span>            | <span data-ttu-id="6fc08-1348">Spécifie l’ID du système de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="6fc08-1349">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fc08-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="6fc08-1350">**EDM. Geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. GeographyCollection, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="6fc08-1351">Non</span><span class="sxs-lookup"><span data-stu-id="6fc08-1351">No</span></span>                               | <span data-ttu-id="6fc08-1352">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1352">Yes</span></span>                 |
| <span data-ttu-id="6fc08-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1353">**Unicode**</span></span>         | <span data-ttu-id="6fc08-1354">Indique si la valeur de propriété est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="6fc08-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="6fc08-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="6fc08-1356">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1356">Yes</span></span>                              | <span data-ttu-id="6fc08-1357">Oui</span><span class="sxs-lookup"><span data-stu-id="6fc08-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="6fc08-1358">Lors de la génération d’une base de données à partir d’un modèle conceptuel, l’Assistant génération de base de données reconnaît la valeur de l’attribut **StoreGeneratedPattern** sur un élément de **propriété** s’il se trouve dans l’espace de noms suivant : https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="6fc08-1359">Les valeurs prises en charge pour l’attribut sont **Identity** et **computeed**.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="6fc08-1360">La valeur **Identity** produit une colonne de base de données avec une valeur d’identité générée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="6fc08-1361">Une valeur **calculée** génère une colonne avec une valeur qui est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6fc08-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="6fc08-1362">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fc08-1362">Example</span></span>

<span data-ttu-id="6fc08-1363">L'exemple suivant illustre l'application de facettes aux propriétés d'un type d'entité :</span><span class="sxs-lookup"><span data-stu-id="6fc08-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
