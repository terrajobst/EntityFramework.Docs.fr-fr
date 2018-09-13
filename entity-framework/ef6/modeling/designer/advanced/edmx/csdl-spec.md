---
title: Spécification CSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: f5bf0dc75a8195e9af979c9e044f36171f46c9b7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490517"
---
# <a name="csdl-specification"></a><span data-ttu-id="19669-102">Spécification CSDL</span><span class="sxs-lookup"><span data-stu-id="19669-102">CSDL Specification</span></span>
<span data-ttu-id="19669-103">Le langage CSDL (Conceptual Schema Definition Language) est un langage basé sur XML qui décrit les entités, relations et fonctions qui composent un modèle conceptuel d'une application pilotée par les données.</span><span class="sxs-lookup"><span data-stu-id="19669-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="19669-104">Ce modèle conceptuel peut être utilisé par Entity Framework ou WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="19669-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="19669-105">Les métadonnées qui sont décrites avec le langage CSDL sont utilisée par Entity Framework pour mapper les entités et relations qui sont définies dans un modèle conceptuel à une source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="19669-106">Pour plus d’informations, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) et [spécification MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="19669-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="19669-107">CSDL est l’implémentation d’Entity Framework de l’Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="19669-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="19669-108">Dans une application Entity Framework, les métadonnées du modèle conceptuel est chargée à partir d’un fichier .csdl (écrit en langage CSDL) dans une instance de la System.Data.Metadata.Edm.EdmItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="19669-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="19669-109">Entity Framework utilise des métadonnées de modèle conceptuel pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques à la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="19669-110">Le Concepteur EF stocke les informations de modèle conceptuel dans un fichier .edmx au moment du design.</span><span class="sxs-lookup"><span data-stu-id="19669-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="19669-111">Au moment de la génération, le Concepteur EF utilise les informations dans un fichier .edmx pour créer le fichier .csdl qui est nécessaire par Entity Framework lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="19669-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="19669-112">Les versions de CSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="19669-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="19669-113">Version du langage CSDL</span><span class="sxs-lookup"><span data-stu-id="19669-113">CSDL Version</span></span> | <span data-ttu-id="19669-114">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="19669-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="19669-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="19669-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="19669-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="19669-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="19669-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="19669-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="19669-118">Association, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-118">Association Element (CSDL)</span></span>

<span data-ttu-id="19669-119">Un **Association** élément définit une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="19669-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="19669-120">Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ».</span><span class="sxs-lookup"><span data-stu-id="19669-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="19669-121">La multiplicité d’une terminaison d’association peut avoir une valeur d’un (1), zéro ou un (0.. 1) ou plusieurs (\*).</span><span class="sxs-lookup"><span data-stu-id="19669-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="19669-122">Ces informations sont spécifiées dans les deux éléments de fin d’enfants.</span><span class="sxs-lookup"><span data-stu-id="19669-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="19669-123">Il est possible d'accéder aux instances de type d'entité au niveau d'une terminaison d'une association via les propriétés de navigation ou les clés étrangères, si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="19669-124">Dans une application, une instance d'une association représente une association spécifique entre des instances de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="19669-125">Les instances d'association sont regroupées logiquement dans un ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="19669-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="19669-126">Un **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-127">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-128">Fin (exactement 2 éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="19669-129">ReferentialConstraint (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="19669-130">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-131">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-131">Applicable Attributes</span></span>

<span data-ttu-id="19669-132">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="19669-133">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-133">Attribute Name</span></span> | <span data-ttu-id="19669-134">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-134">Is Required</span></span> | <span data-ttu-id="19669-135">Value</span><span class="sxs-lookup"><span data-stu-id="19669-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="19669-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-136">**Name**</span></span>       | <span data-ttu-id="19669-137">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-137">Yes</span></span>         | <span data-ttu-id="19669-138">Nom de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-139">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="19669-140">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-141">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-142">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-142">Example</span></span>

<span data-ttu-id="19669-143">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association de clés étrangères n’ont pas été exposées sur le **client** et  **Commande** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="19669-144">Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**.</span><span class="sxs-lookup"><span data-stu-id="19669-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="19669-145">En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext est supprimé. Si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="19669-146">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association lorsque les clés étrangères ont été exposées sur le **client** et  **Commande** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="19669-147">Avec des clés étrangères exposées, la relation entre les entités est gérée avec un **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="19669-148">Un élément AssociationSetMapping correspondant n’est pas nécessaire pour mapper cette association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="19669-149">AssociationSet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="19669-150">Le **AssociationSet** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les instances d’association du même type.</span><span class="sxs-lookup"><span data-stu-id="19669-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="19669-151">Un ensemble d'associations fournit une définition pour le regroupement d'instances d'association afin qu'elles puissent être mappées à une source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="19669-152">Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-153">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-154">Fin (exactement deux éléments requis)</span><span class="sxs-lookup"><span data-stu-id="19669-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="19669-155">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="19669-156">Le **Association** attribut spécifie le type d’association qui contient un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="19669-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="19669-157">Les jeux d’entités qui composent les terminaisons d’un ensemble d’associations sont spécifiés avec exactement deux enfants **fin** éléments.</span><span class="sxs-lookup"><span data-stu-id="19669-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-158">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-158">Applicable Attributes</span></span>

<span data-ttu-id="19669-159">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="19669-160">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-160">Attribute Name</span></span>  | <span data-ttu-id="19669-161">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-161">Is Required</span></span> | <span data-ttu-id="19669-162">Value</span><span class="sxs-lookup"><span data-stu-id="19669-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-163">**Name**</span></span>        | <span data-ttu-id="19669-164">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-164">Yes</span></span>         | <span data-ttu-id="19669-165">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-165">The name of the entity set.</span></span> <span data-ttu-id="19669-166">La valeur de la **nom** attribut ne peut pas être identique à la valeur de la **Association** attribut.</span><span class="sxs-lookup"><span data-stu-id="19669-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="19669-167">**Association**</span><span class="sxs-lookup"><span data-stu-id="19669-167">**Association**</span></span> | <span data-ttu-id="19669-168">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-168">Yes</span></span>         | <span data-ttu-id="19669-169">Nom qualifié complet de l'association dont l'ensemble d'associations contient des instances.</span><span class="sxs-lookup"><span data-stu-id="19669-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="19669-170">L'association doit être dans le même espace de noms que l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="19669-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-171">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="19669-172">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-173">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-174">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-174">Example</span></span>

<span data-ttu-id="19669-175">L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="19669-176">Élément CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="19669-177">Le **CollectionType** élément de langage de définition de schéma conceptuel (CSDL) Spécifie qu’une fonction ou un paramètre de fonction de type de retour est une collection.</span><span class="sxs-lookup"><span data-stu-id="19669-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="19669-178">Le **CollectionType** élément peut être un enfant de l’élément de paramètre ou de l’élément ReturnType (fonction).</span><span class="sxs-lookup"><span data-stu-id="19669-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="19669-179">Le type de collection peut être spécifié en utilisant le **Type** attribut ou l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="19669-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="19669-180">**CollectionType**</span></span>
-   <span data-ttu-id="19669-181">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="19669-181">ReferenceType</span></span>
-   <span data-ttu-id="19669-182">RowType ;</span><span class="sxs-lookup"><span data-stu-id="19669-182">RowType</span></span>
-   <span data-ttu-id="19669-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="19669-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="19669-184">Un modèle ne sera pas validé si le type d’une collection est spécifié avec à la fois le **Type** attribut et un élément enfant.</span><span class="sxs-lookup"><span data-stu-id="19669-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="19669-185">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-185">Applicable Attributes</span></span>

<span data-ttu-id="19669-186">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="19669-187">Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables aux collections de **types Edmsimpletype**.</span><span class="sxs-lookup"><span data-stu-id="19669-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="19669-188">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-188">Attribute Name</span></span>                                                          | <span data-ttu-id="19669-189">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-189">Is Required</span></span> | <span data-ttu-id="19669-190">Value</span><span class="sxs-lookup"><span data-stu-id="19669-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-191">**Type**</span></span>                                                                | <span data-ttu-id="19669-192">Non</span><span class="sxs-lookup"><span data-stu-id="19669-192">No</span></span>          | <span data-ttu-id="19669-193">Type de la collection.</span><span class="sxs-lookup"><span data-stu-id="19669-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="19669-194">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-194">**Nullable**</span></span>                                                            | <span data-ttu-id="19669-195">Non</span><span class="sxs-lookup"><span data-stu-id="19669-195">No</span></span>          | <span data-ttu-id="19669-196">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="19669-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="19669-197">> Dans le langage CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="19669-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="19669-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="19669-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="19669-199">Non</span><span class="sxs-lookup"><span data-stu-id="19669-199">No</span></span>          | <span data-ttu-id="19669-200">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="19669-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="19669-202">Non</span><span class="sxs-lookup"><span data-stu-id="19669-202">No</span></span>          | <span data-ttu-id="19669-203">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="19669-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="19669-205">Non</span><span class="sxs-lookup"><span data-stu-id="19669-205">No</span></span>          | <span data-ttu-id="19669-206">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="19669-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="19669-207">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-207">**Precision**</span></span>                                                           | <span data-ttu-id="19669-208">Non</span><span class="sxs-lookup"><span data-stu-id="19669-208">No</span></span>          | <span data-ttu-id="19669-209">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="19669-210">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-210">**Scale**</span></span>                                                               | <span data-ttu-id="19669-211">Non</span><span class="sxs-lookup"><span data-stu-id="19669-211">No</span></span>          | <span data-ttu-id="19669-212">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="19669-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-213">**SRID**</span></span>                                                                | <span data-ttu-id="19669-214">Non</span><span class="sxs-lookup"><span data-stu-id="19669-214">No</span></span>          | <span data-ttu-id="19669-215">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-216">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="19669-217">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="19669-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="19669-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-218">**Unicode**</span></span>                                                             | <span data-ttu-id="19669-219">Non</span><span class="sxs-lookup"><span data-stu-id="19669-219">No</span></span>          | <span data-ttu-id="19669-220">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="19669-221">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-221">**Collation**</span></span>                                                           | <span data-ttu-id="19669-222">Non</span><span class="sxs-lookup"><span data-stu-id="19669-222">No</span></span>          | <span data-ttu-id="19669-223">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="19669-224">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="19669-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-227">Example</span></span>

