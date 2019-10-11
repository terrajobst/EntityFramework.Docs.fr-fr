---
title: Spécification CSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182597"
---
# <a name="csdl-specification"></a><span data-ttu-id="3f29b-102">Spécification CSDL</span><span class="sxs-lookup"><span data-stu-id="3f29b-102">CSDL Specification</span></span>
<span data-ttu-id="3f29b-103">Le langage CSDL (Conceptual Schema Definition Language) est un langage basé sur XML qui décrit les entités, relations et fonctions qui composent un modèle conceptuel d'une application pilotée par les données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="3f29b-104">Ce modèle conceptuel peut être utilisé par le Entity Framework ou WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="3f29b-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="3f29b-105">Les métadonnées qui sont décrites avec le langage CSDL sont utilisées par le Entity Framework pour mapper les entités et les relations définies dans un modèle conceptuel à une source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="3f29b-106">Pour plus d’informations, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) et [spécification MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="3f29b-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="3f29b-107">Le langage CSDL est l’implémentation de la Entity Framework du Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="3f29b-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="3f29b-108">Dans une application Entity Framework, les métadonnées de modèle conceptuel sont chargées à partir d’un fichier. CSDL (écrit en CSDL) dans une instance de System. Data. Metadata. Edm. EdmItemCollection et sont accessibles à l’aide des méthodes de la Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="3f29b-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="3f29b-109">Entity Framework utilise les métadonnées du modèle conceptuel pour traduire les requêtes sur le modèle conceptuel en commandes spécifiques à la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="3f29b-110">Le concepteur EF stocke les informations de modèle conceptuel dans un fichier. edmx au moment de la conception.</span><span class="sxs-lookup"><span data-stu-id="3f29b-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="3f29b-111">Au moment de la génération, le concepteur EF utilise les informations d’un fichier. edmx pour créer le fichier. csdl requis par Entity Framework au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="3f29b-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="3f29b-112">Les versions de CSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="3f29b-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="3f29b-113">Version CSDL</span><span class="sxs-lookup"><span data-stu-id="3f29b-113">CSDL Version</span></span> | <span data-ttu-id="3f29b-114">Espace de noms XML</span><span class="sxs-lookup"><span data-stu-id="3f29b-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="3f29b-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="3f29b-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="3f29b-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="3f29b-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="3f29b-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="3f29b-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="3f29b-118">Association, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-118">Association Element (CSDL)</span></span>