<span data-ttu-id="19669-228">L’exemple suivant illustre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de **personne** types d’entités (comme spécifié par le **ElementType** attribut).</span><span class="sxs-lookup"><span data-stu-id="19669-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="19669-229">L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="19669-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="19669-230">L’exemple suivant montre une fonction définie par modèle qui utilise le **CollectionType** élément pour spécifier que la fonction accepte en tant que paramètre à une collection de **département** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="19669-231">ComplexType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="19669-232">Un **ComplexType** élément définit une structure de données composée de **EdmSimpleType** propriétés ou autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="19669-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="19669-233">Un type complexe peut être une propriété d'un type d'entité ou d'un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="19669-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="19669-234">Un type complexe est semblable à un type d'entité du fait qu'un type complexe définit des données.</span><span class="sxs-lookup"><span data-stu-id="19669-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="19669-235">Toutefois, il existe des différences clés entre les types complexes et les types d'entités :</span><span class="sxs-lookup"><span data-stu-id="19669-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="19669-236">Les types complexes n'ont pas d'identité (ni de clés), par conséquent, ils ne peuvent pas exister indépendamment.</span><span class="sxs-lookup"><span data-stu-id="19669-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="19669-237">Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="19669-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="19669-238">Les types complexes ne peuvent pas participer aux associations.</span><span class="sxs-lookup"><span data-stu-id="19669-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="19669-239">Ni la fin d’une association peut être un type complexe, et par conséquent les propriétés de navigation ne peut pas être définies pour les types complexes.</span><span class="sxs-lookup"><span data-stu-id="19669-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="19669-240">Une propriété de type complexe ne peut pas avoir de valeur NULL, bien que les propriétés scalaires d'un type complexe puissent chacune être définie sur NULL.</span><span class="sxs-lookup"><span data-stu-id="19669-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="19669-241">Un **ComplexType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-242">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-243">Propriété (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="19669-244">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="19669-245">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ComplexType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="19669-246">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="19669-247">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-247">Is Required</span></span> | <span data-ttu-id="19669-248">Value</span><span class="sxs-lookup"><span data-stu-id="19669-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-249">Name</span><span class="sxs-lookup"><span data-stu-id="19669-249">Name</span></span>                                                                                                           | <span data-ttu-id="19669-250">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-250">Yes</span></span>         | <span data-ttu-id="19669-251">Nom du type complexe.</span><span class="sxs-lookup"><span data-stu-id="19669-251">The name of the complex type.</span></span> <span data-ttu-id="19669-252">Le nom d'un type complexe ne peut pas être identique au nom d'un autre type complexe, d'un type d'entité ou d'une association qui figure dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="19669-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="19669-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="19669-254">Non</span><span class="sxs-lookup"><span data-stu-id="19669-254">No</span></span>          | <span data-ttu-id="19669-255">Nom d'un autre type complexe qui est le type de base du type complexe en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="19669-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="19669-256">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="19669-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="19669-257">L'héritage pour les types complexes n'est pas pris en charge dans cette version.</span><span class="sxs-lookup"><span data-stu-id="19669-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="19669-258">Abstract</span><span class="sxs-lookup"><span data-stu-id="19669-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="19669-259">Non</span><span class="sxs-lookup"><span data-stu-id="19669-259">No</span></span>          | <span data-ttu-id="19669-260">**True** ou **False** (la valeur par défaut) selon que le type complexe est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="19669-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="19669-261">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="19669-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="19669-262">Les types complexes dans cette version ne peuvent pas être des types abstraits.</span><span class="sxs-lookup"><span data-stu-id="19669-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="19669-263">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ComplexType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="19669-264">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-265">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-266">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-266">Example</span></span>

<span data-ttu-id="19669-267">L’exemple suivant illustre un type complexe, **adresse**, avec le **EdmSimpleType** propriétés **StreetAddress**, **Ville**,  **StateOrProvince**, **pays**, et **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="19669-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="19669-268">Pour définir le type complexe **adresse** (ci-dessus) en tant que propriété d’un type d’entité, vous devez déclarer le type de propriété dans la définition de type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="19669-269">L’exemple suivant montre le **adresse** propriété comme un type complexe sur un type d’entité (**Publisher**) :</span><span class="sxs-lookup"><span data-stu-id="19669-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="19669-270">DefiningExpression, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="19669-271">Le **DefiningExpression** élément de langage de définition de schéma conceptuel (CSDL) contient une expression Entity SQL qui définit une fonction dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="19669-272">À des fins de validation, un **DefiningExpression** élément peut contenir du contenu arbitraire.</span><span class="sxs-lookup"><span data-stu-id="19669-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="19669-273">Toutefois, Entity Framework lève une exception lors de l’exécution si un **DefiningExpression** élément ne contient pas de valide Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="19669-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="19669-274">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-274">Applicable Attributes</span></span>

<span data-ttu-id="19669-275">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **DefiningExpression** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="19669-276">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-277">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="19669-278">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-278">Example</span></span>

<span data-ttu-id="19669-279">L’exemple suivant utilise un **DefiningExpression** élément pour définir une fonction qui retourne le nombre d’années depuis un ouvrage a été publié.</span><span class="sxs-lookup"><span data-stu-id="19669-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="19669-280">Le contenu de la **DefiningExpression** élément est écrit dans Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="19669-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="19669-281">Dependent, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="19669-282">Le **dépendants** élément dans le langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint et définit la terminaison dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="19669-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="19669-283">Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="19669-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="19669-284">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="19669-285">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="19669-286">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="19669-287">**PropertyRef** éléments sont utilisés pour spécifier les clés qui référencent la terminaison principale.</span><span class="sxs-lookup"><span data-stu-id="19669-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="19669-288">Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-289">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="19669-290">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-291">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-291">Applicable Attributes</span></span>

<span data-ttu-id="19669-292">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="19669-293">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-293">Attribute Name</span></span> | <span data-ttu-id="19669-294">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-294">Is Required</span></span> | <span data-ttu-id="19669-295">Value</span><span class="sxs-lookup"><span data-stu-id="19669-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="19669-296">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="19669-296">**Role**</span></span>       | <span data-ttu-id="19669-297">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-297">Yes</span></span>         | <span data-ttu-id="19669-298">Nom du type d'entité au niveau de la terminaison dépendante de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-299">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="19669-300">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-301">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-302">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-302">Example</span></span>

<span data-ttu-id="19669-303">L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="19669-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="19669-304">Le **PublisherId** propriété de la **livre** type d’entité compose la terminaison dépendante de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="19669-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="19669-305">Documentation, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="19669-306">Le **Documentation** élément dans le langage de définition de schéma conceptuel (CSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="19669-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="19669-307">Dans un fichier .edmx, lorsque le **Documentation** élément est un enfant d’un élément qui apparaît sous la forme d’un objet sur l’aire de conception du Concepteur EF (par exemple, une entité, associations ou une propriété), le contenu de la **Documentation**  élément apparaîtra dans Visual Studio **propriétés** fenêtre de l’objet.</span><span class="sxs-lookup"><span data-stu-id="19669-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="19669-308">Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-309">**Résumé**: une brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="19669-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="19669-310">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="19669-310">(zero or one element)</span></span>
-   <span data-ttu-id="19669-311">**LongDescription**: une description détaillée de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="19669-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="19669-312">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="19669-312">(zero or one element)</span></span>
-   <span data-ttu-id="19669-313">Éléments d’annotation.</span><span class="sxs-lookup"><span data-stu-id="19669-313">Annotation elements.</span></span> <span data-ttu-id="19669-314">(zéro, un ou plusieurs éléments).</span><span class="sxs-lookup"><span data-stu-id="19669-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-315">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-315">Applicable Attributes</span></span>

<span data-ttu-id="19669-316">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="19669-317">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-318">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="19669-319">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-319">Example</span></span>

<span data-ttu-id="19669-320">L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="19669-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="19669-321">Si l’extrait de code ci-dessous ont été dans le langage CSDL contenu d’un .edmx de fichiers, le contenu de la **Résumé** et **LongDescription** éléments apparaîtrait dans Visual Studio **propriétés** fenêtre lorsque vous cliquez sur le `Customer` type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="19669-322">End, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-322">End Element (CSDL)</span></span>

<span data-ttu-id="19669-323">Le **fin** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="19669-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="19669-324">Dans chaque cas, le rôle de la **fin** élément est différent et les attributs applicables sont différents.</span><span class="sxs-lookup"><span data-stu-id="19669-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="19669-325">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="19669-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="19669-326">Un **fin** élément (en tant qu’enfant de le **Association** élément) identifie le type d’entité sur une extrémité d’une association et le nombre d’instances de type d’entité qui peuvent exister à cette fin d’une association.</span><span class="sxs-lookup"><span data-stu-id="19669-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="19669-327">Les terminaisons d'association sont définies dans le cadre d'une association ; une association doit avoir exactement deux terminaisons d'association.</span><span class="sxs-lookup"><span data-stu-id="19669-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="19669-328">Il est possible d'accéder aux instances de type d'entité au niveau de la terminaison d'une association via les propriétés de navigation ou les clés étrangères si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="19669-329">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-330">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-331">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="19669-332">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-333">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-333">Applicable Attributes</span></span>

<span data-ttu-id="19669-334">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="19669-335">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-335">Attribute Name</span></span>   | <span data-ttu-id="19669-336">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-336">Is Required</span></span> | <span data-ttu-id="19669-337">Value</span><span class="sxs-lookup"><span data-stu-id="19669-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-338">**Type**</span></span>         | <span data-ttu-id="19669-339">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-339">Yes</span></span>         | <span data-ttu-id="19669-340">Nom du type d'entité au niveau de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="19669-341">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="19669-341">**Role**</span></span>         | <span data-ttu-id="19669-342">Non</span><span class="sxs-lookup"><span data-stu-id="19669-342">No</span></span>          | <span data-ttu-id="19669-343">Nom de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-343">A name for the association end.</span></span> <span data-ttu-id="19669-344">Si aucun nom n'est fourni, le nom du type d'entité au niveau de la terminaison de l'association sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="19669-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="19669-345">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="19669-345">**Multiplicity**</span></span> | <span data-ttu-id="19669-346">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-346">Yes</span></span>         | <span data-ttu-id="19669-347">**1**, **valeur 0.. 1**, ou **\*** selon le nombre d’instances de type d’entité qui peut être à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="19669-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="19669-348">**1** indique cette instance de type exactement une entité existe à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="19669-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="19669-349">**valeur 0.. 1** indique que zéro ou une instance de type d’entité existe au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="19669-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="19669-350">**\*** Indique que zéro, une ou plusieurs instances de type d’entité existent à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="19669-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-351">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="19669-352">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-353">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-354">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-354">Example</span></span>

<span data-ttu-id="19669-355">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="19669-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="19669-356">Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**.</span><span class="sxs-lookup"><span data-stu-id="19669-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="19669-357">En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et qui ont été chargés dans ObjectContext sera if supprimé le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="19669-358">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="19669-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="19669-359">Le **fin** élément spécifie une extrémité d’un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="19669-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="19669-360">Le **AssociationSet** élément doit contenir deux **fin** éléments.</span><span class="sxs-lookup"><span data-stu-id="19669-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="19669-361">Les informations contenues dans un **fin** élément est utilisé pour mapper une ensemble d’associations à une source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="19669-362">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-363">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-364">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="19669-365">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="19669-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="19669-366">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="19669-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-367">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-367">Applicable Attributes</span></span>

<span data-ttu-id="19669-368">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="19669-369">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-369">Attribute Name</span></span> | <span data-ttu-id="19669-370">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-370">Is Required</span></span> | <span data-ttu-id="19669-371">Value</span><span class="sxs-lookup"><span data-stu-id="19669-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="19669-372">**EntitySet**</span></span>  | <span data-ttu-id="19669-373">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-373">Yes</span></span>         | <span data-ttu-id="19669-374">Le nom de la **EntitySet** élément qui définit une extrémité du parent **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="19669-375">Le **EntitySet** élément doit être défini dans le même conteneur d’entités en tant que parent **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="19669-376">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="19669-376">**Role**</span></span>       | <span data-ttu-id="19669-377">Non</span><span class="sxs-lookup"><span data-stu-id="19669-377">No</span></span>          | <span data-ttu-id="19669-378">Nom de la terminaison de l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="19669-378">The name of the association set end.</span></span> <span data-ttu-id="19669-379">Si le **rôle** attribut n’est pas utilisé, le nom de l’ensemble d’associations sera le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="19669-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="19669-380">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="19669-381">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-382">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-383">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-383">Example</span></span>

<span data-ttu-id="19669-384">L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments, chacun avec deux **fin** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="19669-385">EntityContainer, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="19669-386">Le **EntityContainer** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les jeux d’entités, les ensembles d’associations et les importations de fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="19669-387">Un conteneur d’entités de modèle conceptuel est mappé à un conteneur d’entités du modèle via l’élément EntityContainerMapping stockage.</span><span class="sxs-lookup"><span data-stu-id="19669-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="19669-388">Un conteneur d'entités de modèle de stockage décrit la structure de la base de données : les jeux d'entités décrivent les tables, les ensembles d'associations décrivent les contraintes de clé étrangère et les importations de fonction décrivent les procédures stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="19669-389">Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation.</span><span class="sxs-lookup"><span data-stu-id="19669-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="19669-390">Si un **Documentation** élément est présent, il doit précéder tous les **EntitySet**, **AssociationSet**, et **FunctionImport** éléments.</span><span class="sxs-lookup"><span data-stu-id="19669-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="19669-391">Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-392">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="19669-392">EntitySet</span></span>
-   <span data-ttu-id="19669-393">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="19669-393">AssociationSet</span></span>
-   <span data-ttu-id="19669-394">FunctionImport ;</span><span class="sxs-lookup"><span data-stu-id="19669-394">FunctionImport</span></span>
-   <span data-ttu-id="19669-395">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="19669-395">Annotation elements</span></span>

<span data-ttu-id="19669-396">Vous pouvez étendre un **EntityContainer** élément pour inclure le contenu d’un autre **EntityContainer** qui figure dans le même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="19669-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="19669-397">Pour inclure le contenu d’un autre **EntityContainer**, dans le référencement **EntityContainer** élément, définissez la valeur de la **étend** nom à l’attribut de la  **EntityContainer** élément que vous souhaitez inclure.</span><span class="sxs-lookup"><span data-stu-id="19669-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="19669-398">Tous les éléments enfants de l’élément inclus **EntityContainer** élément est traité comme éléments enfants de la référence **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-399">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-399">Applicable Attributes</span></span>

<span data-ttu-id="19669-400">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="19669-401">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-401">Attribute Name</span></span> | <span data-ttu-id="19669-402">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-402">Is Required</span></span> | <span data-ttu-id="19669-403">Value</span><span class="sxs-lookup"><span data-stu-id="19669-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="19669-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-404">**Name**</span></span>       | <span data-ttu-id="19669-405">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-405">Yes</span></span>         | <span data-ttu-id="19669-406">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="19669-407">**Étend**</span><span class="sxs-lookup"><span data-stu-id="19669-407">**Extends**</span></span>    | <span data-ttu-id="19669-408">Non</span><span class="sxs-lookup"><span data-stu-id="19669-408">No</span></span>          | <span data-ttu-id="19669-409">Le nom d'un autre conteneur d'entités au sein du même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="19669-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-410">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="19669-411">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-412">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-413">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-413">Example</span></span>

<span data-ttu-id="19669-414">L’exemple suivant montre un **EntityContainer** élément qui définit trois jeux d’entités et les deux ensembles d’associations.</span><span class="sxs-lookup"><span data-stu-id="19669-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="19669-415">EntitySet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="19669-416">Le **EntitySet** élément dans le langage de définition de schéma conceptuel est un conteneur logique pour les instances d’un type d’entité et les instances de n’importe quel type dérivé de ce type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="19669-417">La relation entre un type d'entité et un jeu d'entités est analogue à la relation entre une ligne et une table dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="19669-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="19669-418">Comme une ligne, un type d'entité définit un jeu de données connexes et, comme une table, un jeu d'entités contient des instances de cette définition.</span><span class="sxs-lookup"><span data-stu-id="19669-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="19669-419">Un jeu d'entités fournit une construction pour le regroupement d'instances du type d'entité, afin qu'elles puissent être mappées aux structures de données associées dans une source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="19669-420">Plusieurs jeux d'entités peuvent être définis pour un type d'entité particulier.</span><span class="sxs-lookup"><span data-stu-id="19669-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-421">Le Concepteur EF ne prend pas en charge les modèles conceptuels qui contiennent des jeux d’entités multiples par type.</span><span class="sxs-lookup"><span data-stu-id="19669-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="19669-422">Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-423">Documentation, élément (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-424">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-425">Applicable Attributes</span></span>

<span data-ttu-id="19669-426">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="19669-427">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-427">Attribute Name</span></span> | <span data-ttu-id="19669-428">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-428">Is Required</span></span> | <span data-ttu-id="19669-429">Value</span><span class="sxs-lookup"><span data-stu-id="19669-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-430">**Name**</span></span>       | <span data-ttu-id="19669-431">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-431">Yes</span></span>         | <span data-ttu-id="19669-432">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="19669-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="19669-433">**EntityType**</span></span> | <span data-ttu-id="19669-434">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-434">Yes</span></span>         | <span data-ttu-id="19669-435">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="19669-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-436">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="19669-437">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-438">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-439">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-439">Example</span></span>

<span data-ttu-id="19669-440">L’exemple suivant montre un **EntityContainer** élément avec trois **EntitySet** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="19669-441">Il est possible de définir des jeux d'entités multiples par type (MEST).</span><span class="sxs-lookup"><span data-stu-id="19669-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="19669-442">L’exemple suivant définit un conteneur d’entités avec deux jeux d’entités pour le **livre** type d’entité :</span><span class="sxs-lookup"><span data-stu-id="19669-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="19669-443">EntityType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="19669-444">Le **EntityType** élément représente la structure d’un concept de niveau supérieur, tel qu’un client ou l’ordre, dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="19669-445">Un type d'entité est un modèle pour des instances de types d'entités dans une application.</span><span class="sxs-lookup"><span data-stu-id="19669-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="19669-446">Chaque modèle contient les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="19669-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="19669-447">Nom unique.</span><span class="sxs-lookup"><span data-stu-id="19669-447">A unique name.</span></span> <span data-ttu-id="19669-448">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="19669-448">(Required.)</span></span>
-   <span data-ttu-id="19669-449">Clé d'entité définie par une ou plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="19669-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="19669-450">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="19669-450">(Required.)</span></span>
-   <span data-ttu-id="19669-451">Propriétés pour contenir des données.</span><span class="sxs-lookup"><span data-stu-id="19669-451">Properties for containing data.</span></span> <span data-ttu-id="19669-452">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="19669-452">(Optional.)</span></span>
-   <span data-ttu-id="19669-453">Propriétés de navigation qui permettent de naviguer d'une terminaison d'une association à l'autre terminaison.</span><span class="sxs-lookup"><span data-stu-id="19669-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="19669-454">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="19669-454">(Optional.)</span></span>

<span data-ttu-id="19669-455">Dans une application, une instance d'un type d'entité représente un objet spécifique (tel qu'un client ou une commande spécifique).</span><span class="sxs-lookup"><span data-stu-id="19669-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="19669-456">Chaque instance d'un type d'entité doit avoir une clé d'entité unique dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="19669-457">Deux instances de type d'entité sont considérées égales seulement si elles sont du même type et si les valeurs de leurs clés d'entité sont identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="19669-458">Un **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-459">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-460">Clé (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="19669-461">Propriété (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="19669-462">NavigationProperty (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="19669-463">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-464">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-464">Applicable Attributes</span></span>

<span data-ttu-id="19669-465">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="19669-466">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="19669-467">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-467">Is Required</span></span> | <span data-ttu-id="19669-468">Value</span><span class="sxs-lookup"><span data-stu-id="19669-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="19669-470">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-470">Yes</span></span>         | <span data-ttu-id="19669-471">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="19669-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="19669-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="19669-473">Non</span><span class="sxs-lookup"><span data-stu-id="19669-473">No</span></span>          | <span data-ttu-id="19669-474">Nom d'un autre type d'entité qui est le type de base du type d'entité en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="19669-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="19669-475">**Abstract**</span><span class="sxs-lookup"><span data-stu-id="19669-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="19669-476">Non</span><span class="sxs-lookup"><span data-stu-id="19669-476">No</span></span>          | <span data-ttu-id="19669-477">**True** ou **False**, selon que le type d’entité est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="19669-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="19669-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="19669-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="19669-479">Non</span><span class="sxs-lookup"><span data-stu-id="19669-479">No</span></span>          | <span data-ttu-id="19669-480">**True** ou **False** selon que le type d’entité est un type d’entité ouverte.</span><span class="sxs-lookup"><span data-stu-id="19669-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="19669-481">> Le **OpenType** attribut s’applique uniquement aux types d’entités qui sont définies dans les modèles conceptuels utilisés avec ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="19669-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="19669-482">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="19669-483">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-484">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-485">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-485">Example</span></span>

<span data-ttu-id="19669-486">L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments et deux **NavigationProperty** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="19669-487">EnumType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="19669-488">Le **EnumType** élément représente un type énuméré.</span><span class="sxs-lookup"><span data-stu-id="19669-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="19669-489">Un **EnumType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-490">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-491">Membre (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="19669-492">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-493">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-493">Applicable Attributes</span></span>

<span data-ttu-id="19669-494">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EnumType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="19669-495">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-495">Attribute Name</span></span>     | <span data-ttu-id="19669-496">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-496">Is Required</span></span> | <span data-ttu-id="19669-497">Value</span><span class="sxs-lookup"><span data-stu-id="19669-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-498">**Name**</span></span>           | <span data-ttu-id="19669-499">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-499">Yes</span></span>         | <span data-ttu-id="19669-500">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="19669-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="19669-501">**IsFlags**</span></span>        | <span data-ttu-id="19669-502">Non</span><span class="sxs-lookup"><span data-stu-id="19669-502">No</span></span>          | <span data-ttu-id="19669-503">**True** ou **False**, selon si le type enum peut être utilisé comme un jeu d’indicateurs.</span><span class="sxs-lookup"><span data-stu-id="19669-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="19669-504">La valeur par défaut est **False.**.</span><span class="sxs-lookup"><span data-stu-id="19669-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="19669-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="19669-505">**UnderlyingType**</span></span> | <span data-ttu-id="19669-506">Non</span><span class="sxs-lookup"><span data-stu-id="19669-506">No</span></span>          | <span data-ttu-id="19669-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** ou **Edm.SByte** définition de la plage de valeurs du type.</span><span class="sxs-lookup"><span data-stu-id="19669-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="19669-508">La valeur par défaut, le type sous-jacent d’éléments de l’énumération est **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="19669-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-509">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EnumType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="19669-510">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-511">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-512">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-512">Example</span></span>

<span data-ttu-id="19669-513">L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="19669-514">Function, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-514">Function Element (CSDL)</span></span>

<span data-ttu-id="19669-515">Le **fonction** élément dans le langage de définition de schéma conceptuel (CSDL) est utilisé pour définir ou déclarer des fonctions dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="19669-516">Une fonction est définie à l’aide d’un élément DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="19669-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="19669-517">Un **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-518">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-519">Paramètre (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="19669-520">DefiningExpression (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="19669-521">ReturnType (fonction) (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="19669-522">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="19669-523">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (Function) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="19669-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="19669-524">Les types de retour possibles correspondent à tout type EdmSimpleType, type d'entité, type complexe, type de ligne ou type REF (ou à une collection de l'un de ces types).</span><span class="sxs-lookup"><span data-stu-id="19669-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-525">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-525">Applicable Attributes</span></span>

<span data-ttu-id="19669-526">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="19669-527">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-527">Attribute Name</span></span> | <span data-ttu-id="19669-528">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-528">Is Required</span></span> | <span data-ttu-id="19669-529">Value</span><span class="sxs-lookup"><span data-stu-id="19669-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="19669-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-530">**Name**</span></span>       | <span data-ttu-id="19669-531">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-531">Yes</span></span>         | <span data-ttu-id="19669-532">Nom de la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-532">The name of the function.</span></span>          |
| <span data-ttu-id="19669-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="19669-533">**ReturnType**</span></span> | <span data-ttu-id="19669-534">Non</span><span class="sxs-lookup"><span data-stu-id="19669-534">No</span></span>          | <span data-ttu-id="19669-535">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-536">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="19669-537">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-538">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-539">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-539">Example</span></span>

<span data-ttu-id="19669-540">L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années écoulées depuis l’embauche d’un formateur.</span><span class="sxs-lookup"><span data-stu-id="19669-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="19669-541">FunctionImport, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="19669-542">Le **FunctionImport** élément de langage de définition de schéma conceptuel (CSDL) représente une fonction qui est définie dans la source de données mais disponible pour les objets via le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="19669-543">Par exemple, un élément de la fonction dans le modèle de stockage peut être utilisé pour représenter une procédure stockée dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="19669-544">Un **FunctionImport** élément dans le modèle conceptuel représente la fonction correspondante dans une application Entity Framework et est mappé à la fonction de modèle de stockage à l’aide de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="19669-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="19669-545">Lorsque la fonction est appelée dans l'application, la procédure stockée correspondante est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="19669-546">Le **FunctionImport** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-547">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-548">Paramètre (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="19669-549">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="19669-550">ReturnType (FunctionImport) (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="19669-551">Un **paramètre** élément doit être défini pour chaque paramètre qui accepte la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="19669-552">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (FunctionImport) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="19669-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="19669-553">La valeur de type de retour doit être une collection de type EdmSimpleType, EntityType ou ComplexType.</span><span class="sxs-lookup"><span data-stu-id="19669-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-554">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-554">Applicable Attributes</span></span>

<span data-ttu-id="19669-555">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="19669-556">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-556">Attribute Name</span></span>   | <span data-ttu-id="19669-557">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-557">Is Required</span></span> | <span data-ttu-id="19669-558">Value</span><span class="sxs-lookup"><span data-stu-id="19669-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-559">**Name**</span></span>         | <span data-ttu-id="19669-560">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-560">Yes</span></span>         | <span data-ttu-id="19669-561">Nom de la fonction importée.</span><span class="sxs-lookup"><span data-stu-id="19669-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="19669-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="19669-562">**ReturnType**</span></span>   | <span data-ttu-id="19669-563">Non</span><span class="sxs-lookup"><span data-stu-id="19669-563">No</span></span>          | <span data-ttu-id="19669-564">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-564">The type that the function returns.</span></span> <span data-ttu-id="19669-565">N'utilisez pas cet attribut si la fonction ne retourne pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="19669-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="19669-566">Sinon, la valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="19669-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="19669-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="19669-567">**EntitySet**</span></span>    | <span data-ttu-id="19669-568">Non</span><span class="sxs-lookup"><span data-stu-id="19669-568">No</span></span>          | <span data-ttu-id="19669-569">Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection.</span><span class="sxs-lookup"><span data-stu-id="19669-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="19669-570">Sinon, le **EntitySet** attribut ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="19669-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="19669-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="19669-571">**IsComposable**</span></span> | <span data-ttu-id="19669-572">Non</span><span class="sxs-lookup"><span data-stu-id="19669-572">No</span></span>          | <span data-ttu-id="19669-573">Si la valeur est définie sur true, la fonction est composable (Table-valued Function) et peut être utilisée dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="19669-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="19669-574">La valeur par défaut est **false**.</span><span class="sxs-lookup"><span data-stu-id="19669-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="19669-575">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="19669-576">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-577">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-578">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-578">Example</span></span>

<span data-ttu-id="19669-579">L’exemple suivant montre un **FunctionImport** élément qui accepte un paramètre et retourne une collection de types d’entités :</span><span class="sxs-lookup"><span data-stu-id="19669-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="19669-580">Key, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-580">Key Element (CSDL)</span></span>

<span data-ttu-id="19669-581">Le **clé** élément est un élément enfant de l’élément EntityType et définit un *clé d’entité* (une propriété ou un ensemble de propriétés qui déterminent l’identité d’un type d’entité).</span><span class="sxs-lookup"><span data-stu-id="19669-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="19669-582">Les propriétés qui composent une clé d'entité sont choisies au moment du design.</span><span class="sxs-lookup"><span data-stu-id="19669-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="19669-583">Les valeurs des propriétés de clé d'entité doivent identifier de façon unique une instance de type d'entité dans un jeu d'entités au moment de l'exécution.</span><span class="sxs-lookup"><span data-stu-id="19669-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="19669-584">Les propriétés qui composent une clé d'entité doivent être choisies pour garantir l'unicité des instances dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="19669-585">Le **clé** élément définit une clé d’entité en faisant référence à un ou plusieurs des propriétés d’un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="19669-586">Le **clé** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="19669-587">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="19669-588">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-589">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-589">Applicable Attributes</span></span>

<span data-ttu-id="19669-590">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **clé** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="19669-591">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-592">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="19669-593">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-593">Example</span></span>

<span data-ttu-id="19669-594">L’exemple ci-dessous définit un type d’entité nommé **livre**.</span><span class="sxs-lookup"><span data-stu-id="19669-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="19669-595">La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="19669-596">Le **ISBN** propriété est un bon choix pour la clé d’entité, car un International Standard Book nombre (ISBN) identifie de façon unique un livre.</span><span class="sxs-lookup"><span data-stu-id="19669-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="19669-597">L’exemple suivant montre un type d’entité (**auteur**) qui a une clé d’entité qui se compose de deux propriétés : **nom** et **adresse**.</span><span class="sxs-lookup"><span data-stu-id="19669-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="19669-598">À l’aide de **nom** et **adresse** pour l’entité de clé est un choix raisonnable, étant donné que deux auteurs portant le même nom sont peu susceptibles de vie à la même adresse.</span><span class="sxs-lookup"><span data-stu-id="19669-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="19669-599">Toutefois, ce choix pour une clé d'entité ne garantit pas vraiment l'unicité des clés d'entité dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="19669-600">Ajout d’une propriété, tel que **AuthorId**, qui peut être utilisé pour identifier de manière unique un auteur est recommandé dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="19669-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="19669-601">Member, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-601">Member Element (CSDL)</span></span>

<span data-ttu-id="19669-602">Le **membre** élément est un élément enfant de l’élément EnumType et définit un membre du type énuméré.</span><span class="sxs-lookup"><span data-stu-id="19669-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-603">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-603">Applicable Attributes</span></span>

<span data-ttu-id="19669-604">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="19669-605">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-605">Attribute Name</span></span> | <span data-ttu-id="19669-606">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-606">Is Required</span></span> | <span data-ttu-id="19669-607">Value</span><span class="sxs-lookup"><span data-stu-id="19669-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-608">**Name**</span></span>       | <span data-ttu-id="19669-609">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-609">Yes</span></span>         | <span data-ttu-id="19669-610">Nom du membre.</span><span class="sxs-lookup"><span data-stu-id="19669-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="19669-611">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="19669-611">**Value**</span></span>      | <span data-ttu-id="19669-612">Non</span><span class="sxs-lookup"><span data-stu-id="19669-612">No</span></span>          | <span data-ttu-id="19669-613">Valeur du membre.</span><span class="sxs-lookup"><span data-stu-id="19669-613">The value of the member.</span></span> <span data-ttu-id="19669-614">Par défaut, le premier membre a la valeur 0, et la valeur de chaque énumérateur successif est incrémentée de 1.</span><span class="sxs-lookup"><span data-stu-id="19669-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="19669-615">Il existe peut-être plusieurs membres avec les mêmes valeurs.</span><span class="sxs-lookup"><span data-stu-id="19669-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-616">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="19669-617">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-618">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-619">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-619">Example</span></span>

<span data-ttu-id="19669-620">L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="19669-621">NavigationProperty, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="19669-622">Un **NavigationProperty** élément définit une propriété de navigation, qui fournit une référence à l’autre extrémité d’une association.</span><span class="sxs-lookup"><span data-stu-id="19669-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="19669-623">Contrairement aux propriétés définies par l’élément de propriété, les propriétés de navigation ne définissent pas la forme et les caractéristiques des données.</span><span class="sxs-lookup"><span data-stu-id="19669-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="19669-624">Elles fournissent un moyen d'explorer une association entre deux types d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="19669-625">Notez que les propriétés de navigation sont facultatives sur les deux types d'entités au niveau des terminaisons d'une association.</span><span class="sxs-lookup"><span data-stu-id="19669-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="19669-626">Si vous définissez une propriété de navigation sur un type d'entité au niveau de la terminaison d'une association, il n'est pas nécessaire de définir une propriété de navigation sur le type d'entité à l'autre terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="19669-627">Le type de données retourné par une propriété de navigation est déterminé par la multiplicité de sa terminaison d'association distante.</span><span class="sxs-lookup"><span data-stu-id="19669-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="19669-628">Par exemple, une propriété de navigation, **OrdersNavProp**, existe sur un **client** type d’entité et de naviguer d’une association un-à-plusieurs entre **client** et  **Commande**.</span><span class="sxs-lookup"><span data-stu-id="19669-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="19669-629">Étant donné que la terminaison d’association distante pour la propriété de navigation a une multiplicité plusieurs (\*), son type de données est une collection (de **ordre**).</span><span class="sxs-lookup"><span data-stu-id="19669-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="19669-630">De même, si une propriété de navigation, **CustomerNavProp**, existe sur le **ordre** type d’entité, son type de données serait **client** dans la mesure où la multiplicité de la terminaison distante est un (1).</span><span class="sxs-lookup"><span data-stu-id="19669-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="19669-631">Un **NavigationProperty** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-632">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-633">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-634">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-634">Applicable Attributes</span></span>

<span data-ttu-id="19669-635">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **NavigationProperty** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="19669-636">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-636">Attribute Name</span></span>   | <span data-ttu-id="19669-637">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-637">Is Required</span></span> | <span data-ttu-id="19669-638">Value</span><span class="sxs-lookup"><span data-stu-id="19669-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-639">**Name**</span></span>         | <span data-ttu-id="19669-640">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-640">Yes</span></span>         | <span data-ttu-id="19669-641">Nom de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="19669-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="19669-642">**Relation**</span><span class="sxs-lookup"><span data-stu-id="19669-642">**Relationship**</span></span> | <span data-ttu-id="19669-643">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-643">Yes</span></span>         | <span data-ttu-id="19669-644">Nom d'une association figurant dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="19669-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="19669-645">**ToRole**</span></span>       | <span data-ttu-id="19669-646">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-646">Yes</span></span>         | <span data-ttu-id="19669-647">Terminaison de l'association à laquelle la navigation prend fin.</span><span class="sxs-lookup"><span data-stu-id="19669-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="19669-648">La valeur de la **ToRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="19669-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="19669-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="19669-649">**FromRole**</span></span>     | <span data-ttu-id="19669-650">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-650">Yes</span></span>         | <span data-ttu-id="19669-651">Terminaison de l'association où la navigation commence.</span><span class="sxs-lookup"><span data-stu-id="19669-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="19669-652">La valeur de la **FromRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="19669-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-653">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **NavigationProperty** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="19669-654">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-655">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-656">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-656">Example</span></span>

<span data-ttu-id="19669-657">L’exemple suivant définit un type d’entité (**livre**) avec deux propriétés de navigation (**PublishedBy** et **inscrite**) :</span><span class="sxs-lookup"><span data-stu-id="19669-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="19669-658">OnDelete, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="19669-659">Le **OnDelete** élément dans le langage de définition de schéma conceptuel (CSDL) définit le comportement qui est connecté avec une association.</span><span class="sxs-lookup"><span data-stu-id="19669-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="19669-660">Si le **Action** attribut a la valeur **Cascade** sur une extrémité d’une association, liées à des types d’entités à l’autre extrémité de l’association sont supprimés lorsque le type d’entité de la première terminaison est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="19669-661">Si l’association entre deux types d’entité est une relation clé primaire / clé primaire, un objet dépendant chargé est supprimé lorsque l’objet principal à l’autre extrémité de l’association est supprimé, indépendamment de la **OnDelete** spécification.</span><span class="sxs-lookup"><span data-stu-id="19669-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="19669-662">Le **OnDelete** élément affecte uniquement le comportement d’exécution d’une application ; elle n’affecte pas le comportement de la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="19669-663">Le comportement défini dans la source de données doit être le même que le comportement défini dans l'application.</span><span class="sxs-lookup"><span data-stu-id="19669-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="19669-664">Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-665">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-666">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-667">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-667">Applicable Attributes</span></span>

<span data-ttu-id="19669-668">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="19669-669">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-669">Attribute Name</span></span> | <span data-ttu-id="19669-670">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-670">Is Required</span></span> | <span data-ttu-id="19669-671">Value</span><span class="sxs-lookup"><span data-stu-id="19669-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-672">**Action**</span><span class="sxs-lookup"><span data-stu-id="19669-672">**Action**</span></span>     | <span data-ttu-id="19669-673">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-673">Yes</span></span>         | <span data-ttu-id="19669-674">**Cascade** ou **aucun**.</span><span class="sxs-lookup"><span data-stu-id="19669-674">**Cascade** or **None**.</span></span> <span data-ttu-id="19669-675">Si **Cascade**, types d’entités dépendants sont supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="19669-676">Si **aucun**, types d’entités dépendants ne sont pas supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-677">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="19669-678">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-679">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-680">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-680">Example</span></span>

<span data-ttu-id="19669-681">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="19669-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="19669-682">Le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext seront supprimés lorsque le  **Client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="19669-683">Parameter, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="19669-684">Le **paramètre** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément FunctionImport ou l’élément Function.</span><span class="sxs-lookup"><span data-stu-id="19669-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="19669-685">Application de l'élément FunctionImport</span><span class="sxs-lookup"><span data-stu-id="19669-685">FunctionImport Element Application</span></span>

<span data-ttu-id="19669-686">Un **paramètre** élément (en tant qu’enfant de le **FunctionImport** élément) est utilisé pour définir les paramètres d’entrée et de sortie pour les importations de fonction qui sont déclarées dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="19669-687">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-688">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-689">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-690">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-690">Applicable Attributes</span></span>

<span data-ttu-id="19669-691">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="19669-692">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-692">Attribute Name</span></span> | <span data-ttu-id="19669-693">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-693">Is Required</span></span> | <span data-ttu-id="19669-694">Value</span><span class="sxs-lookup"><span data-stu-id="19669-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-695">**Name**</span></span>       | <span data-ttu-id="19669-696">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-696">Yes</span></span>         | <span data-ttu-id="19669-697">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="19669-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-698">**Type**</span></span>       | <span data-ttu-id="19669-699">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-699">Yes</span></span>         | <span data-ttu-id="19669-700">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-700">The parameter type.</span></span> <span data-ttu-id="19669-701">La valeur doit être un **EDMSimpleType** ou un type complexe qui est dans l’étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="19669-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="19669-702">**Mode**</span></span>       | <span data-ttu-id="19669-703">Non</span><span class="sxs-lookup"><span data-stu-id="19669-703">No</span></span>          | <span data-ttu-id="19669-704">**Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="19669-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="19669-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-705">**MaxLength**</span></span>  | <span data-ttu-id="19669-706">Non</span><span class="sxs-lookup"><span data-stu-id="19669-706">No</span></span>          | <span data-ttu-id="19669-707">Longueur maximale autorisée du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="19669-708">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-708">**Precision**</span></span>  | <span data-ttu-id="19669-709">Non</span><span class="sxs-lookup"><span data-stu-id="19669-709">No</span></span>          | <span data-ttu-id="19669-710">Précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="19669-711">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-711">**Scale**</span></span>      | <span data-ttu-id="19669-712">Non</span><span class="sxs-lookup"><span data-stu-id="19669-712">No</span></span>          | <span data-ttu-id="19669-713">Échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="19669-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-714">**SRID**</span></span>       | <span data-ttu-id="19669-715">Non</span><span class="sxs-lookup"><span data-stu-id="19669-715">No</span></span>          | <span data-ttu-id="19669-716">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-717">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="19669-718">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-719">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="19669-720">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-721">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-722">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-722">Example</span></span>

<span data-ttu-id="19669-723">L’exemple suivant montre un **FunctionImport** élément avec un **paramètre** élément enfant.</span><span class="sxs-lookup"><span data-stu-id="19669-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="19669-724">La fonction accepte un paramètre d'entrée et retourne une collection de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="19669-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="19669-725">Application de l'élément Function</span><span class="sxs-lookup"><span data-stu-id="19669-725">Function Element Application</span></span>

<span data-ttu-id="19669-726">Un **paramètre** élément (en tant qu’enfant de le **fonction** élément) définit les paramètres pour les fonctions qui sont définies ou déclarées dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="19669-727">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-728">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="19669-729">CollectionType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="19669-730">ReferenceType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="19669-731">RowType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="19669-732">Seul l’un de la **CollectionType**, **ReferenceType**, ou **RowType** éléments peuvent être un élément enfant d’un **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="19669-733">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="19669-734">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="19669-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="19669-735">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="19669-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-736">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-736">Applicable Attributes</span></span>

<span data-ttu-id="19669-737">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="19669-738">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-738">Attribute Name</span></span>   | <span data-ttu-id="19669-739">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-739">Is Required</span></span> | <span data-ttu-id="19669-740">Value</span><span class="sxs-lookup"><span data-stu-id="19669-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-741">**Name**</span></span>         | <span data-ttu-id="19669-742">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-742">Yes</span></span>         | <span data-ttu-id="19669-743">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="19669-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-744">**Type**</span></span>         | <span data-ttu-id="19669-745">Non</span><span class="sxs-lookup"><span data-stu-id="19669-745">No</span></span>          | <span data-ttu-id="19669-746">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="19669-746">The parameter type.</span></span> <span data-ttu-id="19669-747">Un paramètre peut correspondre à l'un quelconque des types suivants (ou à des collections de ces types) :</span><span class="sxs-lookup"><span data-stu-id="19669-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="19669-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="19669-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="19669-749">type d'entité</span><span class="sxs-lookup"><span data-stu-id="19669-749">entity type</span></span> <br/> <span data-ttu-id="19669-750">type complexe</span><span class="sxs-lookup"><span data-stu-id="19669-750">complex type</span></span> <br/> <span data-ttu-id="19669-751">type de ligne</span><span class="sxs-lookup"><span data-stu-id="19669-751">row type</span></span> <br/> <span data-ttu-id="19669-752">type référence</span><span class="sxs-lookup"><span data-stu-id="19669-752">reference type</span></span>                             |
| <span data-ttu-id="19669-753">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-753">**Nullable**</span></span>     | <span data-ttu-id="19669-754">Non</span><span class="sxs-lookup"><span data-stu-id="19669-754">No</span></span>          | <span data-ttu-id="19669-755">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir un **null** valeur.</span><span class="sxs-lookup"><span data-stu-id="19669-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="19669-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="19669-756">**DefaultValue**</span></span> | <span data-ttu-id="19669-757">Non</span><span class="sxs-lookup"><span data-stu-id="19669-757">No</span></span>          | <span data-ttu-id="19669-758">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="19669-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-759">**MaxLength**</span></span>    | <span data-ttu-id="19669-760">Non</span><span class="sxs-lookup"><span data-stu-id="19669-760">No</span></span>          | <span data-ttu-id="19669-761">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="19669-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-762">**FixedLength**</span></span>  | <span data-ttu-id="19669-763">Non</span><span class="sxs-lookup"><span data-stu-id="19669-763">No</span></span>          | <span data-ttu-id="19669-764">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="19669-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="19669-765">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-765">**Precision**</span></span>    | <span data-ttu-id="19669-766">Non</span><span class="sxs-lookup"><span data-stu-id="19669-766">No</span></span>          | <span data-ttu-id="19669-767">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="19669-768">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-768">**Scale**</span></span>        | <span data-ttu-id="19669-769">Non</span><span class="sxs-lookup"><span data-stu-id="19669-769">No</span></span>          | <span data-ttu-id="19669-770">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="19669-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-771">**SRID**</span></span>         | <span data-ttu-id="19669-772">Non</span><span class="sxs-lookup"><span data-stu-id="19669-772">No</span></span>          | <span data-ttu-id="19669-773">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-774">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="19669-775">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="19669-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-776">**Unicode**</span></span>      | <span data-ttu-id="19669-777">Non</span><span class="sxs-lookup"><span data-stu-id="19669-777">No</span></span>          | <span data-ttu-id="19669-778">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="19669-779">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-779">**Collation**</span></span>    | <span data-ttu-id="19669-780">Non</span><span class="sxs-lookup"><span data-stu-id="19669-780">No</span></span>          | <span data-ttu-id="19669-781">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="19669-782">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="19669-783">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-784">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-785">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-785">Example</span></span>

<span data-ttu-id="19669-786">L’exemple suivant montre un **fonction** élément qui utilise un **paramètre** élément enfant pour définir un paramètre de fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="19669-787">Principal, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="19669-788">Le **Principal** en langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="19669-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="19669-789">Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="19669-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="19669-790">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="19669-791">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="19669-792">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="19669-793">**PropertyRef** éléments sont utilisés pour spécifier les clés référencées par la terminaison dépendante.</span><span class="sxs-lookup"><span data-stu-id="19669-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="19669-794">Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-795">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="19669-796">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-797">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-797">Applicable Attributes</span></span>

<span data-ttu-id="19669-798">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="19669-799">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-799">Attribute Name</span></span> | <span data-ttu-id="19669-800">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-800">Is Required</span></span> | <span data-ttu-id="19669-801">Value</span><span class="sxs-lookup"><span data-stu-id="19669-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="19669-802">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="19669-802">**Role**</span></span>       | <span data-ttu-id="19669-803">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-803">Yes</span></span>         | <span data-ttu-id="19669-804">Nom du type d'entité au niveau de la terminaison principale de l'association.</span><span class="sxs-lookup"><span data-stu-id="19669-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-805">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="19669-806">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-807">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-808">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-808">Example</span></span>

<span data-ttu-id="19669-809">L’exemple suivant montre un **ReferentialConstraint** élément qui fait partie de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="19669-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="19669-810">Le **Id** propriété de la **Publisher** type d’entité compose la terminaison principale de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="19669-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="19669-811">Property, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-811">Property Element (CSDL)</span></span>

<span data-ttu-id="19669-812">Le **propriété** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément EntityType, ComplexType, élément ou l’élément RowType.</span><span class="sxs-lookup"><span data-stu-id="19669-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="19669-813">Applications des éléments EntityType et ComplexType</span><span class="sxs-lookup"><span data-stu-id="19669-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="19669-814">**Propriété** éléments (en tant qu’enfants de **EntityType** ou **ComplexType** éléments) définissent la forme et les caractéristiques des données qui contient une instance de type d’entité ou d’une instance de type complexe .</span><span class="sxs-lookup"><span data-stu-id="19669-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="19669-815">Les propriétés dans un modèle conceptuel sont analogues aux propriétés qui sont définies sur une classe.</span><span class="sxs-lookup"><span data-stu-id="19669-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="19669-816">De même que les propriétés sur une classe définissent la forme de la classe et acheminent des informations sur les objets, les propriétés dans un modèle conceptuel définissent la forme d'un type d'entité et acheminent des informations sur les instances de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="19669-817">Le **propriété** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-818">Documentation, élément (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-819">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="19669-820">Les facettes suivantes peuvent être appliqués à un **propriété** élément : **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **précision**, **mise à l’échelle**, **Unicode**, **classement**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="19669-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="19669-821">Les facettes sont des attributs XML qui fournissent des informations sur la manière dont les valeurs de propriété sont stockées dans la banque de données.</span><span class="sxs-lookup"><span data-stu-id="19669-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-822">Facettes peuvent uniquement être appliqués aux propriétés de type **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="19669-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-823">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-823">Applicable Attributes</span></span>

<span data-ttu-id="19669-824">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="19669-825">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-825">Attribute Name</span></span>                                                         | <span data-ttu-id="19669-826">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-826">Is Required</span></span> | <span data-ttu-id="19669-827">Value</span><span class="sxs-lookup"><span data-stu-id="19669-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-828">**Name**</span></span>                                                               | <span data-ttu-id="19669-829">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-829">Yes</span></span>         | <span data-ttu-id="19669-830">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="19669-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-831">**Type**</span></span>                                                               | <span data-ttu-id="19669-832">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-832">Yes</span></span>         | <span data-ttu-id="19669-833">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-833">The type of the property value.</span></span> <span data-ttu-id="19669-834">Le type de valeur de propriété doit être un **EDMSimpleType** ou un type complexe (indiqué par un nom qualifié complet) qui se trouve dans la portée du modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="19669-835">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-835">**Nullable**</span></span>                                                           | <span data-ttu-id="19669-836">Non</span><span class="sxs-lookup"><span data-stu-id="19669-836">No</span></span>          | <span data-ttu-id="19669-837">**True** (la valeur par défaut) ou <strong>False</strong> selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="19669-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="19669-838">> Dans le langage CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="19669-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="19669-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="19669-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="19669-840">Non</span><span class="sxs-lookup"><span data-stu-id="19669-840">No</span></span>          | <span data-ttu-id="19669-841">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="19669-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="19669-843">Non</span><span class="sxs-lookup"><span data-stu-id="19669-843">No</span></span>          | <span data-ttu-id="19669-844">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="19669-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="19669-846">Non</span><span class="sxs-lookup"><span data-stu-id="19669-846">No</span></span>          | <span data-ttu-id="19669-847">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="19669-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="19669-848">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-848">**Precision**</span></span>                                                          | <span data-ttu-id="19669-849">Non</span><span class="sxs-lookup"><span data-stu-id="19669-849">No</span></span>          | <span data-ttu-id="19669-850">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="19669-851">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-851">**Scale**</span></span>                                                              | <span data-ttu-id="19669-852">Non</span><span class="sxs-lookup"><span data-stu-id="19669-852">No</span></span>          | <span data-ttu-id="19669-853">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="19669-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-854">**SRID**</span></span>                                                               | <span data-ttu-id="19669-855">Non</span><span class="sxs-lookup"><span data-stu-id="19669-855">No</span></span>          | <span data-ttu-id="19669-856">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-857">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="19669-858">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="19669-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-859">**Unicode**</span></span>                                                            | <span data-ttu-id="19669-860">Non</span><span class="sxs-lookup"><span data-stu-id="19669-860">No</span></span>          | <span data-ttu-id="19669-861">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="19669-862">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-862">**Collation**</span></span>                                                          | <span data-ttu-id="19669-863">Non</span><span class="sxs-lookup"><span data-stu-id="19669-863">No</span></span>          | <span data-ttu-id="19669-864">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="19669-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="19669-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="19669-866">Non</span><span class="sxs-lookup"><span data-stu-id="19669-866">No</span></span>          | <span data-ttu-id="19669-867">**Aucun** (la valeur par défaut) ou **fixe**.</span><span class="sxs-lookup"><span data-stu-id="19669-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="19669-868">Si la valeur est définie sur **fixe**, la valeur de propriété sera utilisée dans les contrôles d’accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="19669-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="19669-869">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="19669-870">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-871">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-872">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-872">Example</span></span>

<span data-ttu-id="19669-873">L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="19669-874">L’exemple suivant montre un **ComplexType** élément avec cinq **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="19669-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="19669-875">Application de l'élément RowType</span><span class="sxs-lookup"><span data-stu-id="19669-875">RowType Element Application</span></span>

<span data-ttu-id="19669-876">**Propriété** éléments (comme enfants d’un **RowType** élément) définissent la forme et les caractéristiques des données qui peuvent être passées à ou retournées à partir d’une fonction définie par le modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="19669-877">Le **propriété** élément peut avoir exactement un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="19669-878">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="19669-878">CollectionType</span></span>
-   <span data-ttu-id="19669-879">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="19669-879">ReferenceType</span></span>
-   <span data-ttu-id="19669-880">RowType ;</span><span class="sxs-lookup"><span data-stu-id="19669-880">RowType</span></span>

<span data-ttu-id="19669-881">Le **propriété** élément peut avoir n’importe quel nombre éléments d’annotation enfant.</span><span class="sxs-lookup"><span data-stu-id="19669-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-882">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="19669-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="19669-883">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-883">Applicable Attributes</span></span>

<span data-ttu-id="19669-884">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="19669-885">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-885">Attribute Name</span></span>                                                     | <span data-ttu-id="19669-886">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-886">Is Required</span></span> | <span data-ttu-id="19669-887">Value</span><span class="sxs-lookup"><span data-stu-id="19669-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-888">**Name**</span></span>                                                           | <span data-ttu-id="19669-889">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-889">Yes</span></span>         | <span data-ttu-id="19669-890">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="19669-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-891">**Type**</span></span>                                                           | <span data-ttu-id="19669-892">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-892">Yes</span></span>         | <span data-ttu-id="19669-893">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="19669-894">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-894">**Nullable**</span></span>                                                       | <span data-ttu-id="19669-895">Non</span><span class="sxs-lookup"><span data-stu-id="19669-895">No</span></span>          | <span data-ttu-id="19669-896">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="19669-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="19669-897">> Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="19669-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="19669-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="19669-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="19669-899">Non</span><span class="sxs-lookup"><span data-stu-id="19669-899">No</span></span>          | <span data-ttu-id="19669-900">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="19669-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="19669-902">Non</span><span class="sxs-lookup"><span data-stu-id="19669-902">No</span></span>          | <span data-ttu-id="19669-903">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="19669-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="19669-905">Non</span><span class="sxs-lookup"><span data-stu-id="19669-905">No</span></span>          | <span data-ttu-id="19669-906">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="19669-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="19669-907">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-907">**Precision**</span></span>                                                      | <span data-ttu-id="19669-908">Non</span><span class="sxs-lookup"><span data-stu-id="19669-908">No</span></span>          | <span data-ttu-id="19669-909">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="19669-910">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-910">**Scale**</span></span>                                                          | <span data-ttu-id="19669-911">Non</span><span class="sxs-lookup"><span data-stu-id="19669-911">No</span></span>          | <span data-ttu-id="19669-912">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="19669-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-913">**SRID**</span></span>                                                           | <span data-ttu-id="19669-914">Non</span><span class="sxs-lookup"><span data-stu-id="19669-914">No</span></span>          | <span data-ttu-id="19669-915">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-916">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="19669-917">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="19669-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-918">**Unicode**</span></span>                                                        | <span data-ttu-id="19669-919">Non</span><span class="sxs-lookup"><span data-stu-id="19669-919">No</span></span>          | <span data-ttu-id="19669-920">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="19669-921">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-921">**Collation**</span></span>                                                      | <span data-ttu-id="19669-922">Non</span><span class="sxs-lookup"><span data-stu-id="19669-922">No</span></span>          | <span data-ttu-id="19669-923">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="19669-924">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="19669-925">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-926">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="19669-927">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-927">Example</span></span>

<span data-ttu-id="19669-928">L’exemple suivant **propriété** éléments utilisés pour définir la forme du type de retour d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="19669-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="19669-929">PropertyRef, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="19669-930">Le **PropertyRef** élément dans le langage de définition de schéma conceptuel (CSDL) fait référence à une propriété d’un type d’entité pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="19669-931">Partie de la clé de l'entité (une propriété ou un jeu de propriétés d'un type d'entité qui détermine l'identité).</span><span class="sxs-lookup"><span data-stu-id="19669-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="19669-932">Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="19669-933">Terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="19669-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="19669-934">Le **PropertyRef** élément peut avoir uniquement des éléments d’annotation (zéro ou plus) en tant qu’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="19669-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-935">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="19669-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="19669-936">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-936">Applicable Attributes</span></span>

<span data-ttu-id="19669-937">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="19669-938">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-938">Attribute Name</span></span> | <span data-ttu-id="19669-939">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-939">Is Required</span></span> | <span data-ttu-id="19669-940">Value</span><span class="sxs-lookup"><span data-stu-id="19669-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="19669-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="19669-941">**Name**</span></span>       | <span data-ttu-id="19669-942">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-942">Yes</span></span>         | <span data-ttu-id="19669-943">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="19669-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-944">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="19669-945">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-946">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-947">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-947">Example</span></span>

<span data-ttu-id="19669-948">L’exemple ci-dessous définit un type d’entité (**livre**).</span><span class="sxs-lookup"><span data-stu-id="19669-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="19669-949">La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="19669-950">Dans l’exemple suivant, deux **PropertyRef** éléments sont utilisés pour indiquer que deux propriétés (**Id** et **PublisherId**) sont les terminaisons principales et dépendantes d’un référentiel contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="19669-951">Élément ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="19669-952">Le **ReferenceType** élément de langage de définition de schéma conceptuel (CSDL) spécifie une référence à un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="19669-953">Le **ReferenceType** élément peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="19669-954">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="19669-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="19669-955">Paramètre</span><span class="sxs-lookup"><span data-stu-id="19669-955">Parameter</span></span>
-   <span data-ttu-id="19669-956">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="19669-956">CollectionType</span></span>

<span data-ttu-id="19669-957">Le **ReferenceType** élément est utilisé lors de la définition d’un type de paramètre ou de retour pour une fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="19669-958">Un **ReferenceType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-959">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-960">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-961">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-961">Applicable Attributes</span></span>

<span data-ttu-id="19669-962">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ReferenceType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="19669-963">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-963">Attribute Name</span></span> | <span data-ttu-id="19669-964">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-964">Is Required</span></span> | <span data-ttu-id="19669-965">Value</span><span class="sxs-lookup"><span data-stu-id="19669-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="19669-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-966">**Type**</span></span>       | <span data-ttu-id="19669-967">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-967">Yes</span></span>         | <span data-ttu-id="19669-968">Nom du type d'entité référencé.</span><span class="sxs-lookup"><span data-stu-id="19669-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-969">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferenceType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="19669-970">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-971">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-972">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-972">Example</span></span>

<span data-ttu-id="19669-973">L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **paramètre** élément dans une fonction définie par modèle qui accepte une référence à un **personne** entité type :</span><span class="sxs-lookup"><span data-stu-id="19669-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="19669-974">L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **ReturnType** élément (fonction) dans une fonction définie par modèle qui retourne une référence à un **personne**type d’entité :</span><span class="sxs-lookup"><span data-stu-id="19669-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="19669-975">ReferentialConstraint, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="19669-976">Un **ReferentialConstraint** élément dans le langage de définition de schéma conceptuel (CSDL) définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="19669-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="19669-977">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="19669-978">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="19669-979">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="19669-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="19669-980">Si une clé étrangère qui est exposée sur un type d’entité fait référence à une propriété sur un autre type d’entité, le **ReferentialConstraint** élément définit une association entre les deux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="19669-981">Étant donné que le **ReferentialConstraint** élément fournit des informations sur la manière dont deux types d’entités sont liées, aucun élément AssociationSetMapping correspondant n’est nécessaire dans le langage de spécification de mappage (MSL).</span><span class="sxs-lookup"><span data-stu-id="19669-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="19669-982">Une association entre deux types d’entités qui n’ont pas de clés étrangères exposées doit avoir un correspondant **AssociationSetMapping** élément afin de mapper les informations d’association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="19669-983">Si une clé étrangère n’est pas exposée sur un type d’entité, le **ReferentialConstraint** élément peut uniquement définir une contrainte de clé primaire / clé primaire entre le type d’entité et un autre type d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="19669-984">Un **ReferentialConstraint** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-985">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-986">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="19669-987">Dépendante (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="19669-988">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-989">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-989">Applicable Attributes</span></span>

<span data-ttu-id="19669-990">Le **ReferentialConstraint** élément peut avoir n’importe quel nombre d’attributs d’annotation (attributs XML personnalisés).</span><span class="sxs-lookup"><span data-stu-id="19669-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="19669-991">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-992">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="19669-993">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-993">Example</span></span>

<span data-ttu-id="19669-994">L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="19669-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="19669-995">Élément ReturnType (fonction) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="19669-996">Le **ReturnType** (Function), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément de la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="19669-997">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="19669-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="19669-998">Retourner les types peuvent être les **EdmSimpleType**, type d’entité, type complexe, type de ligne, type ref ou une collection d’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="19669-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="19669-999">Le type de retour d’une fonction peut être spécifié avec soit le **Type** attribut de la **ReturnType** élément (fonction), ou avec l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="19669-1000">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="19669-1000">CollectionType</span></span>
-   <span data-ttu-id="19669-1001">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="19669-1001">ReferenceType</span></span>
-   <span data-ttu-id="19669-1002">RowType ;</span><span class="sxs-lookup"><span data-stu-id="19669-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="19669-1003">Un modèle ne sera pas validé si vous spécifiez une fonction de type de retour avec à la fois le **Type** attribut de la **ReturnType** élément (Function) et l’autre des éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="19669-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1004">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1004">Applicable Attributes</span></span>

<span data-ttu-id="19669-1005">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="19669-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="19669-1006">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-1006">Attribute Name</span></span> | <span data-ttu-id="19669-1007">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-1007">Is Required</span></span> | <span data-ttu-id="19669-1008">Value</span><span class="sxs-lookup"><span data-stu-id="19669-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="19669-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="19669-1009">**ReturnType**</span></span> | <span data-ttu-id="19669-1010">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1010">No</span></span>          | <span data-ttu-id="19669-1011">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-1012">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="19669-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="19669-1013">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1014">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-1015">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1015">Example</span></span>

<span data-ttu-id="19669-1016">L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années un livre a été imprimé.</span><span class="sxs-lookup"><span data-stu-id="19669-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="19669-1017">Notez que le type de retour est spécifié par le **Type** attribut d’un **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="19669-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="19669-1018">ReturnType (FunctionImport) élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="19669-1019">Le **ReturnType** (FunctionImport), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="19669-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="19669-1020">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="19669-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="19669-1021">Retourner les types peuvent être n’importe quelle collection de type d’entité, type complexe, ou **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="19669-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="19669-1022">Le type de retour d’une fonction est spécifié avec la **Type** attribut de la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1023">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1023">Applicable Attributes</span></span>

<span data-ttu-id="19669-1024">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="19669-1025">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-1025">Attribute Name</span></span> | <span data-ttu-id="19669-1026">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-1026">Is Required</span></span> | <span data-ttu-id="19669-1027">Value</span><span class="sxs-lookup"><span data-stu-id="19669-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-1028">**Type**</span></span>       | <span data-ttu-id="19669-1029">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1029">No</span></span>          | <span data-ttu-id="19669-1030">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="19669-1030">The type that the function returns.</span></span> <span data-ttu-id="19669-1031">La valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="19669-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="19669-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="19669-1032">**EntitySet**</span></span>  | <span data-ttu-id="19669-1033">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1033">No</span></span>          | <span data-ttu-id="19669-1034">Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection.</span><span class="sxs-lookup"><span data-stu-id="19669-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="19669-1035">Sinon, le **EntitySet** attribut ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="19669-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-1036">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="19669-1037">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1038">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-1039">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1039">Example</span></span>

<span data-ttu-id="19669-1040">L’exemple suivant utilise un **FunctionImport** qui retourne la documentation et les serveurs de publication.</span><span class="sxs-lookup"><span data-stu-id="19669-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="19669-1041">Notez que la fonction retourne deux jeux de résultats et par conséquent deux **ReturnType** (FunctionImport) éléments sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="19669-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="19669-1042">Élément RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="19669-1043">Un **RowType** élément dans le langage de définition de schéma conceptuel (CSDL) définit une structure sans nom comme paramètre ou type de retour pour une fonction définie dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="19669-1044">Un **RowType** élément peut être l’enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="19669-1045">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="19669-1045">CollectionType</span></span>
-   <span data-ttu-id="19669-1046">Paramètre</span><span class="sxs-lookup"><span data-stu-id="19669-1046">Parameter</span></span>
-   <span data-ttu-id="19669-1047">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="19669-1047">ReturnType (Function)</span></span>

<span data-ttu-id="19669-1048">Un **RowType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-1049">Propriété (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="19669-1049">Property (one or more)</span></span>
-   <span data-ttu-id="19669-1050">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="19669-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1051">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1051">Applicable Attributes</span></span>

<span data-ttu-id="19669-1052">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **RowType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="19669-1053">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1054">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="19669-1055">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1055">Example</span></span>

<span data-ttu-id="19669-1056">L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="19669-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="19669-1057">Schema, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="19669-1058">Le **schéma** élément est l’élément racine d’une définition de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="19669-1059">Il contient des définitions pour les objets, fonctions et conteneurs qui composent un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="19669-1060">Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="19669-1061">Using</span><span class="sxs-lookup"><span data-stu-id="19669-1061">Using</span></span>
-   <span data-ttu-id="19669-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="19669-1062">EntityContainer</span></span>
-   <span data-ttu-id="19669-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="19669-1063">EntityType</span></span>
-   <span data-ttu-id="19669-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="19669-1064">EnumType</span></span>
-   <span data-ttu-id="19669-1065">Association</span><span class="sxs-lookup"><span data-stu-id="19669-1065">Association</span></span>
-   <span data-ttu-id="19669-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="19669-1066">ComplexType</span></span>
-   <span data-ttu-id="19669-1067">Fonction</span><span class="sxs-lookup"><span data-stu-id="19669-1067">Function</span></span>

<span data-ttu-id="19669-1068">Un **schéma** élément peut contenir zéro ou un élément d’Annotation.</span><span class="sxs-lookup"><span data-stu-id="19669-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-1069">Le **fonction** élément et les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="19669-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="19669-1070">Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour le type d’entité, type complexe et objets d’association dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="19669-1071">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="19669-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="19669-1072">Espaces de noms peuvent couvrir plusieurs **schéma** éléments et plusieurs fichiers .csdl.</span><span class="sxs-lookup"><span data-stu-id="19669-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="19669-1073">Un espace de noms du modèle conceptuel est différent de l’espace de noms XML de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="19669-1074">Un espace de noms de modèle conceptuel (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités, types complexes et types d’associations.</span><span class="sxs-lookup"><span data-stu-id="19669-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="19669-1075">L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="19669-1076">Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="19669-1077">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="19669-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1078">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1078">Applicable Attributes</span></span>

<span data-ttu-id="19669-1079">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="19669-1080">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-1080">Attribute Name</span></span> | <span data-ttu-id="19669-1081">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-1081">Is Required</span></span> | <span data-ttu-id="19669-1082">Value</span><span class="sxs-lookup"><span data-stu-id="19669-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-1083">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="19669-1083">**Namespace**</span></span>  | <span data-ttu-id="19669-1084">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1084">Yes</span></span>         | <span data-ttu-id="19669-1085">Espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="19669-1086">La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="19669-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="19669-1087">Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms Simple.Example.Model, le nom qualifié complet de le **EntityType** est SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="19669-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="19669-1088">Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="19669-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="19669-1089">La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma SSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="19669-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="19669-1090">**Alias**</span></span>      | <span data-ttu-id="19669-1091">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1091">No</span></span>          | <span data-ttu-id="19669-1092">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="19669-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="19669-1093">Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms Simple.Example.Model et la valeur de la **Alias** attribut est *modèle*, vous pouvez utiliser modèle.client comme nom qualifié complet de le **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="19669-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="19669-1094">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="19669-1095">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1096">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-1097">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1097">Example</span></span>

<span data-ttu-id="19669-1098">L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="19669-1099">Élément TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="19669-1100">Le **TypeRef** élément dans le langage de définition de schéma conceptuel (CSDL) fournit une référence à un type nommé existant.</span><span class="sxs-lookup"><span data-stu-id="19669-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="19669-1101">Le **TypeRef** élément peut être un enfant de l’élément CollectionType, qui est utilisé pour spécifier qu’une fonction a une collection comme un paramètre ou type de retour.</span><span class="sxs-lookup"><span data-stu-id="19669-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="19669-1102">Un **TypeRef** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="19669-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="19669-1103">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="19669-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="19669-1104">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="19669-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1105">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1105">Applicable Attributes</span></span>

<span data-ttu-id="19669-1106">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **TypeRef** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="19669-1107">Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables à **types Edmsimpletype**.</span><span class="sxs-lookup"><span data-stu-id="19669-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="19669-1108">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="19669-1109">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-1109">Is Required</span></span> | <span data-ttu-id="19669-1110">Value</span><span class="sxs-lookup"><span data-stu-id="19669-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="19669-1111">**Type**</span></span>                                                           | <span data-ttu-id="19669-1112">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1112">No</span></span>          | <span data-ttu-id="19669-1113">Nom du type référencé.</span><span class="sxs-lookup"><span data-stu-id="19669-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="19669-1114">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="19669-1115">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1115">No</span></span>          | <span data-ttu-id="19669-1116">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="19669-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="19669-1117">> Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="19669-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="19669-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="19669-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="19669-1119">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1119">No</span></span>          | <span data-ttu-id="19669-1120">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="19669-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="19669-1122">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1122">No</span></span>          | <span data-ttu-id="19669-1123">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="19669-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="19669-1125">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1125">No</span></span>          | <span data-ttu-id="19669-1126">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="19669-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="19669-1127">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-1127">**Precision**</span></span>                                                      | <span data-ttu-id="19669-1128">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1128">No</span></span>          | <span data-ttu-id="19669-1129">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="19669-1130">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-1130">**Scale**</span></span>                                                          | <span data-ttu-id="19669-1131">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1131">No</span></span>          | <span data-ttu-id="19669-1132">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="19669-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-1133">**SRID**</span></span>                                                           | <span data-ttu-id="19669-1134">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1134">No</span></span>          | <span data-ttu-id="19669-1135">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="19669-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="19669-1136">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="19669-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="19669-1137">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="19669-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="19669-1139">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1139">No</span></span>          | <span data-ttu-id="19669-1140">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="19669-1141">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-1141">**Collation**</span></span>                                                      | <span data-ttu-id="19669-1142">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1142">No</span></span>          | <span data-ttu-id="19669-1143">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="19669-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="19669-1144">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="19669-1145">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1146">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-1147">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1147">Example</span></span>

<span data-ttu-id="19669-1148">L’exemple suivant montre une fonction définie par modèle qui utilise le **TypeRef** élément (en tant qu’enfant d’un **CollectionType** élément) pour spécifier que la fonction accepte une collection de  **Département** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="19669-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="19669-1149">Using, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="19669-1150">Le **Using** élément dans le langage de définition de schéma conceptuel (CSDL) importe le contenu d’un modèle conceptuel qui existe dans un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="19669-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="19669-1151">En définissant la valeur de la **Namespace** attribut, vous pouvez faire référence à des types d’entités, types complexes et les types d’associations sont définies dans un autre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="19669-1152">Plusieurs **Using** élément peut être un enfant d’un **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-1153">Le **Using** élément dans le langage CSDL ne fonctionne pas exactement comme un **à l’aide de** instruction dans un langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="19669-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="19669-1154">En important un espace de noms avec un **à l’aide de** instruction dans un langage de programmation, vous n’affectez pas les objets dans l’espace de noms d’origine.</span><span class="sxs-lookup"><span data-stu-id="19669-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="19669-1155">Dans le langage CSDL, un espace de noms importé peut contenir un type d'entité dérivé d'un type d'entité figurant dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="19669-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="19669-1156">Cela peut affecter les jeux d'entités déclarés dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="19669-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="19669-1157">Le **Using** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="19669-1158">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="19669-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="19669-1159">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="19669-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="19669-1160">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1160">Applicable Attributes</span></span>

<span data-ttu-id="19669-1161">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="19669-1162">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="19669-1162">Attribute Name</span></span> | <span data-ttu-id="19669-1163">Requis</span><span class="sxs-lookup"><span data-stu-id="19669-1163">Is Required</span></span> | <span data-ttu-id="19669-1164">Value</span><span class="sxs-lookup"><span data-stu-id="19669-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19669-1165">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="19669-1165">**Namespace**</span></span>  | <span data-ttu-id="19669-1166">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1166">Yes</span></span>         | <span data-ttu-id="19669-1167">Nom de l'espace de noms importé.</span><span class="sxs-lookup"><span data-stu-id="19669-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="19669-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="19669-1168">**Alias**</span></span>      | <span data-ttu-id="19669-1169">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1169">Yes</span></span>         | <span data-ttu-id="19669-1170">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="19669-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="19669-1171">Bien que cet attribut soit obligatoire, il n'est pas nécessaire qu'il soit utilisé à la place du nom de l'espace de noms pour qualifier les noms d'objets.</span><span class="sxs-lookup"><span data-stu-id="19669-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="19669-1172">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="19669-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="19669-1173">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="19669-1174">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="19669-1175">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1175">Example</span></span>

<span data-ttu-id="19669-1176">L’exemple suivant montre le **Using** élément utilisé pour importer un espace de noms qui est défini ailleurs.</span><span class="sxs-lookup"><span data-stu-id="19669-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="19669-1177">Notez que l’espace de noms pour le **schéma** élément indiqué est `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="19669-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="19669-1178">Le `Address` propriété sur le `Publisher` **EntityType** est un type complexe qui est défini dans le `ExtendedBooksModel` espace de noms (importé avec le **Using** élément).</span><span class="sxs-lookup"><span data-stu-id="19669-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="19669-1179">Attributs d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="19669-1180">Les attributs d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des attributs XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="19669-1181">En plus d'avoir une structure XML valide, les attributs d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="19669-1182">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="19669-1183">Plusieurs attributs d'annotation peuvent être appliqués à un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="19669-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="19669-1184">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="19669-1185">Les attributs d'annotation peuvent être utilisés pour fournir des métadonnées supplémentaires sur des éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="19669-1186">Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="19669-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="19669-1187">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1187">Example</span></span>

<span data-ttu-id="19669-1188">L’exemple suivant montre un **EntityType** élément avec un attribut d’annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="19669-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="19669-1189">L'exemple fait également apparaître un élément d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="19669-1190">Le code suivant récupère les métadonnées dans l'attribut d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="19669-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="19669-1191">Le code ci-dessus suppose que le fichier `School.csdl` se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="19669-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="19669-1192">Éléments d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="19669-1193">Les éléments d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des éléments XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="19669-1194">En plus d'avoir une structure XML valide, les éléments d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="19669-1195">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="19669-1196">Plusieurs éléments d'annotation peuvent être des enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="19669-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="19669-1197">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="19669-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="19669-1198">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="19669-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="19669-1199">Les éléments d'annotation permettent de fournir des métadonnées supplémentaires sur les éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="19669-1200">À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="19669-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="19669-1201">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1201">Example</span></span>

<span data-ttu-id="19669-1202">L’exemple suivant montre un **EntityType** élément avec un élément d’annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="19669-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="19669-1203">L'exemple fait également apparaître un attribut d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="19669-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="19669-1204">Le code suivant récupère les métadonnées dans l'élément d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="19669-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="19669-1205">Le code ci-dessus suppose que le fichier School.csdl se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="19669-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="19669-1206">Types de modèles conceptuels (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="19669-1207">Langage de définition de schéma conceptuel (CSDL) prend en charge un ensemble de types de données primitif abstrait, appelé **types Edmsimpletype**, qui définissent des propriétés dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="19669-1208">**Types Edmsimpletype** sont des proxys pour les types de données primitifs sont prises en charge dans le stockage ou d’un environnement d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="19669-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="19669-1209">Le tableau suivant répertorie les types de données primitifs pris en charge par CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="19669-1210">Le tableau répertorie également les facettes peuvent être appliquées à chaque **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="19669-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="19669-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="19669-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="19669-1212">Description</span><span class="sxs-lookup"><span data-stu-id="19669-1212">Description</span></span>                                                | <span data-ttu-id="19669-1213">Facettes applicables</span><span class="sxs-lookup"><span data-stu-id="19669-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="19669-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="19669-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="19669-1215">Contient des données binaires.</span><span class="sxs-lookup"><span data-stu-id="19669-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="19669-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="19669-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="19669-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="19669-1218">Contient la valeur **true** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="19669-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="19669-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="19669-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="19669-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="19669-1221">Contient une valeur d'entier 8 bits non signé.</span><span class="sxs-lookup"><span data-stu-id="19669-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="19669-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="19669-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="19669-1224">Représente une date et une heure.</span><span class="sxs-lookup"><span data-stu-id="19669-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="19669-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="19669-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="19669-1227">Contient une date et une heure en tant que décalage en minutes par rapport à l'heure GMT.</span><span class="sxs-lookup"><span data-stu-id="19669-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="19669-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="19669-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="19669-1230">Contient une valeur numérique avec une précision et une échelle fixes.</span><span class="sxs-lookup"><span data-stu-id="19669-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="19669-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="19669-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="19669-1233">Contient un nombre avec une précision de 15 chiffres à virgule flottante</span><span class="sxs-lookup"><span data-stu-id="19669-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="19669-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="19669-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="19669-1236">Contient un nombre à virgule flottante avec une précision de 7 chiffres.</span><span class="sxs-lookup"><span data-stu-id="19669-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="19669-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="19669-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="19669-1239">Contient un identificateur unique de 16 octets.</span><span class="sxs-lookup"><span data-stu-id="19669-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="19669-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="19669-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="19669-1242">Contient une valeur d'entier 16 bits signé.</span><span class="sxs-lookup"><span data-stu-id="19669-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="19669-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="19669-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="19669-1245">Contient une valeur d'entier 32 bits signé.</span><span class="sxs-lookup"><span data-stu-id="19669-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="19669-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="19669-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="19669-1248">Contient une valeur d'entier 64 bits signé.</span><span class="sxs-lookup"><span data-stu-id="19669-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="19669-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="19669-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="19669-1251">Contient une valeur d'entier 8 bits signé.</span><span class="sxs-lookup"><span data-stu-id="19669-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="19669-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="19669-1253">**Edm.String**</span></span>                   | <span data-ttu-id="19669-1254">Contient des données caractères.</span><span class="sxs-lookup"><span data-stu-id="19669-1254">Contains character data.</span></span>                                   | <span data-ttu-id="19669-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="19669-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="19669-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="19669-1257">Contient une heure.</span><span class="sxs-lookup"><span data-stu-id="19669-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="19669-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="19669-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="19669-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="19669-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="19669-1260">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="19669-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="19669-1262">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="19669-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="19669-1264">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="19669-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="19669-1266">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="19669-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="19669-1268">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="19669-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="19669-1270">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="19669-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="19669-1272">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="19669-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="19669-1274">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="19669-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="19669-1276">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="19669-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="19669-1278">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="19669-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="19669-1280">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="19669-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="19669-1282">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="19669-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="19669-1284">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="19669-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="19669-1286">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="19669-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="19669-1288">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="19669-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="19669-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="19669-1290">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="19669-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="19669-1291">Facettes (CSDL)</span><span class="sxs-lookup"><span data-stu-id="19669-1291">Facets (CSDL)</span></span>

<span data-ttu-id="19669-1292">Les facettes dans le langage CSDL (Conceptual Schema Definition Language) représentent des contraintes sur les propriétés de types d'entités et de types complexes.</span><span class="sxs-lookup"><span data-stu-id="19669-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="19669-1293">Les facettes apparaissent comme des attributs XML sur les éléments CSDL suivants :</span><span class="sxs-lookup"><span data-stu-id="19669-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="19669-1294">Propriété</span><span class="sxs-lookup"><span data-stu-id="19669-1294">Property</span></span>
-   <span data-ttu-id="19669-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="19669-1295">TypeRef</span></span>
-   <span data-ttu-id="19669-1296">Paramètre</span><span class="sxs-lookup"><span data-stu-id="19669-1296">Parameter</span></span>

<span data-ttu-id="19669-1297">Le tableau ci-dessous décrit les facettes prises en charge dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="19669-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="19669-1298">Toutes les facettes sont facultatives.</span><span class="sxs-lookup"><span data-stu-id="19669-1298">All facets are optional.</span></span> <span data-ttu-id="19669-1299">Certaines facettes répertoriées ci-dessous sont utilisées par Entity Framework lors de la génération d’une base de données à partir d’un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="19669-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="19669-1300">Pour plus d’informations sur les types de données dans un modèle conceptuel, consultez Types de modèle conceptuel (CSDL).</span><span class="sxs-lookup"><span data-stu-id="19669-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="19669-1301">Facette</span><span class="sxs-lookup"><span data-stu-id="19669-1301">Facet</span></span>               | <span data-ttu-id="19669-1302">Description</span><span class="sxs-lookup"><span data-stu-id="19669-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="19669-1303">S'applique à</span><span class="sxs-lookup"><span data-stu-id="19669-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="19669-1304">Utilisée pour la génération de base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-1304">Used for the database generation</span></span> | <span data-ttu-id="19669-1305">Utilisée par le runtime.</span><span class="sxs-lookup"><span data-stu-id="19669-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="19669-1306">**classement**</span><span class="sxs-lookup"><span data-stu-id="19669-1306">**Collation**</span></span>       | <span data-ttu-id="19669-1307">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="19669-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="19669-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="19669-1309">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1309">Yes</span></span>                              | <span data-ttu-id="19669-1310">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1310">No</span></span>                  |
| <span data-ttu-id="19669-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="19669-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="19669-1312">Indique que la valeur de la propriété doit être utilisée pour des contrôles d'accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="19669-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="19669-1313">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="19669-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="19669-1314">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1314">No</span></span>                               | <span data-ttu-id="19669-1315">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1315">Yes</span></span>                 |
| <span data-ttu-id="19669-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="19669-1316">**Default**</span></span>         | <span data-ttu-id="19669-1317">Spécifie la valeur par défaut de la propriété si aucune valeur n'est fournie en cas d'instanciation.</span><span class="sxs-lookup"><span data-stu-id="19669-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="19669-1318">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="19669-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="19669-1319">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1319">Yes</span></span>                              | <span data-ttu-id="19669-1320">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1320">Yes</span></span>                 |
| <span data-ttu-id="19669-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="19669-1321">**FixedLength**</span></span>     | <span data-ttu-id="19669-1322">Spécifie si la longueur de la valeur de propriété peut varier.</span><span class="sxs-lookup"><span data-stu-id="19669-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="19669-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="19669-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="19669-1324">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1324">Yes</span></span>                              | <span data-ttu-id="19669-1325">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1325">No</span></span>                  |
| <span data-ttu-id="19669-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="19669-1326">**MaxLength**</span></span>       | <span data-ttu-id="19669-1327">Spécifie la longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="19669-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="19669-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="19669-1329">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1329">Yes</span></span>                              | <span data-ttu-id="19669-1330">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1330">No</span></span>                  |
| <span data-ttu-id="19669-1331">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="19669-1331">**Nullable**</span></span>        | <span data-ttu-id="19669-1332">Spécifie si la propriété peut avoir un **null** valeur.</span><span class="sxs-lookup"><span data-stu-id="19669-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="19669-1333">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="19669-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="19669-1334">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1334">Yes</span></span>                              | <span data-ttu-id="19669-1335">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1335">Yes</span></span>                 |
| <span data-ttu-id="19669-1336">**Précision**</span><span class="sxs-lookup"><span data-stu-id="19669-1336">**Precision**</span></span>       | <span data-ttu-id="19669-1337">Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="19669-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="19669-1338">Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="19669-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="19669-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="19669-1340">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1340">Yes</span></span>                              | <span data-ttu-id="19669-1341">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1341">No</span></span>                  |
| <span data-ttu-id="19669-1342">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="19669-1342">**Scale**</span></span>           | <span data-ttu-id="19669-1343">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="19669-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="19669-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="19669-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="19669-1345">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1345">Yes</span></span>                              | <span data-ttu-id="19669-1346">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1346">No</span></span>                  |
| <span data-ttu-id="19669-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="19669-1347">**SRID**</span></span>            | <span data-ttu-id="19669-1348">Spécifie l’ID système Spatial référence système.</span><span class="sxs-lookup"><span data-stu-id="19669-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="19669-1349">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="19669-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="19669-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="19669-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="19669-1351">Non</span><span class="sxs-lookup"><span data-stu-id="19669-1351">No</span></span>                               | <span data-ttu-id="19669-1352">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1352">Yes</span></span>                 |
| <span data-ttu-id="19669-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="19669-1353">**Unicode**</span></span>         | <span data-ttu-id="19669-1354">Indique si la valeur de propriété est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="19669-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="19669-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="19669-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="19669-1356">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1356">Yes</span></span>                              | <span data-ttu-id="19669-1357">Oui</span><span class="sxs-lookup"><span data-stu-id="19669-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="19669-1358">Lors de la génération d’une base de données à partir d’un modèle conceptuel, l’Assistant génération de base de données reconnaîtra la valeur de la **StoreGeneratedPattern** attribut sur un **propriété** élément si elle est la suivante espace de noms : http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="19669-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="19669-1359">Les valeurs prises en charge pour l’attribut sont **identité** et **calculé**.</span><span class="sxs-lookup"><span data-stu-id="19669-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="19669-1360">La valeur **identité** produira une colonne de base de données avec une valeur d’identité générée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="19669-1361">La valeur **calculé** produira une colonne avec une valeur qui est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="19669-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="19669-1362">Exemple</span><span class="sxs-lookup"><span data-stu-id="19669-1362">Example</span></span>

<span data-ttu-id="19669-1363">L'exemple suivant illustre l'application de facettes aux propriétés d'un type d'entité :</span><span class="sxs-lookup"><span data-stu-id="19669-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