<span data-ttu-id="3f29b-119">Un élément **Association** définit une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="3f29b-120">Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ».</span><span class="sxs-lookup"><span data-stu-id="3f29b-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="3f29b-121">La multiplicité d’une terminaison d’association peut avoir la valeur un (1), zéro ou un (0.. 1), ou plusieurs (\*).</span><span class="sxs-lookup"><span data-stu-id="3f29b-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="3f29b-122">Ces informations sont spécifiées dans deux éléments finaux enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="3f29b-123">Il est possible d'accéder aux instances de type d'entité au niveau d'une terminaison d'une association via les propriétés de navigation ou les clés étrangères, si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="3f29b-124">Dans une application, une instance d'une association représente une association spécifique entre des instances de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="3f29b-125">Les instances d'association sont regroupées logiquement dans un ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="3f29b-126">Un élément **Association** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-127">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-128">End (exactement 2 éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="3f29b-129">ReferentialConstraint (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-130">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-131">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-131">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-132">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="3f29b-133">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-133">Attribute Name</span></span> | <span data-ttu-id="3f29b-134">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-134">Is Required</span></span> | <span data-ttu-id="3f29b-135">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="3f29b-136">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-136">**Name**</span></span>       | <span data-ttu-id="3f29b-137">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-137">Yes</span></span>         | <span data-ttu-id="3f29b-138">Nom de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-139">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="3f29b-140">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-141">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-142">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-142">Example</span></span>

<span data-ttu-id="3f29b-143">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** lorsque les clés étrangères n’ont pas été exposées sur les types d’entités **Customer** et **Order** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="3f29b-144">Les valeurs de **multiplicité** pour chaque **terminaison** de l’Association indiquent que de nombreuses **commandes** peuvent être associées à un **client**, mais qu’un seul **client** peut être associé à une **commande**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="3f29b-145">En outre, l’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext seront supprimées si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="3f29b-146">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** lorsque des clés étrangères ont été exposées sur les types d’entités **Customer** et **Order** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="3f29b-147">Avec les clés étrangères exposées, la relation entre les entités est gérée à l’aide d’un élément **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="3f29b-148">Un élément AssociationSetMapping correspondant n’est pas nécessaire pour mapper cette association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="3f29b-149">AssociationSet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="3f29b-150">L’élément **AssociationSet** en Conceptual Schema Definition Language (CSDL) est un conteneur logique pour les instances d’association du même type.</span><span class="sxs-lookup"><span data-stu-id="3f29b-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="3f29b-151">Un ensemble d'associations fournit une définition pour le regroupement d'instances d'association afin qu'elles puissent être mappées à une source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="3f29b-152">L’élément **AssociationSet** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-153">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-154">End (exactement deux éléments requis)</span><span class="sxs-lookup"><span data-stu-id="3f29b-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="3f29b-155">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="3f29b-156">L’attribut **Association** spécifie le type d’association contenu dans un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="3f29b-157">Les jeux d’entités qui composent les terminaisons d’un ensemble d’associations sont spécifiés avec exactement deux éléments **finaux** enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-158">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-158">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-159">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="3f29b-160">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-160">Attribute Name</span></span>  | <span data-ttu-id="3f29b-161">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-161">Is Required</span></span> | <span data-ttu-id="3f29b-162">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-163">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-163">**Name**</span></span>        | <span data-ttu-id="3f29b-164">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-164">Yes</span></span>         | <span data-ttu-id="3f29b-165">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-165">The name of the entity set.</span></span> <span data-ttu-id="3f29b-166">La valeur de l’attribut **Name** ne peut pas être la même que la valeur de l’attribut **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="3f29b-167">**Association**</span><span class="sxs-lookup"><span data-stu-id="3f29b-167">**Association**</span></span> | <span data-ttu-id="3f29b-168">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-168">Yes</span></span>         | <span data-ttu-id="3f29b-169">Nom qualifié complet de l'association dont l'ensemble d'associations contient des instances.</span><span class="sxs-lookup"><span data-stu-id="3f29b-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="3f29b-170">L'association doit être dans le même espace de noms que l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-171">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="3f29b-172">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-173">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-174">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-174">Example</span></span>

<span data-ttu-id="3f29b-175">L’exemple suivant montre un élément **EntityContainer** avec deux éléments **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="3f29b-176">Élément CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-177">L’élément **CollectionType** en Conceptual Schema Definition Language (CSDL) spécifie qu’un paramètre de fonction ou un type de retour de fonction est une collection.</span><span class="sxs-lookup"><span data-stu-id="3f29b-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="3f29b-178">L’élément **CollectionType** peut être un enfant de l’élément parameter ou de l’élément ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="3f29b-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="3f29b-179">Le type de collection peut être spécifié à l’aide de l’attribut **type** ou de l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="3f29b-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-180">**CollectionType**</span></span>
-   <span data-ttu-id="3f29b-181">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-181">ReferenceType</span></span>
-   <span data-ttu-id="3f29b-182">RowType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-182">RowType</span></span>
-   <span data-ttu-id="3f29b-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="3f29b-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-184">Un modèle ne sera pas validé si le type d’une collection est spécifié avec l’attribut **type** et un élément enfant.</span><span class="sxs-lookup"><span data-stu-id="3f29b-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-185">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-185">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-186">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3f29b-187">Notez que les attributs **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**et **collation** sont uniquement applicables aux collections de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="3f29b-188">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-188">Attribute Name</span></span>                                                          | <span data-ttu-id="3f29b-189">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-189">Is Required</span></span> | <span data-ttu-id="3f29b-190">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-191">**Type**</span></span>                                                                | <span data-ttu-id="3f29b-192">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-192">No</span></span>          | <span data-ttu-id="3f29b-193">Type de la collection.</span><span class="sxs-lookup"><span data-stu-id="3f29b-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3f29b-194">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-194">**Nullable**</span></span>                                                            | <span data-ttu-id="3f29b-195">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-195">No</span></span>          | <span data-ttu-id="3f29b-196">**True** (valeur par défaut) ou **false** selon que la propriété peut avoir une valeur null ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="3f29b-197">> Dans le CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="3f29b-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3f29b-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="3f29b-199">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-199">No</span></span>          | <span data-ttu-id="3f29b-200">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="3f29b-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="3f29b-202">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-202">No</span></span>          | <span data-ttu-id="3f29b-203">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="3f29b-204">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="3f29b-205">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-205">No</span></span>          | <span data-ttu-id="3f29b-206">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="3f29b-207">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-207">**Precision**</span></span>                                                           | <span data-ttu-id="3f29b-208">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-208">No</span></span>          | <span data-ttu-id="3f29b-209">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="3f29b-210">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-210">**Scale**</span></span>                                                               | <span data-ttu-id="3f29b-211">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-211">No</span></span>          | <span data-ttu-id="3f29b-212">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-213">**SRID**</span></span>                                                                | <span data-ttu-id="3f29b-214">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-214">No</span></span>          | <span data-ttu-id="3f29b-215">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-216">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="3f29b-217">   Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="3f29b-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="3f29b-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-218">**Unicode**</span></span>                                                             | <span data-ttu-id="3f29b-219">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-219">No</span></span>          | <span data-ttu-id="3f29b-220">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="3f29b-221">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-221">**Collation**</span></span>                                                           | <span data-ttu-id="3f29b-222">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-222">No</span></span>          | <span data-ttu-id="3f29b-223">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="3f29b-224">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3f29b-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-227">Example</span></span>

<span data-ttu-id="3f29b-228">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de types d’entité **Person** (comme spécifié avec l’attribut **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="3f29b-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="3f29b-229">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de lignes (comme spécifié dans l’élément **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="3f29b-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="3f29b-230">L’exemple suivant montre une fonction définie par modèle qui utilise l’élément **CollectionType** pour spécifier que la fonction accepte comme paramètre une collection de types d’entités **Department** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="3f29b-231">ComplexType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-232">Un élément **complexType** définit une structure de données composée de propriétés **type EDMSimpleType** ou d’autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="3f29b-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="3f29b-233">  Un type complexe peut être une propriété d'un type d'entité ou d'un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="3f29b-234">Un type complexe est semblable à un type d'entité du fait qu'un type complexe définit des données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="3f29b-235">Toutefois, il existe des différences clés entre les types complexes et les types d'entités :</span><span class="sxs-lookup"><span data-stu-id="3f29b-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="3f29b-236">Les types complexes n'ont pas d'identité (ni de clés), par conséquent, ils ne peuvent pas exister indépendamment.</span><span class="sxs-lookup"><span data-stu-id="3f29b-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="3f29b-237">Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="3f29b-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="3f29b-238">Les types complexes ne peuvent pas participer aux associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="3f29b-239">Aucune des terminaisons d’une association ne peut être un type complexe ; par conséquent, les propriétés de navigation ne peuvent pas être définies pour des types complexes.</span><span class="sxs-lookup"><span data-stu-id="3f29b-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="3f29b-240">Une propriété de type complexe ne peut pas avoir de valeur NULL, bien que les propriétés scalaires d'un type complexe puissent chacune être définie sur NULL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="3f29b-241">Un élément **complexType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-242">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-243">Property (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="3f29b-244">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="3f29b-245">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **complexType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="3f29b-246">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="3f29b-247">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-247">Is Required</span></span> | <span data-ttu-id="3f29b-248">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-249">Name</span><span class="sxs-lookup"><span data-stu-id="3f29b-249">Name</span></span>                                                                                                           | <span data-ttu-id="3f29b-250">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-250">Yes</span></span>         | <span data-ttu-id="3f29b-251">Nom du type complexe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-251">The name of the complex type.</span></span> <span data-ttu-id="3f29b-252">Le nom d'un type complexe ne peut pas être identique au nom d'un autre type complexe, d'un type d'entité ou d'une association qui figure dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="3f29b-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="3f29b-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="3f29b-254">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-254">No</span></span>          | <span data-ttu-id="3f29b-255">Nom d'un autre type complexe qui est le type de base du type complexe en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="3f29b-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="3f29b-256">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="3f29b-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="3f29b-257">L'héritage pour les types complexes n'est pas pris en charge dans cette version.</span><span class="sxs-lookup"><span data-stu-id="3f29b-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="3f29b-258">Abstraction</span><span class="sxs-lookup"><span data-stu-id="3f29b-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="3f29b-259">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-259">No</span></span>          | <span data-ttu-id="3f29b-260">**True** ou **false** (valeur par défaut), selon que le type complexe est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="3f29b-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="3f29b-261">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="3f29b-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="3f29b-262">Les types complexes dans cette version ne peuvent pas être des types abstraits.</span><span class="sxs-lookup"><span data-stu-id="3f29b-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="3f29b-263">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **complexType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="3f29b-264">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-265">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-266">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-266">Example</span></span>

<span data-ttu-id="3f29b-267">L’exemple suivant montre un type complexe, **Address**, avec les propriétés type EDMSimpleType **StreetAddress**, **City**, **StateOrProvince**, **Country**et **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="3f29b-268">Pour définir l' **adresse** de type complexe (ci-dessus) en tant que propriété d’un type d’entité, vous devez déclarer le type de propriété dans la définition du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="3f29b-269">L’exemple suivant illustre la propriété **Address** en tant que type complexe sur un type d’entité (**éditeur**) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="3f29b-270">DefiningExpression, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="3f29b-271">L’élément **DefiningExpression** en Conceptual Schema Definition Language (CSDL) contient une expression Entity SQL qui définit une fonction dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="3f29b-272">À des fins de validation, un élément **DefiningExpression** peut contenir du contenu arbitraire.</span><span class="sxs-lookup"><span data-stu-id="3f29b-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="3f29b-273">Toutefois, Entity Framework lèvera une exception au moment de l’exécution si un élément **DefiningExpression** ne contient pas d’Entity SQL valide.</span><span class="sxs-lookup"><span data-stu-id="3f29b-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-274">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-274">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-275">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **DefiningExpression** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="3f29b-276">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-277">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-278">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-278">Example</span></span>

<span data-ttu-id="3f29b-279">L’exemple suivant utilise un élément **DefiningExpression** pour définir une fonction qui retourne le nombre d’années écoulées depuis la publication d’un livre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="3f29b-280">Le contenu de l’élément **DefiningExpression** est écrit en Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="3f29b-281">Dependent, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="3f29b-282">L’élément **dépendant** en Conceptual Schema Definition Language (CSDL) est un élément enfant de l’élément ReferentialConstraint et définit la terminaison dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="3f29b-283">Un élément **ReferentialConstraint** définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3f29b-284">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3f29b-285">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3f29b-286">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="3f29b-287">Les éléments **PropertyRef** sont utilisés pour spécifier les clés qui référencent la terminaison principale.</span><span class="sxs-lookup"><span data-stu-id="3f29b-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="3f29b-288">L’élément **dépendant** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-289">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3f29b-290">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-291">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-291">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-292">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **dépendant** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="3f29b-293">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-293">Attribute Name</span></span> | <span data-ttu-id="3f29b-294">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-294">Is Required</span></span> | <span data-ttu-id="3f29b-295">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="3f29b-296">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-296">**Role**</span></span>       | <span data-ttu-id="3f29b-297">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-297">Yes</span></span>         | <span data-ttu-id="3f29b-298">Nom du type d'entité au niveau de la terminaison dépendante de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-299">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **dépendant** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="3f29b-300">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-301">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-302">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-302">Example</span></span>

<span data-ttu-id="3f29b-303">L’exemple suivant montre un élément **ReferentialConstraint** utilisé dans le cadre de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="3f29b-304">La propriété **PublisherId** du type **d’entité Book** compose la terminaison dépendante de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="3f29b-305">Documentation, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="3f29b-306">L’élément **documentation** en Conceptual Schema Definition Language (CSDL) peut être utilisé pour fournir des informations sur un objet défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="3f29b-307">Dans un fichier. edmx, lorsque l’élément **documentation** est un enfant d’un élément qui apparaît sous la forme d’un objet sur l’aire de conception du concepteur EF (tel qu’une entité, une association ou une propriété), le contenu de l’élément de **documentation** s’affiche dans la Fenêtre **Propriétés** de Visual Studio pour l’objet.</span><span class="sxs-lookup"><span data-stu-id="3f29b-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="3f29b-308">L’élément **documentation** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-309">**Résumé**: Brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="3f29b-310">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="3f29b-310">(zero or one element)</span></span>
-   <span data-ttu-id="3f29b-311">**LongDescription**: Description complète de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="3f29b-312">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="3f29b-312">(zero or one element)</span></span>
-   <span data-ttu-id="3f29b-313">Éléments d’annotation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-313">Annotation elements.</span></span> <span data-ttu-id="3f29b-314">(zéro, un ou plusieurs éléments).</span><span class="sxs-lookup"><span data-stu-id="3f29b-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-315">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-315">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-316">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **documentation** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="3f29b-317">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-318">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-319">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-319">Example</span></span>

<span data-ttu-id="3f29b-320">L’exemple suivant montre l’élément de **documentation** en tant qu’élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="3f29b-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="3f29b-321">Si l’extrait de code ci-dessous se trouve dans le contenu CSDL d’un fichier. edmx, le contenu des éléments **Summary** et **LongDescription** apparaît dans la fenêtre **Propriétés** de Visual Studio lorsque vous cliquez sur le type d’entité `Customer`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="3f29b-322">End, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-322">End Element (CSDL)</span></span>

<span data-ttu-id="3f29b-323">L’élément **end** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="3f29b-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="3f29b-324">Dans chaque cas, le rôle de l’élément de **fin** est différent et les attributs applicables sont différents.</span><span class="sxs-lookup"><span data-stu-id="3f29b-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="3f29b-325">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="3f29b-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="3f29b-326">Un élément **end** (en tant qu’enfant de l’élément **Association** ) identifie le type d’entité à une terminaison d’une association et le nombre d’instances de type d’entité qui peuvent exister à cette terminaison d’une association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="3f29b-327">Les terminaisons d'association sont définies dans le cadre d'une association ; une association doit avoir exactement deux terminaisons d'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="3f29b-328">Il est possible d'accéder aux instances de type d'entité au niveau de la terminaison d'une association via les propriétés de navigation ou les clés étrangères si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="3f29b-329">Un élément **end** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-330">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-331">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-332">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-333">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-333">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-334">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **final** lorsqu’il est l’enfant d’un élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="3f29b-335">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-335">Attribute Name</span></span>   | <span data-ttu-id="3f29b-336">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-336">Is Required</span></span> | <span data-ttu-id="3f29b-337">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-338">**Type**</span></span>         | <span data-ttu-id="3f29b-339">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-339">Yes</span></span>         | <span data-ttu-id="3f29b-340">Nom du type d'entité au niveau de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="3f29b-341">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-341">**Role**</span></span>         | <span data-ttu-id="3f29b-342">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-342">No</span></span>          | <span data-ttu-id="3f29b-343">Nom de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-343">A name for the association end.</span></span> <span data-ttu-id="3f29b-344">Si aucun nom n'est fourni, le nom du type d'entité au niveau de la terminaison de l'association sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="3f29b-345">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="3f29b-345">**Multiplicity**</span></span> | <span data-ttu-id="3f29b-346">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-346">Yes</span></span>         | <span data-ttu-id="3f29b-347">**1**, **0.. 1**ou **\*** selon le nombre d’instances de type d’entité qui peuvent être à la fin de l’Association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="3f29b-348">**1** indique qu’il existe exactement une instance de type d’entité au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="3f29b-349">**0.. 1** indique que zéro ou une instance de type d’entité existe au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="3f29b-350">**\*** indique qu’il existe zéro, une ou plusieurs instances de type d’entité au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-351">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fin** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3f29b-352">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-353">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-354">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-354">Example</span></span>

<span data-ttu-id="3f29b-355">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="3f29b-356">Les valeurs de **multiplicité** pour chaque **terminaison** de l’Association indiquent que de nombreuses **commandes** peuvent être associées à un **client**, mais qu’un seul **client** peut être associé à une **commande**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="3f29b-357">En outre, l’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext sont supprimées si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="3f29b-358">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="3f29b-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="3f29b-359">L’élément **end** spécifie une extrémité d’un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="3f29b-360">L’élément **AssociationSet** doit contenir deux éléments **end** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="3f29b-361">Les informations contenues dans un élément **end** sont utilisées dans le mappage d’un ensemble d’associations à une source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="3f29b-362">Un élément **end** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-363">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-364">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-365">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="3f29b-366">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3f29b-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-367">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-367">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-368">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **end** lorsqu’il est l’enfant d’un élément **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="3f29b-369">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-369">Attribute Name</span></span> | <span data-ttu-id="3f29b-370">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-370">Is Required</span></span> | <span data-ttu-id="3f29b-371">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3f29b-372">**EntitySet**</span></span>  | <span data-ttu-id="3f29b-373">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-373">Yes</span></span>         | <span data-ttu-id="3f29b-374">Nom de l’élément **EntitySet** qui définit une extrémité de l’élément **AssociationSet** parent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="3f29b-375">L’élément **EntitySet** doit être défini dans le même conteneur d’entités que l’élément **AssociationSet** parent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="3f29b-376">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-376">**Role**</span></span>       | <span data-ttu-id="3f29b-377">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-377">No</span></span>          | <span data-ttu-id="3f29b-378">Nom de la terminaison de l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-378">The name of the association set end.</span></span> <span data-ttu-id="3f29b-379">Si l’attribut **role** n’est pas utilisé, le nom de la terminaison de l’ensemble d’associations sera le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3f29b-380">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fin** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3f29b-381">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-382">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-383">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-383">Example</span></span>

<span data-ttu-id="3f29b-384">L’exemple suivant montre un élément **EntityContainer** avec deux éléments **AssociationSet** , chacun avec deux éléments **end** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="3f29b-385">EntityContainer, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="3f29b-386">L’élément **EntityContainer** en Conceptual Schema Definition Language (CSDL) est un conteneur logique pour les jeux d’entités, les ensembles d’associations et les importations de fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="3f29b-387">Un conteneur d’entités de modèle conceptuel est mappé à un conteneur d’entités de modèle de stockage par le biais de l’élément EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="3f29b-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="3f29b-388">Un conteneur d'entités de modèle de stockage décrit la structure de la base de données : les jeux d'entités décrivent les tables, les ensembles d'associations décrivent les contraintes de clé étrangère et les importations de fonction décrivent les procédures stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="3f29b-389">Un élément **EntityContainer** peut avoir zéro ou un élément documentation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="3f29b-390">Si un élément de **documentation** est présent, il doit précéder tous les éléments **EntitySet**, **AssociationSet**et **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="3f29b-391">Un élément **EntityContainer** peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-392">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-392">EntitySet</span></span>
-   <span data-ttu-id="3f29b-393">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-393">AssociationSet</span></span>
-   <span data-ttu-id="3f29b-394">FunctionImport ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-394">FunctionImport</span></span>
-   <span data-ttu-id="3f29b-395">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-395">Annotation elements</span></span>

<span data-ttu-id="3f29b-396">Vous pouvez étendre un élément **EntityContainer** pour inclure le contenu d’un autre **EntityContainer** qui se trouve dans le même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3f29b-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="3f29b-397">Pour inclure le contenu d’un autre **EntityContainer**, dans l’élément **EntityContainer** de référence, définissez la valeur de l’attribut **extends** sur le nom de l’élément **EntityContainer** que vous souhaitez inclure.</span><span class="sxs-lookup"><span data-stu-id="3f29b-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="3f29b-398">Tous les éléments enfants de l’élément **EntityContainer** inclus sont traités comme des éléments enfants de l’élément **EntityContainer** de référence.</span><span class="sxs-lookup"><span data-stu-id="3f29b-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-399">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-399">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-400">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="3f29b-401">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-401">Attribute Name</span></span> | <span data-ttu-id="3f29b-402">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-402">Is Required</span></span> | <span data-ttu-id="3f29b-403">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="3f29b-404">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-404">**Name**</span></span>       | <span data-ttu-id="3f29b-405">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-405">Yes</span></span>         | <span data-ttu-id="3f29b-406">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="3f29b-407">**Dure**</span><span class="sxs-lookup"><span data-stu-id="3f29b-407">**Extends**</span></span>    | <span data-ttu-id="3f29b-408">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-408">No</span></span>          | <span data-ttu-id="3f29b-409">Le nom d'un autre conteneur d'entités au sein du même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3f29b-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-410">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="3f29b-411">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-412">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-413">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-413">Example</span></span>

<span data-ttu-id="3f29b-414">L’exemple suivant montre un élément **EntityContainer** qui définit trois jeux d’entités et deux ensembles d’associations.</span><span class="sxs-lookup"><span data-stu-id="3f29b-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="3f29b-415">EntitySet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="3f29b-416">L’élément **EntitySet** dans Conceptual Schema Definition Language est un conteneur logique pour les instances d’un type d’entité et les instances de tout type dérivé de ce type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="3f29b-417">La relation entre un type d'entité et un jeu d'entités est analogue à la relation entre une ligne et une table dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="3f29b-418">Comme une ligne, un type d'entité définit un jeu de données connexes et, comme une table, un jeu d'entités contient des instances de cette définition.</span><span class="sxs-lookup"><span data-stu-id="3f29b-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="3f29b-419">Un jeu d'entités fournit une construction pour le regroupement d'instances du type d'entité, afin qu'elles puissent être mappées aux structures de données associées dans une source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="3f29b-420">Plusieurs jeux d'entités peuvent être définis pour un type d'entité particulier.</span><span class="sxs-lookup"><span data-stu-id="3f29b-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-421">Le concepteur EF ne prend pas en charge les modèles conceptuels qui contiennent plusieurs jeux d’entités par type.</span><span class="sxs-lookup"><span data-stu-id="3f29b-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="3f29b-422">L’élément **EntitySet** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-423">Élément documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-424">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-425">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-426">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="3f29b-427">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-427">Attribute Name</span></span> | <span data-ttu-id="3f29b-428">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-428">Is Required</span></span> | <span data-ttu-id="3f29b-429">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-430">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-430">**Name**</span></span>       | <span data-ttu-id="3f29b-431">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-431">Yes</span></span>         | <span data-ttu-id="3f29b-432">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="3f29b-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-433">**EntityType**</span></span> | <span data-ttu-id="3f29b-434">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-434">Yes</span></span>         | <span data-ttu-id="3f29b-435">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="3f29b-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-436">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="3f29b-437">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-438">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-439">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-439">Example</span></span>

<span data-ttu-id="3f29b-440">L’exemple suivant montre un élément **EntityContainer** avec trois éléments **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="3f29b-441">Il est possible de définir des jeux d'entités multiples par type (MEST).</span><span class="sxs-lookup"><span data-stu-id="3f29b-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="3f29b-442">L’exemple suivant définit un conteneur d’entités avec deux jeux d’entités pour le type **d’entité Book** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="3f29b-443">EntityType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-444">L’élément **EntityType** représente la structure d’un concept de niveau supérieur, tel qu’un client ou une commande, dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="3f29b-445">Un type d'entité est un modèle pour des instances de types d'entités dans une application.</span><span class="sxs-lookup"><span data-stu-id="3f29b-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="3f29b-446">Chaque modèle contient les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3f29b-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="3f29b-447">Nom unique.</span><span class="sxs-lookup"><span data-stu-id="3f29b-447">A unique name.</span></span> <span data-ttu-id="3f29b-448">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="3f29b-448">(Required.)</span></span>
-   <span data-ttu-id="3f29b-449">Clé d'entité définie par une ou plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="3f29b-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="3f29b-450">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="3f29b-450">(Required.)</span></span>
-   <span data-ttu-id="3f29b-451">Propriétés pour contenir des données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-451">Properties for containing data.</span></span> <span data-ttu-id="3f29b-452">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="3f29b-452">(Optional.)</span></span>
-   <span data-ttu-id="3f29b-453">Propriétés de navigation qui permettent de naviguer d'une terminaison d'une association à l'autre terminaison.</span><span class="sxs-lookup"><span data-stu-id="3f29b-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="3f29b-454">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="3f29b-454">(Optional.)</span></span>

<span data-ttu-id="3f29b-455">Dans une application, une instance d'un type d'entité représente un objet spécifique (tel qu'un client ou une commande spécifique).</span><span class="sxs-lookup"><span data-stu-id="3f29b-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="3f29b-456">Chaque instance d'un type d'entité doit avoir une clé d'entité unique dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="3f29b-457">Deux instances de type d'entité sont considérées égales seulement si elles sont du même type et si les valeurs de leurs clés d'entité sont identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="3f29b-458">Un élément **EntityType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-459">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-460">Key (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-461">Property (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="3f29b-462">NavigationProperty (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="3f29b-463">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-464">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-464">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-465">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="3f29b-466">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="3f29b-467">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-467">Is Required</span></span> | <span data-ttu-id="3f29b-468">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-469">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="3f29b-470">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-470">Yes</span></span>         | <span data-ttu-id="3f29b-471">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="3f29b-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="3f29b-473">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-473">No</span></span>          | <span data-ttu-id="3f29b-474">Nom d'un autre type d'entité qui est le type de base du type d'entité en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="3f29b-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="3f29b-475">**Abstraction**</span><span class="sxs-lookup"><span data-stu-id="3f29b-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="3f29b-476">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-476">No</span></span>          | <span data-ttu-id="3f29b-477">**True** ou **false**, selon que le type d’entité est un type abstrait ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="3f29b-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="3f29b-479">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-479">No</span></span>          | <span data-ttu-id="3f29b-480">**True** ou **false** selon que le type d’entité est un type d’entité ouvert.</span><span class="sxs-lookup"><span data-stu-id="3f29b-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="3f29b-481">> L’attribut **OpenType** s’applique uniquement aux types d’entité définis dans les modèles conceptuels utilisés avec ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="3f29b-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="3f29b-482">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="3f29b-483">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-484">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-485">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-485">Example</span></span>

<span data-ttu-id="3f29b-486">L’exemple suivant montre un élément **EntityType** avec trois éléments de **propriété** et deux éléments **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="3f29b-487">EnumType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-488">L’élément **enumType** représente un type énuméré.</span><span class="sxs-lookup"><span data-stu-id="3f29b-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="3f29b-489">Un élément **enumType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-490">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-491">Membre (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="3f29b-492">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-493">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-493">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-494">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **enumType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="3f29b-495">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-495">Attribute Name</span></span>     | <span data-ttu-id="3f29b-496">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-496">Is Required</span></span> | <span data-ttu-id="3f29b-497">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-498">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-498">**Name**</span></span>           | <span data-ttu-id="3f29b-499">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-499">Yes</span></span>         | <span data-ttu-id="3f29b-500">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="3f29b-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="3f29b-501">**IsFlags**</span></span>        | <span data-ttu-id="3f29b-502">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-502">No</span></span>          | <span data-ttu-id="3f29b-503">**True** ou **false**, selon que le type enum peut être utilisé comme un ensemble d’indicateurs.</span><span class="sxs-lookup"><span data-stu-id="3f29b-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="3f29b-504">La valeur par défaut est **false.**</span><span class="sxs-lookup"><span data-stu-id="3f29b-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="3f29b-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-505">**UnderlyingType**</span></span> | <span data-ttu-id="3f29b-506">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-506">No</span></span>          | <span data-ttu-id="3f29b-507">**Edm. Byte**, **Edm. Int16**, **Edm. Int32**, **Edm. Int64** ou **Edm. SByte** définissant la plage de valeurs du type.</span><span class="sxs-lookup"><span data-stu-id="3f29b-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="3f29b-508">  Le type sous-jacent par défaut des éléments d’énumération est **Edm. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-509">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **enumType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="3f29b-510">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-511">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-512">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-512">Example</span></span>

<span data-ttu-id="3f29b-513">L’exemple suivant montre un élément **enumType** avec trois éléments **membres** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="3f29b-514">Function, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-514">Function Element (CSDL)</span></span>

<span data-ttu-id="3f29b-515">L’élément de **fonction** en Conceptual Schema Definition Language (CSDL) est utilisé pour définir ou déclarer des fonctions dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="3f29b-516">Une fonction est définie à l’aide d’un élément DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="3f29b-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="3f29b-517">Un élément **Function** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-518">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-519">Paramètre (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="3f29b-520">DefiningExpression (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-521">ReturnType (Function) (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-522">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="3f29b-523">Un type de retour pour une fonction doit être spécifié avec l’élément **ReturnType** (Function) ou **ReturnType** (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="3f29b-524">Les types de retour possibles correspondent à tout type EdmSimpleType, type d'entité, type complexe, type de ligne ou type REF (ou à une collection de l'un de ces types).</span><span class="sxs-lookup"><span data-stu-id="3f29b-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-525">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-525">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-526">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Function** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="3f29b-527">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-527">Attribute Name</span></span> | <span data-ttu-id="3f29b-528">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-528">Is Required</span></span> | <span data-ttu-id="3f29b-529">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="3f29b-530">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-530">**Name**</span></span>       | <span data-ttu-id="3f29b-531">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-531">Yes</span></span>         | <span data-ttu-id="3f29b-532">Nom de la fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-532">The name of the function.</span></span>          |
| <span data-ttu-id="3f29b-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-533">**ReturnType**</span></span> | <span data-ttu-id="3f29b-534">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-534">No</span></span>          | <span data-ttu-id="3f29b-535">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-536">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **fonction** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="3f29b-537">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-538">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-539">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-539">Example</span></span>

<span data-ttu-id="3f29b-540">L’exemple suivant utilise un élément **Function** pour définir une fonction qui retourne le nombre d’années écoulées depuis l’embauche d’un formateur.</span><span class="sxs-lookup"><span data-stu-id="3f29b-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="3f29b-541">FunctionImport, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="3f29b-542">L’élément **FunctionImport** en Conceptual Schema Definition Language (CSDL) représente une fonction qui est définie dans la source de données, mais qui est disponible pour les objets via le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="3f29b-543">Par exemple, un élément Function dans le modèle de stockage peut être utilisé pour représenter une procédure stockée dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="3f29b-544">Un élément **FunctionImport** du modèle conceptuel représente la fonction correspondante dans une Entity Framework application et est mappée à la fonction de modèle de stockage à l’aide de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="3f29b-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="3f29b-545">Lorsque la fonction est appelée dans l'application, la procédure stockée correspondante est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="3f29b-546">L’élément **FunctionImport** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-547">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-548">Paramètre (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="3f29b-549">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="3f29b-550">ReturnType (FunctionImport) (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="3f29b-551">Un élément de **paramètre** doit être défini pour chaque paramètre que la fonction accepte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="3f29b-552">Un type de retour pour une fonction doit être spécifié avec l’élément **ReturnType** (FunctionImport) ou **ReturnType** (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="3f29b-553">La valeur du type de retour doit être une collection de type EDMSimpleType, d’EntityType ou de ComplexType.</span><span class="sxs-lookup"><span data-stu-id="3f29b-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-554">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-554">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-555">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="3f29b-556">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-556">Attribute Name</span></span>   | <span data-ttu-id="3f29b-557">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-557">Is Required</span></span> | <span data-ttu-id="3f29b-558">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-559">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-559">**Name**</span></span>         | <span data-ttu-id="3f29b-560">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-560">Yes</span></span>         | <span data-ttu-id="3f29b-561">Nom de la fonction importée.</span><span class="sxs-lookup"><span data-stu-id="3f29b-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="3f29b-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-562">**ReturnType**</span></span>   | <span data-ttu-id="3f29b-563">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-563">No</span></span>          | <span data-ttu-id="3f29b-564">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-564">The type that the function returns.</span></span> <span data-ttu-id="3f29b-565">N'utilisez pas cet attribut si la fonction ne retourne pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="3f29b-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="3f29b-566">Dans le cas contraire, la valeur doit être une collection de ComplexType, EntityType ou type EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="3f29b-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="3f29b-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3f29b-567">**EntitySet**</span></span>    | <span data-ttu-id="3f29b-568">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-568">No</span></span>          | <span data-ttu-id="3f29b-569">Si la fonction retourne une collection de types d’entités, la valeur de l' **EntitySet** doit être le jeu d’entités auquel la collection appartient.</span><span class="sxs-lookup"><span data-stu-id="3f29b-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="3f29b-570">Dans le cas contraire, l’attribut **EntitySet** ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="3f29b-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-571">**IsComposable**</span></span> | <span data-ttu-id="3f29b-572">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-572">No</span></span>          | <span data-ttu-id="3f29b-573">Si la valeur est définie sur true, la fonction est composable (fonction table) et peut être utilisée dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="3f29b-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="3f29b-574">  La valeur par défaut est **false**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="3f29b-575">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="3f29b-576">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-577">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-578">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-578">Example</span></span>

<span data-ttu-id="3f29b-579">L’exemple suivant montre un élément **FunctionImport** qui accepte un paramètre et retourne une collection de types d’entités :</span><span class="sxs-lookup"><span data-stu-id="3f29b-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="3f29b-580">Key, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-580">Key Element (CSDL)</span></span>

<span data-ttu-id="3f29b-581">L’élément **Key** est un élément enfant de l’élément EntityType et définit une *clé d’entité* (une propriété ou un ensemble de propriétés d’un type d’entité qui détermine l’identité).</span><span class="sxs-lookup"><span data-stu-id="3f29b-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="3f29b-582">Les propriétés qui composent une clé d'entité sont choisies au moment du design.</span><span class="sxs-lookup"><span data-stu-id="3f29b-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="3f29b-583">Les valeurs des propriétés de clé d'entité doivent identifier de façon unique une instance de type d'entité dans un jeu d'entités au moment de l'exécution.</span><span class="sxs-lookup"><span data-stu-id="3f29b-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="3f29b-584">Les propriétés qui composent une clé d'entité doivent être choisies pour garantir l'unicité des instances dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="3f29b-585">L’élément **Key** définit une clé d’entité en référençant une ou plusieurs des propriétés d’un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="3f29b-586">L’élément **Key** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="3f29b-587">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3f29b-588">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-589">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-589">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-590">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Key** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="3f29b-591">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-592">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-593">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-593">Example</span></span>

<span data-ttu-id="3f29b-594">L’exemple ci-dessous définit un type **d'** entité nommé Book.</span><span class="sxs-lookup"><span data-stu-id="3f29b-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="3f29b-595">La clé d’entité est définie en référençant la propriété **ISBN** du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="3f29b-596">La propriété **ISBN** est un bon choix pour la clé d’entité, car un numéro ISBN (International Standard Book Number) identifie de manière unique un livre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="3f29b-597">L’exemple suivant montre un type d’entité (**auteur**) qui a une clé d’entité composée de deux propriétés, **nom** et **adresse**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="3f29b-598">L’utilisation du **nom** et de l' **adresse** pour la clé d’entité est un choix raisonnable, car il est peu probable que deux auteurs du même nom vivent à la même adresse.</span><span class="sxs-lookup"><span data-stu-id="3f29b-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="3f29b-599">Toutefois, ce choix pour une clé d'entité ne garantit pas vraiment l'unicité des clés d'entité dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="3f29b-600">L’ajout d’une **propriété, telle**que la réutilisation, qui pourrait être utilisée pour identifier un auteur de manière unique, est recommandé dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="3f29b-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="3f29b-601">Member, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-601">Member Element (CSDL)</span></span>

<span data-ttu-id="3f29b-602">L’élément **member** est un élément enfant de l’élément enumType et définit un membre du type énuméré.</span><span class="sxs-lookup"><span data-stu-id="3f29b-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-603">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-603">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-604">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="3f29b-605">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-605">Attribute Name</span></span> | <span data-ttu-id="3f29b-606">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-606">Is Required</span></span> | <span data-ttu-id="3f29b-607">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-608">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-608">**Name**</span></span>       | <span data-ttu-id="3f29b-609">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-609">Yes</span></span>         | <span data-ttu-id="3f29b-610">Nom du membre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="3f29b-611">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="3f29b-611">**Value**</span></span>      | <span data-ttu-id="3f29b-612">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-612">No</span></span>          | <span data-ttu-id="3f29b-613">Valeur du membre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-613">The value of the member.</span></span> <span data-ttu-id="3f29b-614">Par défaut, le premier membre a la valeur 0, et la valeur de chaque énumérateur successif est incrémentée de 1.</span><span class="sxs-lookup"><span data-stu-id="3f29b-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="3f29b-615">Plusieurs membres ayant les mêmes valeurs peuvent exister.</span><span class="sxs-lookup"><span data-stu-id="3f29b-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-616">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="3f29b-617">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-618">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-619">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-619">Example</span></span>

<span data-ttu-id="3f29b-620">L’exemple suivant montre un élément **enumType** avec trois éléments **membres** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="3f29b-621">NavigationProperty, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="3f29b-622">Un élément **NavigationProperty** définit une propriété de navigation, qui fournit une référence à l’autre terminaison d’une association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="3f29b-623">Contrairement aux propriétés définies avec l’élément Property, les propriétés de navigation ne définissent pas la forme et les caractéristiques des données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="3f29b-624">Elles fournissent un moyen d'explorer une association entre deux types d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="3f29b-625">Notez que les propriétés de navigation sont facultatives sur les deux types d'entités au niveau des terminaisons d'une association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="3f29b-626">Si vous définissez une propriété de navigation sur un type d'entité au niveau de la terminaison d'une association, il n'est pas nécessaire de définir une propriété de navigation sur le type d'entité à l'autre terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="3f29b-627">Le type de données retourné par une propriété de navigation est déterminé par la multiplicité de sa terminaison d'association distante.</span><span class="sxs-lookup"><span data-stu-id="3f29b-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="3f29b-628">Par exemple, supposons qu’une propriété de navigation, **OrdersNavProp**, existe sur un type d’entité **Customer** et navigue vers une association un-à-plusieurs entre **Customer** et **Order**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="3f29b-629">Étant donné que la terminaison d’association distante pour la propriété de navigation a une multiplicité de plusieurs (\*), son type de données est une collection ( **ordre**).</span><span class="sxs-lookup"><span data-stu-id="3f29b-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="3f29b-630">De même, si une propriété de navigation, **CustomerNavProp**, existe sur le type d’entité **Order** , son type de données est **Customer** , car la multiplicité de la terminaison distante est un (1).</span><span class="sxs-lookup"><span data-stu-id="3f29b-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="3f29b-631">Un élément **NavigationProperty** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-632">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-633">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-634">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-634">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-635">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="3f29b-636">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-636">Attribute Name</span></span>   | <span data-ttu-id="3f29b-637">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-637">Is Required</span></span> | <span data-ttu-id="3f29b-638">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-639">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-639">**Name**</span></span>         | <span data-ttu-id="3f29b-640">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-640">Yes</span></span>         | <span data-ttu-id="3f29b-641">Nom de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="3f29b-642">**Relation**</span><span class="sxs-lookup"><span data-stu-id="3f29b-642">**Relationship**</span></span> | <span data-ttu-id="3f29b-643">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-643">Yes</span></span>         | <span data-ttu-id="3f29b-644">Nom d'une association figurant dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="3f29b-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="3f29b-645">**ToRole**</span></span>       | <span data-ttu-id="3f29b-646">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-646">Yes</span></span>         | <span data-ttu-id="3f29b-647">Terminaison de l'association à laquelle la navigation prend fin.</span><span class="sxs-lookup"><span data-stu-id="3f29b-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="3f29b-648">La valeur de l’attribut **ToRole** doit être identique à la valeur de l’un des attributs de **rôle** définis sur l’une des terminaisons d’Association (définie dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="3f29b-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="3f29b-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="3f29b-649">**FromRole**</span></span>     | <span data-ttu-id="3f29b-650">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-650">Yes</span></span>         | <span data-ttu-id="3f29b-651">Terminaison de l'association où la navigation commence.</span><span class="sxs-lookup"><span data-stu-id="3f29b-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="3f29b-652">La valeur de l’attribut **FromRole** doit être identique à la valeur de l’un des attributs de **rôle** définis sur l’une des terminaisons d’Association (définie dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="3f29b-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-653">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="3f29b-654">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-655">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-656">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-656">Example</span></span>

<span data-ttu-id="3f29b-657">L’exemple suivant définit un type d’entité (**Book**) avec deux propriétés de navigation (**PublishedBy** et **WrittenBy**) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="3f29b-658">OnDelete, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="3f29b-659">L’élément **OnDelete** en Conceptual Schema Definition Language (CSDL) définit le comportement qui est connecté avec une association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="3f29b-660">Si l’attribut **action** est défini sur **cascade** à une terminaison d’une association, les types d’entités associés à l’autre terminaison de l’Association sont supprimés lorsque le type d’entité de la première terminaison est supprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="3f29b-661">Si l’association entre deux types d’entités est une relation clé primaire-clé primaire, un objet dépendant chargé est supprimé lorsque l’objet principal de l’autre terminaison de l’Association est supprimé, quelle que soit la spécification de **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="3f29b-662">L’élément **OnDelete** affecte uniquement le comportement au moment de l’exécution d’une application ; elle n’affecte pas le comportement dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="3f29b-663">Le comportement défini dans la source de données doit être le même que le comportement défini dans l'application.</span><span class="sxs-lookup"><span data-stu-id="3f29b-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="3f29b-664">Un élément **OnDelete** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-665">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-666">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-667">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-667">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-668">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="3f29b-669">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-669">Attribute Name</span></span> | <span data-ttu-id="3f29b-670">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-670">Is Required</span></span> | <span data-ttu-id="3f29b-671">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-672">**Action**</span><span class="sxs-lookup"><span data-stu-id="3f29b-672">**Action**</span></span>     | <span data-ttu-id="3f29b-673">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-673">Yes</span></span>         | <span data-ttu-id="3f29b-674">**Cascade** ou **None**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-674">**Cascade** or **None**.</span></span> <span data-ttu-id="3f29b-675">Si vous disposez d’une **cascade**, les types d’entités dépendants sont supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="3f29b-676">Si **aucune**valeur n’est, les types d’entités dépendants ne sont pas supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-677">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="3f29b-678">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-679">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-680">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-680">Example</span></span>

<span data-ttu-id="3f29b-681">L’exemple suivant montre un élément **Association** qui définit l’Association **customerOrders** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="3f29b-682">L’élément **OnDelete** indique que toutes les **commandes** associées à un **client** particulier et qui ont été chargées dans ObjectContext seront supprimées lors de la suppression du **client** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="3f29b-683">Parameter, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="3f29b-684">L’élément **Parameter** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément FunctionImport ou de l’élément Function.</span><span class="sxs-lookup"><span data-stu-id="3f29b-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="3f29b-685">Application de l'élément FunctionImport</span><span class="sxs-lookup"><span data-stu-id="3f29b-685">FunctionImport Element Application</span></span>

<span data-ttu-id="3f29b-686">Un élément **Parameter** (en tant qu’enfant de l’élément **FunctionImport** ) est utilisé pour définir les paramètres d’entrée et de sortie des importations de fonctions déclarées dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="3f29b-687">L’élément **Parameter** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-688">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-689">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-690">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-690">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-691">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="3f29b-692">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-692">Attribute Name</span></span> | <span data-ttu-id="3f29b-693">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-693">Is Required</span></span> | <span data-ttu-id="3f29b-694">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-695">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-695">**Name**</span></span>       | <span data-ttu-id="3f29b-696">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-696">Yes</span></span>         | <span data-ttu-id="3f29b-697">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3f29b-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-698">**Type**</span></span>       | <span data-ttu-id="3f29b-699">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-699">Yes</span></span>         | <span data-ttu-id="3f29b-700">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-700">The parameter type.</span></span> <span data-ttu-id="3f29b-701">La valeur doit être un **type EDMSimpleType** ou un type complexe qui se trouve dans l’étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="3f29b-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-702">**Mode**</span></span>       | <span data-ttu-id="3f29b-703">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-703">No</span></span>          | <span data-ttu-id="3f29b-704">**In**, **out**ou **INOUT** selon que le paramètre est un paramètre d’entrée, de sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="3f29b-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="3f29b-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-705">**MaxLength**</span></span>  | <span data-ttu-id="3f29b-706">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-706">No</span></span>          | <span data-ttu-id="3f29b-707">Longueur maximale autorisée du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="3f29b-708">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-708">**Precision**</span></span>  | <span data-ttu-id="3f29b-709">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-709">No</span></span>          | <span data-ttu-id="3f29b-710">Précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-711">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-711">**Scale**</span></span>      | <span data-ttu-id="3f29b-712">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-712">No</span></span>          | <span data-ttu-id="3f29b-713">Échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="3f29b-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-714">**SRID**</span></span>       | <span data-ttu-id="3f29b-715">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-715">No</span></span>          | <span data-ttu-id="3f29b-716">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-717">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="3f29b-718">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-719">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="3f29b-720">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-721">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-722">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-722">Example</span></span>

<span data-ttu-id="3f29b-723">L’exemple suivant montre un élément **FunctionImport** avec un élément enfant **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="3f29b-724">La fonction accepte un paramètre d'entrée et retourne une collection de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="3f29b-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="3f29b-725">Application de l'élément Function</span><span class="sxs-lookup"><span data-stu-id="3f29b-725">Function Element Application</span></span>

<span data-ttu-id="3f29b-726">Un élément **Parameter** (en tant qu’enfant de l’élément **Function** ) définit des paramètres pour les fonctions qui sont définies ou déclarées dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="3f29b-727">L’élément **Parameter** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-728">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="3f29b-729">CollectionType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="3f29b-730">ReferenceType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="3f29b-731">RowType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-732">Seul l’un des éléments **CollectionType**, **ReferenceType**ou **RowType** peut être un élément enfant d’un élément de **propriété** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="3f29b-733">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-734">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="3f29b-735">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3f29b-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-736">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-736">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-737">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="3f29b-738">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-738">Attribute Name</span></span>   | <span data-ttu-id="3f29b-739">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-739">Is Required</span></span> | <span data-ttu-id="3f29b-740">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-741">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-741">**Name**</span></span>         | <span data-ttu-id="3f29b-742">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-742">Yes</span></span>         | <span data-ttu-id="3f29b-743">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3f29b-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-744">**Type**</span></span>         | <span data-ttu-id="3f29b-745">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-745">No</span></span>          | <span data-ttu-id="3f29b-746">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f29b-746">The parameter type.</span></span> <span data-ttu-id="3f29b-747">Un paramètre peut correspondre à l'un quelconque des types suivants (ou à des collections de ces types) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="3f29b-748">**Type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="3f29b-749">type d'entité</span><span class="sxs-lookup"><span data-stu-id="3f29b-749">entity type</span></span> <br/> <span data-ttu-id="3f29b-750">type complexe</span><span class="sxs-lookup"><span data-stu-id="3f29b-750">complex type</span></span> <br/> <span data-ttu-id="3f29b-751">type de ligne</span><span class="sxs-lookup"><span data-stu-id="3f29b-751">row type</span></span> <br/> <span data-ttu-id="3f29b-752">type référence</span><span class="sxs-lookup"><span data-stu-id="3f29b-752">reference type</span></span>                             |
| <span data-ttu-id="3f29b-753">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-753">**Nullable**</span></span>     | <span data-ttu-id="3f29b-754">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-754">No</span></span>          | <span data-ttu-id="3f29b-755">**True** (valeur par défaut) ou **false** selon que la propriété peut avoir une valeur **null** ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="3f29b-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3f29b-756">**DefaultValue**</span></span> | <span data-ttu-id="3f29b-757">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-757">No</span></span>          | <span data-ttu-id="3f29b-758">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3f29b-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-759">**MaxLength**</span></span>    | <span data-ttu-id="3f29b-760">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-760">No</span></span>          | <span data-ttu-id="3f29b-761">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-762">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-762">**FixedLength**</span></span>  | <span data-ttu-id="3f29b-763">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-763">No</span></span>          | <span data-ttu-id="3f29b-764">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3f29b-765">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-765">**Precision**</span></span>    | <span data-ttu-id="3f29b-766">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-766">No</span></span>          | <span data-ttu-id="3f29b-767">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3f29b-768">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-768">**Scale**</span></span>        | <span data-ttu-id="3f29b-769">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-769">No</span></span>          | <span data-ttu-id="3f29b-770">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3f29b-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-771">**SRID**</span></span>         | <span data-ttu-id="3f29b-772">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-772">No</span></span>          | <span data-ttu-id="3f29b-773">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-774">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3f29b-775">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3f29b-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-776">**Unicode**</span></span>      | <span data-ttu-id="3f29b-777">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-777">No</span></span>          | <span data-ttu-id="3f29b-778">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3f29b-779">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-779">**Collation**</span></span>    | <span data-ttu-id="3f29b-780">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-780">No</span></span>          | <span data-ttu-id="3f29b-781">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3f29b-782">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="3f29b-783">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-784">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-785">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-785">Example</span></span>

<span data-ttu-id="3f29b-786">L’exemple suivant montre un élément **Function** qui utilise un élément enfant **Parameter** pour définir un paramètre de fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="3f29b-787">Principal, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="3f29b-788">L’élément **principal** en Conceptual Schema Definition Language (CSDL) est un élément enfant de l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="3f29b-789">Un élément **ReferentialConstraint** définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3f29b-790">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3f29b-791">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3f29b-792">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="3f29b-793">Les éléments **PropertyRef** sont utilisés pour spécifier les clés qui sont référencées par la terminaison dépendante.</span><span class="sxs-lookup"><span data-stu-id="3f29b-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="3f29b-794">L’élément **principal** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-795">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3f29b-796">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-797">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-797">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-798">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **principal** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="3f29b-799">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-799">Attribute Name</span></span> | <span data-ttu-id="3f29b-800">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-800">Is Required</span></span> | <span data-ttu-id="3f29b-801">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="3f29b-802">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-802">**Role**</span></span>       | <span data-ttu-id="3f29b-803">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-803">Yes</span></span>         | <span data-ttu-id="3f29b-804">Nom du type d'entité au niveau de la terminaison principale de l'association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-805">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **principal** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="3f29b-806">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-807">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-808">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-808">Example</span></span>

<span data-ttu-id="3f29b-809">L’exemple suivant montre un élément **ReferentialConstraint** qui fait partie de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="3f29b-810">La propriété **ID** du type d’entité du serveur de **publication** compose la terminaison principale de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="3f29b-811">Property, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-811">Property Element (CSDL)</span></span>

<span data-ttu-id="3f29b-812">L’élément de **propriété** en Conceptual Schema Definition Language (CSDL) peut être un enfant de l’élément EntityType, de l’élément complexType ou de l’élément RowType.</span><span class="sxs-lookup"><span data-stu-id="3f29b-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="3f29b-813">Applications des éléments EntityType et ComplexType</span><span class="sxs-lookup"><span data-stu-id="3f29b-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="3f29b-814">Les éléments de **propriété** (en tant qu’enfants des éléments **EntityType** ou **complexType** ) définissent la forme et les caractéristiques des données qu’une instance de type d’entité ou une instance de type complexe contiendra.</span><span class="sxs-lookup"><span data-stu-id="3f29b-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="3f29b-815">Les propriétés dans un modèle conceptuel sont analogues aux propriétés qui sont définies sur une classe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="3f29b-816">De même que les propriétés sur une classe définissent la forme de la classe et acheminent des informations sur les objets, les propriétés dans un modèle conceptuel définissent la forme d'un type d'entité et acheminent des informations sur les instances de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="3f29b-817">L’élément **Property** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-818">Élément documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-819">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="3f29b-820">Les facettes suivantes peuvent être appliquées à un élément **Property** : **Nullable**, **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**, **collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="3f29b-821">Les facettes sont des attributs XML qui fournissent des informations sur la manière dont les valeurs de propriété sont stockées dans la banque de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-822">Les facettes ne peuvent être appliquées qu’à des propriétés de type **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-823">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-823">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-824">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="3f29b-825">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-825">Attribute Name</span></span>                                                         | <span data-ttu-id="3f29b-826">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-826">Is Required</span></span> | <span data-ttu-id="3f29b-827">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-828">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-828">**Name**</span></span>                                                               | <span data-ttu-id="3f29b-829">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-829">Yes</span></span>         | <span data-ttu-id="3f29b-830">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-831">**Type**</span></span>                                                               | <span data-ttu-id="3f29b-832">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-832">Yes</span></span>         | <span data-ttu-id="3f29b-833">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-833">The type of the property value.</span></span> <span data-ttu-id="3f29b-834">Le type de valeur de propriété doit être un **type EDMSimpleType** ou un type complexe (indiqué par un nom qualifié complet) qui se trouve dans l’étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="3f29b-835">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-835">**Nullable**</span></span>                                                           | <span data-ttu-id="3f29b-836">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-836">No</span></span>          | <span data-ttu-id="3f29b-837">**True** (valeur par défaut) ou <strong>false</strong> selon que la propriété peut avoir une valeur null ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="3f29b-838">> Dans le langage CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3f29b-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="3f29b-840">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-840">No</span></span>          | <span data-ttu-id="3f29b-841">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3f29b-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="3f29b-843">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-843">No</span></span>          | <span data-ttu-id="3f29b-844">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-845">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="3f29b-846">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-846">No</span></span>          | <span data-ttu-id="3f29b-847">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3f29b-848">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-848">**Precision**</span></span>                                                          | <span data-ttu-id="3f29b-849">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-849">No</span></span>          | <span data-ttu-id="3f29b-850">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3f29b-851">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-851">**Scale**</span></span>                                                              | <span data-ttu-id="3f29b-852">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-852">No</span></span>          | <span data-ttu-id="3f29b-853">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3f29b-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-854">**SRID**</span></span>                                                               | <span data-ttu-id="3f29b-855">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-855">No</span></span>          | <span data-ttu-id="3f29b-856">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-857">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3f29b-858">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3f29b-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-859">**Unicode**</span></span>                                                            | <span data-ttu-id="3f29b-860">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-860">No</span></span>          | <span data-ttu-id="3f29b-861">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3f29b-862">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-862">**Collation**</span></span>                                                          | <span data-ttu-id="3f29b-863">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-863">No</span></span>          | <span data-ttu-id="3f29b-864">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="3f29b-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="3f29b-866">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-866">No</span></span>          | <span data-ttu-id="3f29b-867">**None** (valeur par défaut) ou **fixed**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="3f29b-868">Si la valeur est définie sur **fixed**, la valeur de la propriété sera utilisée dans les contrôles d’accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="3f29b-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="3f29b-869">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="3f29b-870">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-871">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-872">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-872">Example</span></span>

<span data-ttu-id="3f29b-873">L’exemple suivant montre un élément **EntityType** avec trois éléments **Property** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="3f29b-874">L’exemple suivant montre un élément **complexType** avec cinq éléments de **propriété** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="3f29b-875">Application de l'élément RowType</span><span class="sxs-lookup"><span data-stu-id="3f29b-875">RowType Element Application</span></span>

<span data-ttu-id="3f29b-876">Les éléments de **propriété** (comme les enfants d’un élément **RowType** ) définissent la forme et les caractéristiques des données qui peuvent être passées ou retournées à partir d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="3f29b-877">L’élément **Property** peut avoir exactement l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="3f29b-878">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-878">CollectionType</span></span>
-   <span data-ttu-id="3f29b-879">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-879">ReferenceType</span></span>
-   <span data-ttu-id="3f29b-880">RowType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-880">RowType</span></span>

<span data-ttu-id="3f29b-881">L’élément **Property** peut avoir n’importe quel nombre d’éléments d’annotation enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-882">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3f29b-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-883">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-883">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-884">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="3f29b-885">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-885">Attribute Name</span></span>                                                     | <span data-ttu-id="3f29b-886">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-886">Is Required</span></span> | <span data-ttu-id="3f29b-887">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-888">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-888">**Name**</span></span>                                                           | <span data-ttu-id="3f29b-889">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-889">Yes</span></span>         | <span data-ttu-id="3f29b-890">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-891">**Type**</span></span>                                                           | <span data-ttu-id="3f29b-892">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-892">Yes</span></span>         | <span data-ttu-id="3f29b-893">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-894">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-894">**Nullable**</span></span>                                                       | <span data-ttu-id="3f29b-895">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-895">No</span></span>          | <span data-ttu-id="3f29b-896">**True** (valeur par défaut) ou **false** selon que la propriété peut avoir une valeur null ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="3f29b-897">> Dans CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3f29b-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="3f29b-899">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-899">No</span></span>          | <span data-ttu-id="3f29b-900">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3f29b-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="3f29b-902">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-902">No</span></span>          | <span data-ttu-id="3f29b-903">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-904">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="3f29b-905">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-905">No</span></span>          | <span data-ttu-id="3f29b-906">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3f29b-907">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-907">**Precision**</span></span>                                                      | <span data-ttu-id="3f29b-908">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-908">No</span></span>          | <span data-ttu-id="3f29b-909">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3f29b-910">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-910">**Scale**</span></span>                                                          | <span data-ttu-id="3f29b-911">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-911">No</span></span>          | <span data-ttu-id="3f29b-912">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3f29b-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-913">**SRID**</span></span>                                                           | <span data-ttu-id="3f29b-914">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-914">No</span></span>          | <span data-ttu-id="3f29b-915">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-916">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3f29b-917">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3f29b-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-918">**Unicode**</span></span>                                                        | <span data-ttu-id="3f29b-919">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-919">No</span></span>          | <span data-ttu-id="3f29b-920">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3f29b-921">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-921">**Collation**</span></span>                                                      | <span data-ttu-id="3f29b-922">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-922">No</span></span>          | <span data-ttu-id="3f29b-923">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3f29b-924">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **Property** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="3f29b-925">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-926">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3f29b-927">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-927">Example</span></span>

<span data-ttu-id="3f29b-928">L’exemple suivant montre les éléments de **propriété** utilisés pour définir la forme du type de retour d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="3f29b-929">PropertyRef, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="3f29b-930">L’élément **PropertyRef** en Conceptual Schema Definition Language (CSDL) fait référence à une propriété d’un type d’entité pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="3f29b-931">Partie de la clé de l'entité (une propriété ou un jeu de propriétés d'un type d'entité qui détermine l'identité).</span><span class="sxs-lookup"><span data-stu-id="3f29b-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="3f29b-932">Un ou plusieurs éléments **PropertyRef** peuvent être utilisés pour définir une clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="3f29b-933">Terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="3f29b-934">L’élément **PropertyRef** peut uniquement comporter des éléments d’annotation (zéro ou plus) en tant qu’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-935">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3f29b-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-936">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-936">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-937">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="3f29b-938">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-938">Attribute Name</span></span> | <span data-ttu-id="3f29b-939">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-939">Is Required</span></span> | <span data-ttu-id="3f29b-940">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="3f29b-941">**Nom**</span><span class="sxs-lookup"><span data-stu-id="3f29b-941">**Name**</span></span>       | <span data-ttu-id="3f29b-942">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-942">Yes</span></span>         | <span data-ttu-id="3f29b-943">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="3f29b-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-944">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="3f29b-945">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-946">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-947">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-947">Example</span></span>

<span data-ttu-id="3f29b-948">L’exemple ci-dessous définit un type**d'** entité (Book).</span><span class="sxs-lookup"><span data-stu-id="3f29b-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="3f29b-949">La clé d’entité est définie en référençant la propriété **ISBN** du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="3f29b-950">Dans l’exemple suivant, deux éléments **PropertyRef** sont utilisés pour indiquer que deux propriétés (**ID** et **PublisherId**) sont les terminaisons principale et dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="3f29b-951">Élément ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-952">L’élément **ReferenceType** en Conceptual Schema Definition Language (CSDL) spécifie une référence à un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="3f29b-953">L’élément **ReferenceType** peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="3f29b-954">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="3f29b-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="3f29b-955">Paramètre</span><span class="sxs-lookup"><span data-stu-id="3f29b-955">Parameter</span></span>
-   <span data-ttu-id="3f29b-956">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-956">CollectionType</span></span>

<span data-ttu-id="3f29b-957">L’élément **ReferenceType** est utilisé lors de la définition d’un paramètre ou d’un type de retour pour une fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="3f29b-958">Un élément **ReferenceType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-959">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-960">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-961">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-961">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-962">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="3f29b-963">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-963">Attribute Name</span></span> | <span data-ttu-id="3f29b-964">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-964">Is Required</span></span> | <span data-ttu-id="3f29b-965">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="3f29b-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-966">**Type**</span></span>       | <span data-ttu-id="3f29b-967">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-967">Yes</span></span>         | <span data-ttu-id="3f29b-968">Nom du type d'entité référencé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-969">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="3f29b-970">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-971">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-972">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-972">Example</span></span>

<span data-ttu-id="3f29b-973">L’exemple suivant montre l’élément **ReferenceType** utilisé comme enfant d’un élément **Parameter** dans une fonction définie par modèle qui accepte une référence à un type d’entité **Person** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="3f29b-974">L’exemple suivant montre l’élément **ReferenceType** utilisé comme enfant d’un élément **ReturnType** (Function) dans une fonction définie par modèle qui retourne une référence à un type d’entité **Person** :</span><span class="sxs-lookup"><span data-stu-id="3f29b-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="3f29b-975">ReferentialConstraint, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="3f29b-976">Un élément **ReferentialConstraint** en Conceptual Schema Definition Language (CSDL) définit des fonctionnalités qui sont semblables à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="3f29b-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3f29b-977">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3f29b-978">Le type d’entité référencé est appelé *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3f29b-979">Le type d’entité qui référence la terminaison principale est appelé *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="3f29b-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="3f29b-980">Si une clé étrangère exposée sur un type d’entité fait référence à une propriété sur un autre type d’entité, l’élément **ReferentialConstraint** définit une association entre les deux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="3f29b-981">Étant donné que l’élément **ReferentialConstraint** fournit des informations sur la façon dont deux types d’entité sont associés, aucun élément AssociationSetMapping correspondant n’est nécessaire dans le Mapping Specification Language (MSL).</span><span class="sxs-lookup"><span data-stu-id="3f29b-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="3f29b-982">Une association entre deux types d’entités qui n’ont pas de clés étrangères exposées doit avoir un élément **AssociationSetMapping** correspondant pour mapper les informations d’association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="3f29b-983">Si une clé étrangère n’est pas exposée sur un type d’entité, l’élément **ReferentialConstraint** ne peut définir qu’une contrainte de clé primaire-clé primaire entre le type d’entité et un autre type d’entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="3f29b-984">Un élément **ReferentialConstraint** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-985">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-986">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="3f29b-987">Dependent (un seul élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="3f29b-988">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-989">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-989">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-990">L’élément **ReferentialConstraint** peut avoir n’importe quel nombre d’attributs d’annotation (attributs XML personnalisés).</span><span class="sxs-lookup"><span data-stu-id="3f29b-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="3f29b-991">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-992">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-993">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-993">Example</span></span>

<span data-ttu-id="3f29b-994">L’exemple suivant montre un élément **ReferentialConstraint** utilisé dans le cadre de la définition de l’Association **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="3f29b-995">ReturnType (Function), élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="3f29b-996">L’élément **ReturnType** (Function) en Conceptual Schema Definition Language (CSDL) spécifie le type de retour pour une fonction définie dans un élément Function.</span><span class="sxs-lookup"><span data-stu-id="3f29b-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="3f29b-997">Un type de retour de fonction peut également être spécifié avec un attribut **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="3f29b-998">Les types de retour peuvent être n’importe quel **type EDMSimpleType**, type d’entité, type complexe, type de ligne, type ref ou une collection de l’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="3f29b-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="3f29b-999">Le type de retour d’une fonction peut être spécifié avec l’attribut de **type** de l’élément **ReturnType** (Function), ou avec l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="3f29b-1000">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-1000">CollectionType</span></span>
-   <span data-ttu-id="3f29b-1001">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-1001">ReferenceType</span></span>
-   <span data-ttu-id="3f29b-1002">RowType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-1003">Un modèle ne sera pas validé si vous spécifiez un type de retour de fonction avec à la fois l’attribut de **type** de l’élément **ReturnType** (Function) et l’un des éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1004">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1004">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1005">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="3f29b-1006">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-1006">Attribute Name</span></span> | <span data-ttu-id="3f29b-1007">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-1007">Is Required</span></span> | <span data-ttu-id="3f29b-1008">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="3f29b-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1009">**ReturnType**</span></span> | <span data-ttu-id="3f29b-1010">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1010">No</span></span>          | <span data-ttu-id="3f29b-1011">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-1012">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="3f29b-1013">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1014">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-1015">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1015">Example</span></span>

<span data-ttu-id="3f29b-1016">L’exemple suivant utilise un élément **Function** pour définir une fonction qui retourne le nombre d’années pendant lequel un livre a été imprimé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="3f29b-1017">Notez que le type de retour est spécifié par l’attribut **type** d’un élément **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="3f29b-1018">ReturnType (FunctionImport), élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="3f29b-1019">L’élément **ReturnType** (FunctionImport) de Conceptual Schema Definition Language (CSDL) spécifie le type de retour pour une fonction définie dans un élément FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="3f29b-1020">Un type de retour de fonction peut également être spécifié avec un attribut **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="3f29b-1021">Les types de retour peuvent être n’importe quelle collection de type d’entité, de type complexe ou de **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="3f29b-1022">Le type de retour d’une fonction est spécifié avec l’attribut de **type** de l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1023">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1023">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1024">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="3f29b-1025">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-1025">Attribute Name</span></span> | <span data-ttu-id="3f29b-1026">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-1026">Is Required</span></span> | <span data-ttu-id="3f29b-1027">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1028">**Type**</span></span>       | <span data-ttu-id="3f29b-1029">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1029">No</span></span>          | <span data-ttu-id="3f29b-1030">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1030">The type that the function returns.</span></span> <span data-ttu-id="3f29b-1031">La valeur doit être une collection de ComplexType, EntityType ou type EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="3f29b-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1032">**EntitySet**</span></span>  | <span data-ttu-id="3f29b-1033">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1033">No</span></span>          | <span data-ttu-id="3f29b-1034">Si la fonction retourne une collection de types d’entités, la valeur de l' **EntitySet** doit être le jeu d’entités auquel la collection appartient.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="3f29b-1035">Dans le cas contraire, l’attribut **EntitySet** ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-1036">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="3f29b-1037">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1038">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-1039">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1039">Example</span></span>

<span data-ttu-id="3f29b-1040">L’exemple suivant utilise un **FunctionImport** qui retourne des livres et des serveurs de publication.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="3f29b-1041">Notez que la fonction retourne deux jeux de résultats et, par conséquent, deux éléments **ReturnType** (FunctionImport) sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="3f29b-1042">Élément RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="3f29b-1043">Un élément **RowType** en Conceptual Schema Definition Language (CSDL) définit une structure sans nom en tant que paramètre ou type de retour pour une fonction définie dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="3f29b-1044">Un élément **RowType** peut être l’enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="3f29b-1045">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="3f29b-1045">CollectionType</span></span>
-   <span data-ttu-id="3f29b-1046">Paramètre</span><span class="sxs-lookup"><span data-stu-id="3f29b-1046">Parameter</span></span>
-   <span data-ttu-id="3f29b-1047">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1047">ReturnType (Function)</span></span>

<span data-ttu-id="3f29b-1048">Un élément **RowType** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-1049">Property (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1049">Property (one or more)</span></span>
-   <span data-ttu-id="3f29b-1050">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1051">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1051">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1052">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **RowType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="3f29b-1053">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1054">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-1055">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1055">Example</span></span>

<span data-ttu-id="3f29b-1056">L’exemple suivant montre une fonction définie par modèle qui utilise un élément **CollectionType** pour spécifier que la fonction retourne une collection de lignes (comme spécifié dans l’élément **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="3f29b-1057">Schema, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="3f29b-1058">L’élément **Schema** est l’élément racine d’une définition de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="3f29b-1059">Il contient des définitions pour les objets, fonctions et conteneurs qui composent un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="3f29b-1060">L’élément **Schema** peut contenir zéro, un ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="3f29b-1061">Using</span><span class="sxs-lookup"><span data-stu-id="3f29b-1061">Using</span></span>
-   <span data-ttu-id="3f29b-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="3f29b-1062">EntityContainer</span></span>
-   <span data-ttu-id="3f29b-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="3f29b-1063">EntityType</span></span>
-   <span data-ttu-id="3f29b-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="3f29b-1064">EnumType</span></span>
-   <span data-ttu-id="3f29b-1065">Association</span><span class="sxs-lookup"><span data-stu-id="3f29b-1065">Association</span></span>
-   <span data-ttu-id="3f29b-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="3f29b-1066">ComplexType</span></span>
-   <span data-ttu-id="3f29b-1067">Fonction</span><span class="sxs-lookup"><span data-stu-id="3f29b-1067">Function</span></span>

<span data-ttu-id="3f29b-1068">Un élément de **schéma** peut contenir zéro ou un élément d’annotation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-1069">L’élément de **fonction** et les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="3f29b-1070">L’élément **Schema** utilise l’attribut **namespace** pour définir l’espace de noms pour le type d’entité, le type complexe et les objets Association dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="3f29b-1071">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="3f29b-1072">Les espaces de noms peuvent s’étendre sur plusieurs éléments de **schéma** et plusieurs fichiers. csdl.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="3f29b-1073">Un espace de noms de modèle conceptuel est différent de l’espace de noms XML de l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="3f29b-1074">Un espace de noms de modèle conceptuel (tel que défini par l’attribut **namespace** ) est un conteneur logique pour les types d’entité, les types complexes et les types d’association.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="3f29b-1075">L’espace de noms XML (indiqué par l’attribut **xmlns** ) d’un élément de **schéma** est l’espace de noms par défaut pour les éléments enfants et les attributs de l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="3f29b-1076">Les espaces de noms XML de la forme https://schemas.microsoft.com/ado/YYYY/MM/edm (où aaaa et MM représentent respectivement une année et un mois) sont réservés au langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1077">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1078">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1078">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1079">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Schema** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="3f29b-1080">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-1080">Attribute Name</span></span> | <span data-ttu-id="3f29b-1081">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-1081">Is Required</span></span> | <span data-ttu-id="3f29b-1082">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-1083">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1083">**Namespace**</span></span>  | <span data-ttu-id="3f29b-1084">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1084">Yes</span></span>         | <span data-ttu-id="3f29b-1085">Espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="3f29b-1086">La valeur de l’attribut d' **espace de noms** est utilisée pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="3f29b-1087">Par exemple, si un **EntityType** nommé *Customer* est dans l’espace de noms simple. example. Model, le nom qualifié complet de l' **EntityType** est SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="3f29b-1088">Les chaînes suivantes ne peuvent pas être utilisées comme valeur pour l’attribut d' **espace de noms** : **Système**, **transitoire**ou **EDM**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="3f29b-1089">La valeur de l’attribut d' **espace de noms** ne peut pas être la même que la valeur de l’attribut d' **espace de noms** dans l’élément de schéma SSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="3f29b-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1090">**Alias**</span></span>      | <span data-ttu-id="3f29b-1091">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1091">No</span></span>          | <span data-ttu-id="3f29b-1092">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="3f29b-1093">Par exemple, si un **EntityType** nommé *Customer* est dans l’espace de noms simple. example. Model et que la valeur de l’attribut **alias** est *Model*, vous pouvez utiliser Model. Customer comme nom qualifié complet de l' **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="3f29b-1094">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément de **schéma** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="3f29b-1095">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1096">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-1097">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1097">Example</span></span>

<span data-ttu-id="3f29b-1098">L’exemple suivant illustre un élément de **schéma** qui contient un élément **EntityContainer** , deux éléments **EntityType** et un élément **Association** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="3f29b-1099">Élément TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="3f29b-1100">L’élément **TypeRef** en Conceptual Schema Definition Language (CSDL) fournit une référence à un type nommé existant.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="3f29b-1101">L’élément **TypeRef** peut être un enfant de l’élément CollectionType, qui est utilisé pour spécifier qu’une fonction a une collection en tant que paramètre ou type de retour.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="3f29b-1102">Un élément **TypeRef** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3f29b-1103">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3f29b-1104">Éléments d’annotation (zéro, un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1105">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1105">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1106">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="3f29b-1107">Notez que les attributs **DefaultValue**, **MaxLength**, **multiple**, **PRECISION**, **Scale**, **Unicode**et **collation** sont uniquement applicables à **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="3f29b-1108">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="3f29b-1109">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-1109">Is Required</span></span> | <span data-ttu-id="3f29b-1110">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1111">**Type**</span></span>                                                           | <span data-ttu-id="3f29b-1112">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1112">No</span></span>          | <span data-ttu-id="3f29b-1113">Nom du type référencé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="3f29b-1114">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="3f29b-1115">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1115">No</span></span>          | <span data-ttu-id="3f29b-1116">**True** (valeur par défaut) ou **false** selon que la propriété peut avoir une valeur null ou non.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="3f29b-1117">> Dans CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3f29b-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="3f29b-1119">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1119">No</span></span>          | <span data-ttu-id="3f29b-1120">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3f29b-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="3f29b-1122">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1122">No</span></span>          | <span data-ttu-id="3f29b-1123">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3f29b-1124">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="3f29b-1125">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1125">No</span></span>          | <span data-ttu-id="3f29b-1126">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3f29b-1127">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1127">**Precision**</span></span>                                                      | <span data-ttu-id="3f29b-1128">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1128">No</span></span>          | <span data-ttu-id="3f29b-1129">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3f29b-1130">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1130">**Scale**</span></span>                                                          | <span data-ttu-id="3f29b-1131">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1131">No</span></span>          | <span data-ttu-id="3f29b-1132">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3f29b-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1133">**SRID**</span></span>                                                           | <span data-ttu-id="3f29b-1134">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1134">No</span></span>          | <span data-ttu-id="3f29b-1135">Identificateur de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3f29b-1136">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3f29b-1137">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3f29b-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="3f29b-1139">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1139">No</span></span>          | <span data-ttu-id="3f29b-1140">**True** ou **false** selon que la valeur de la propriété sera stockée ou non comme une chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3f29b-1141">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1141">**Collation**</span></span>                                                      | <span data-ttu-id="3f29b-1142">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1142">No</span></span>          | <span data-ttu-id="3f29b-1143">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3f29b-1144">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3f29b-1145">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1146">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-1147">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1147">Example</span></span>

<span data-ttu-id="3f29b-1148">L’exemple suivant montre une fonction définie par modèle qui utilise l’élément **TypeRef** (en tant qu’enfant d’un élément **CollectionType** ) pour spécifier que la fonction accepte une collection de types d’entités **Department** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="3f29b-1149">Using, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="3f29b-1150">L’élément **using** de Conceptual Schema Definition Language (CSDL) importe le contenu d’un modèle conceptuel qui existe dans un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="3f29b-1151">En définissant la valeur de l’attribut d' **espace de noms** , vous pouvez faire référence à des types d’entités, des types complexes et des types d’associations définis dans un autre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="3f29b-1152">Plus d’un élément **using** peut être un enfant d’un élément **Schema** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-1153">L’élément **using** dans CSDL ne fonctionne pas exactement comme une instruction **using** dans un langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="3f29b-1154">En important un espace de noms avec une instruction **using** dans un langage de programmation, vous n’affectez pas les objets de l’espace de noms d’origine.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="3f29b-1155">Dans le langage CSDL, un espace de noms importé peut contenir un type d'entité dérivé d'un type d'entité figurant dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="3f29b-1156">Cela peut affecter les jeux d'entités déclarés dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="3f29b-1157">L’élément **using** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="3f29b-1158">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3f29b-1159">Éléments d’annotation (zéro, un ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3f29b-1160">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1160">Applicable Attributes</span></span>

<span data-ttu-id="3f29b-1161">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="3f29b-1162">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="3f29b-1162">Attribute Name</span></span> | <span data-ttu-id="3f29b-1163">Requis</span><span class="sxs-lookup"><span data-stu-id="3f29b-1163">Is Required</span></span> | <span data-ttu-id="3f29b-1164">Value</span><span class="sxs-lookup"><span data-stu-id="3f29b-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-1165">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1165">**Namespace**</span></span>  | <span data-ttu-id="3f29b-1166">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1166">Yes</span></span>         | <span data-ttu-id="3f29b-1167">Nom de l'espace de noms importé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="3f29b-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1168">**Alias**</span></span>      | <span data-ttu-id="3f29b-1169">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1169">Yes</span></span>         | <span data-ttu-id="3f29b-1170">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="3f29b-1171">Bien que cet attribut soit obligatoire, il n'est pas nécessaire qu'il soit utilisé à la place du nom de l'espace de noms pour qualifier les noms d'objets.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3f29b-1172">Un nombre quelconque d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à l’élément **using** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="3f29b-1173">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3f29b-1174">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3f29b-1175">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1175">Example</span></span>

<span data-ttu-id="3f29b-1176">L’exemple suivant illustre l' **utilisation** de l’élément Using pour importer un espace de noms défini ailleurs.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="3f29b-1177">Notez que l’espace de noms de l’élément de **schéma** indiqué est `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="3f29b-1178">La propriété `Address` sur l'**EntityType** `Publisher` est un type complexe défini dans l’espace de noms `ExtendedBooksModel` (importé avec l’élément **using** ).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="3f29b-1179">Attributs d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="3f29b-1180">Les attributs d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des attributs XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="3f29b-1181">En plus d'avoir une structure XML valide, les attributs d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="3f29b-1182">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="3f29b-1183">Plusieurs attributs d'annotation peuvent être appliqués à un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="3f29b-1184">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="3f29b-1185">Les attributs d'annotation peuvent être utilisés pour fournir des métadonnées supplémentaires sur des éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="3f29b-1186">Vous pouvez accéder aux métadonnées contenues dans les éléments d’annotation au moment de l’exécution à l’aide des classes de l’espace de noms System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-1187">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1187">Example</span></span>

<span data-ttu-id="3f29b-1188">L’exemple suivant montre un élément **EntityType** avec un attribut d’annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="3f29b-1189">L'exemple fait également apparaître un élément d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="3f29b-1190">Le code suivant récupère les métadonnées dans l'attribut d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="3f29b-1191">Le code ci-dessus suppose que le fichier `School.csdl` se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="3f29b-1192">Éléments d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="3f29b-1193">Les éléments d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des éléments XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="3f29b-1194">En plus d'avoir une structure XML valide, les éléments d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="3f29b-1195">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="3f29b-1196">Plusieurs éléments d'annotation peuvent être des enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="3f29b-1197">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="3f29b-1198">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="3f29b-1199">Les éléments d'annotation permettent de fournir des métadonnées supplémentaires sur les éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="3f29b-1200">À partir de la .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles au moment de l’exécution à l’aide des classes de l’espace de noms System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-1201">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1201">Example</span></span>

<span data-ttu-id="3f29b-1202">L’exemple suivant montre un élément **EntityType** avec un élément annotation (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="3f29b-1203">L'exemple fait également apparaître un attribut d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="3f29b-1204">Le code suivant récupère les métadonnées dans l'élément d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="3f29b-1205">Le code ci-dessus suppose que le fichier School.csdl se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="3f29b-1206">Types de modèles conceptuels (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="3f29b-1207">Le langage CSDL (Conceptual Schema Definition Language) prend en charge un ensemble de types de données primitifs abstraits, appelés **EDMSimpleTypes**, qui définissent des propriétés dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="3f29b-1208">Les **EDMSimpleTypes** sont des proxys pour les types de données primitifs pris en charge dans l’environnement de stockage ou d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="3f29b-1209">Le tableau suivant répertorie les types de données primitifs pris en charge par CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="3f29b-1210">Le tableau répertorie également les facettes qui peuvent être appliquées à chaque **type EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="3f29b-1211">Type EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="3f29b-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="3f29b-1212">Description</span><span class="sxs-lookup"><span data-stu-id="3f29b-1212">Description</span></span>                                                | <span data-ttu-id="3f29b-1213">Facettes applicables</span><span class="sxs-lookup"><span data-stu-id="3f29b-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="3f29b-1214">**EDM. binaire**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="3f29b-1215">Contient des données binaires.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="3f29b-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="3f29b-1217">**EDM. Boolean**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="3f29b-1218">Contient la valeur **true** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="3f29b-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="3f29b-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="3f29b-1221">Contient une valeur d'entier 8 bits non signé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="3f29b-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="3f29b-1224">Représente une date et une heure.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="3f29b-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1226">**EDM. DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="3f29b-1227">Contient une date et une heure en tant que décalage en minutes par rapport à l'heure GMT.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="3f29b-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="3f29b-1230">Contient une valeur numérique avec une précision et une échelle fixes.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="3f29b-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1232">**EDM. double**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="3f29b-1233">Contient un nombre à virgule flottante avec une précision de 15 chiffres</span><span class="sxs-lookup"><span data-stu-id="3f29b-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="3f29b-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="3f29b-1236">Contient un nombre à virgule flottante avec une précision de 7 chiffres.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="3f29b-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1238">**EDM. Guid**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="3f29b-1239">Contient un identificateur unique de 16 octets.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="3f29b-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="3f29b-1242">Contient une valeur d'entier 16 bits signé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="3f29b-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1244">**EDM. Int32**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="3f29b-1245">Contient une valeur d'entier 32 bits signé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="3f29b-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1247">**EDM. Int64**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="3f29b-1248">Contient une valeur d'entier 64 bits signé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="3f29b-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="3f29b-1251">Contient une valeur d'entier 8 bits signé.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="3f29b-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1253">**Edm.String**</span></span>                   | <span data-ttu-id="3f29b-1254">Contient des données caractères.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1254">Contains character data.</span></span>                                   | <span data-ttu-id="3f29b-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="3f29b-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="3f29b-1257">Contient une heure.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="3f29b-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3f29b-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3f29b-1259">**EDM. Geography**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="3f29b-1260">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1261">**EDM. GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="3f29b-1262">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="3f29b-1264">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="3f29b-1266">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="3f29b-1268">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="3f29b-1270">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="3f29b-1272">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1273">**EDM. GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="3f29b-1274">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="3f29b-1276">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="3f29b-1278">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="3f29b-1280">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="3f29b-1282">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="3f29b-1284">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="3f29b-1286">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="3f29b-1288">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3f29b-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="3f29b-1290">Nullable, par défaut, SRID</span><span class="sxs-lookup"><span data-stu-id="3f29b-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="3f29b-1291">Facettes (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3f29b-1291">Facets (CSDL)</span></span>

<span data-ttu-id="3f29b-1292">Les facettes dans le langage CSDL (Conceptual Schema Definition Language) représentent des contraintes sur les propriétés de types d'entités et de types complexes.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="3f29b-1293">Les facettes apparaissent comme des attributs XML sur les éléments CSDL suivants :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="3f29b-1294">Propriété</span><span class="sxs-lookup"><span data-stu-id="3f29b-1294">Property</span></span>
-   <span data-ttu-id="3f29b-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="3f29b-1295">TypeRef</span></span>
-   <span data-ttu-id="3f29b-1296">Paramètre</span><span class="sxs-lookup"><span data-stu-id="3f29b-1296">Parameter</span></span>

<span data-ttu-id="3f29b-1297">Le tableau ci-dessous décrit les facettes prises en charge dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="3f29b-1298">Toutes les facettes sont facultatives.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1298">All facets are optional.</span></span> <span data-ttu-id="3f29b-1299">Certaines facettes répertoriées ci-dessous sont utilisées par la Entity Framework lors de la génération d’une base de données à partir d’un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="3f29b-1300">Pour plus d’informations sur les types de données dans un modèle conceptuel, consultez types de modèles conceptuels (CSDL).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="3f29b-1301">Facette</span><span class="sxs-lookup"><span data-stu-id="3f29b-1301">Facet</span></span>               | <span data-ttu-id="3f29b-1302">Description</span><span class="sxs-lookup"><span data-stu-id="3f29b-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="3f29b-1303">S'applique à</span><span class="sxs-lookup"><span data-stu-id="3f29b-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="3f29b-1304">Utilisée pour la génération de base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1304">Used for the database generation</span></span> | <span data-ttu-id="3f29b-1305">Utilisée par le runtime.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="3f29b-1306">**Classement**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1306">**Collation**</span></span>       | <span data-ttu-id="3f29b-1307">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="3f29b-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3f29b-1309">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1309">Yes</span></span>                              | <span data-ttu-id="3f29b-1310">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1310">No</span></span>                  |
| <span data-ttu-id="3f29b-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="3f29b-1312">Indique que la valeur de la propriété doit être utilisée pour des contrôles d'accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="3f29b-1313">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3f29b-1314">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1314">No</span></span>                               | <span data-ttu-id="3f29b-1315">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1315">Yes</span></span>                 |
| <span data-ttu-id="3f29b-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1316">**Default**</span></span>         | <span data-ttu-id="3f29b-1317">Spécifie la valeur par défaut de la propriété si aucune valeur n'est fournie en cas d'instanciation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="3f29b-1318">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3f29b-1319">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1319">Yes</span></span>                              | <span data-ttu-id="3f29b-1320">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1320">Yes</span></span>                 |
| <span data-ttu-id="3f29b-1321">**Multiple**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1321">**FixedLength**</span></span>     | <span data-ttu-id="3f29b-1322">Spécifie si la longueur de la valeur de propriété peut varier.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="3f29b-1323">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3f29b-1324">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1324">Yes</span></span>                              | <span data-ttu-id="3f29b-1325">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1325">No</span></span>                  |
| <span data-ttu-id="3f29b-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1326">**MaxLength**</span></span>       | <span data-ttu-id="3f29b-1327">Spécifie la longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="3f29b-1328">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3f29b-1329">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1329">Yes</span></span>                              | <span data-ttu-id="3f29b-1330">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1330">No</span></span>                  |
| <span data-ttu-id="3f29b-1331">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1331">**Nullable**</span></span>        | <span data-ttu-id="3f29b-1332">Spécifie si la propriété peut avoir une valeur **null** .</span><span class="sxs-lookup"><span data-stu-id="3f29b-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="3f29b-1333">Toutes les propriétés **type EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3f29b-1334">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1334">Yes</span></span>                              | <span data-ttu-id="3f29b-1335">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1335">Yes</span></span>                 |
| <span data-ttu-id="3f29b-1336">**Précision**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1336">**Precision**</span></span>       | <span data-ttu-id="3f29b-1337">Pour les propriétés de type **Decimal**, spécifie le nombre de chiffres qu’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="3f29b-1338">Pour les propriétés de type **Time**, **DateTime**et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="3f29b-1339">**Edm. DateTime**, **Edm. DateTimeOffset**, **Edm. Decimal**, **Edm. Time**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="3f29b-1340">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1340">Yes</span></span>                              | <span data-ttu-id="3f29b-1341">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1341">No</span></span>                  |
| <span data-ttu-id="3f29b-1342">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1342">**Scale**</span></span>           | <span data-ttu-id="3f29b-1343">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="3f29b-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="3f29b-1345">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1345">Yes</span></span>                              | <span data-ttu-id="3f29b-1346">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1346">No</span></span>                  |
| <span data-ttu-id="3f29b-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1347">**SRID**</span></span>            | <span data-ttu-id="3f29b-1348">Spécifie l’ID du système de référence système spatial.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="3f29b-1349">Pour plus d’informations, consultez [SRID](https://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f29b-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="3f29b-1350">**EDM. Geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. GeographyCollection, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="3f29b-1351">Non</span><span class="sxs-lookup"><span data-stu-id="3f29b-1351">No</span></span>                               | <span data-ttu-id="3f29b-1352">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1352">Yes</span></span>                 |
| <span data-ttu-id="3f29b-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1353">**Unicode**</span></span>         | <span data-ttu-id="3f29b-1354">Indique si la valeur de propriété est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="3f29b-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="3f29b-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3f29b-1356">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1356">Yes</span></span>                              | <span data-ttu-id="3f29b-1357">Oui</span><span class="sxs-lookup"><span data-stu-id="3f29b-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="3f29b-1358">Lors de la génération d’une base de données à partir d’un modèle conceptuel, l’Assistant génération de base de données reconnaît la valeur de l’attribut **StoreGeneratedPattern** sur un élément de **propriété** s’il se trouve dans l’espace de noms suivant : https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="3f29b-1359">Les valeurs prises en charge pour l’attribut sont **Identity** et **computeed**.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="3f29b-1360">La valeur **Identity** produit une colonne de base de données avec une valeur d’identité générée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="3f29b-1361">Une valeur **calculée** génère une colonne avec une valeur qui est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="3f29b-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="3f29b-1362">Exemple</span><span class="sxs-lookup"><span data-stu-id="3f29b-1362">Example</span></span>

<span data-ttu-id="3f29b-1363">L'exemple suivant illustre l'application de facettes aux propriétés d'un type d'entité :</span><span class="sxs-lookup"><span data-stu-id="3f29b-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
