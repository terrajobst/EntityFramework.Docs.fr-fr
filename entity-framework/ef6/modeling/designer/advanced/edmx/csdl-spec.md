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
# <a name="csdl-specification"></a><span data-ttu-id="72309-102">Spécification CSDL</span><span class="sxs-lookup"><span data-stu-id="72309-102">CSDL Specification</span></span>
<span data-ttu-id="72309-103">Le langage CSDL (Conceptual Schema Definition Language) est un langage basé sur XML qui décrit les entités, relations et fonctions qui composent un modèle conceptuel d'une application pilotée par les données.</span><span class="sxs-lookup"><span data-stu-id="72309-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="72309-104">Ce modèle conceptuel peut être utilisé par Entity Framework ou WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="72309-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="72309-105">Les métadonnées qui sont décrites avec le langage CSDL sont utilisée par Entity Framework pour mapper les entités et relations qui sont définies dans un modèle conceptuel à une source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="72309-106">Pour plus d’informations, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) et [spécification MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="72309-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="72309-107">CSDL est l’implémentation d’Entity Framework de l’Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="72309-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="72309-108">Dans une application Entity Framework, les métadonnées du modèle conceptuel est chargée à partir d’un fichier .csdl (écrit en langage CSDL) dans une instance de la System.Data.Metadata.Edm.EdmItemCollection et est accessible à l’aide de méthodes dans le Classe de System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="72309-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="72309-109">Entity Framework utilise des métadonnées de modèle conceptuel pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques à la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="72309-110">Le Concepteur EF stocke les informations de modèle conceptuel dans un fichier .edmx au moment du design.</span><span class="sxs-lookup"><span data-stu-id="72309-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="72309-111">Au moment de la génération, le Concepteur EF utilise les informations dans un fichier .edmx pour créer le fichier .csdl qui est nécessaire par Entity Framework lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="72309-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="72309-112">Les versions de CSDL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="72309-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="72309-113">Version du langage CSDL</span><span class="sxs-lookup"><span data-stu-id="72309-113">CSDL Version</span></span> | <span data-ttu-id="72309-114">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="72309-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="72309-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="72309-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="72309-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="72309-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="72309-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="72309-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="72309-118">Association, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-118">Association Element (CSDL)</span></span>

<span data-ttu-id="72309-119">Un **Association** élément définit une relation entre deux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="72309-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="72309-120">Une association doit spécifier les types d'entités impliqués dans la relation et le nombre possible de types d'entités à chaque terminaison de la relation, appelé « multiplicité ».</span><span class="sxs-lookup"><span data-stu-id="72309-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="72309-121">La multiplicité d’une terminaison d’association peut avoir une valeur d’un (1), zéro ou un (0.. 1) ou plusieurs (\*).</span><span class="sxs-lookup"><span data-stu-id="72309-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="72309-122">Ces informations sont spécifiées dans les deux éléments de fin d’enfants.</span><span class="sxs-lookup"><span data-stu-id="72309-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="72309-123">Il est possible d'accéder aux instances de type d'entité au niveau d'une terminaison d'une association via les propriétés de navigation ou les clés étrangères, si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="72309-124">Dans une application, une instance d'une association représente une association spécifique entre des instances de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="72309-125">Les instances d'association sont regroupées logiquement dans un ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="72309-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="72309-126">Un **Association** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-127">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-128">Fin (exactement 2 éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="72309-129">ReferentialConstraint (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="72309-130">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-131">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-131">Applicable Attributes</span></span>

<span data-ttu-id="72309-132">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="72309-133">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-133">Attribute Name</span></span> | <span data-ttu-id="72309-134">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-134">Is Required</span></span> | <span data-ttu-id="72309-135">Value</span><span class="sxs-lookup"><span data-stu-id="72309-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="72309-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-136">**Name**</span></span>       | <span data-ttu-id="72309-137">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-137">Yes</span></span>         | <span data-ttu-id="72309-138">Nom de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-139">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="72309-140">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-141">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-142">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-142">Example</span></span>

<span data-ttu-id="72309-143">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association de clés étrangères n’ont pas été exposées sur le **client** et  **Commande** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="72309-144">Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**.</span><span class="sxs-lookup"><span data-stu-id="72309-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="72309-145">En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext est supprimé. Si le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="72309-146">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association lorsque les clés étrangères ont été exposées sur le **client** et  **Commande** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="72309-147">Avec des clés étrangères exposées, la relation entre les entités est gérée avec un **ReferentialConstraint** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="72309-148">Un élément AssociationSetMapping correspondant n’est pas nécessaire pour mapper cette association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="72309-149">AssociationSet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="72309-150">Le **AssociationSet** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les instances d’association du même type.</span><span class="sxs-lookup"><span data-stu-id="72309-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="72309-151">Un ensemble d'associations fournit une définition pour le regroupement d'instances d'association afin qu'elles puissent être mappées à une source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="72309-152">Le **AssociationSet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-153">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-154">Fin (exactement deux éléments requis)</span><span class="sxs-lookup"><span data-stu-id="72309-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="72309-155">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="72309-156">Le **Association** attribut spécifie le type d’association qui contient un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="72309-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="72309-157">Les jeux d’entités qui composent les terminaisons d’un ensemble d’associations sont spécifiés avec exactement deux enfants **fin** éléments.</span><span class="sxs-lookup"><span data-stu-id="72309-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-158">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-158">Applicable Attributes</span></span>

<span data-ttu-id="72309-159">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="72309-160">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-160">Attribute Name</span></span>  | <span data-ttu-id="72309-161">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-161">Is Required</span></span> | <span data-ttu-id="72309-162">Value</span><span class="sxs-lookup"><span data-stu-id="72309-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-163">**Name**</span></span>        | <span data-ttu-id="72309-164">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-164">Yes</span></span>         | <span data-ttu-id="72309-165">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-165">The name of the entity set.</span></span> <span data-ttu-id="72309-166">La valeur de la **nom** attribut ne peut pas être identique à la valeur de la **Association** attribut.</span><span class="sxs-lookup"><span data-stu-id="72309-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="72309-167">**Association**</span><span class="sxs-lookup"><span data-stu-id="72309-167">**Association**</span></span> | <span data-ttu-id="72309-168">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-168">Yes</span></span>         | <span data-ttu-id="72309-169">Nom qualifié complet de l'association dont l'ensemble d'associations contient des instances.</span><span class="sxs-lookup"><span data-stu-id="72309-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="72309-170">L'association doit être dans le même espace de noms que l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="72309-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-171">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="72309-172">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-173">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-174">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-174">Example</span></span>

<span data-ttu-id="72309-175">L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="72309-176">Élément CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="72309-177">Le **CollectionType** élément de langage de définition de schéma conceptuel (CSDL) Spécifie qu’une fonction ou un paramètre de fonction de type de retour est une collection.</span><span class="sxs-lookup"><span data-stu-id="72309-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="72309-178">Le **CollectionType** élément peut être un enfant de l’élément de paramètre ou de l’élément ReturnType (fonction).</span><span class="sxs-lookup"><span data-stu-id="72309-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="72309-179">Le type de collection peut être spécifié en utilisant le **Type** attribut ou l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="72309-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="72309-180">**CollectionType**</span></span>
-   <span data-ttu-id="72309-181">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="72309-181">ReferenceType</span></span>
-   <span data-ttu-id="72309-182">RowType ;</span><span class="sxs-lookup"><span data-stu-id="72309-182">RowType</span></span>
-   <span data-ttu-id="72309-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="72309-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="72309-184">Un modèle ne sera pas validé si le type d’une collection est spécifié avec à la fois le **Type** attribut et un élément enfant.</span><span class="sxs-lookup"><span data-stu-id="72309-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="72309-185">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-185">Applicable Attributes</span></span>

<span data-ttu-id="72309-186">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="72309-187">Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables aux collections de **types Edmsimpletype**.</span><span class="sxs-lookup"><span data-stu-id="72309-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="72309-188">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-188">Attribute Name</span></span>                                                          | <span data-ttu-id="72309-189">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-189">Is Required</span></span> | <span data-ttu-id="72309-190">Value</span><span class="sxs-lookup"><span data-stu-id="72309-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-191">**Type**</span></span>                                                                | <span data-ttu-id="72309-192">Non</span><span class="sxs-lookup"><span data-stu-id="72309-192">No</span></span>          | <span data-ttu-id="72309-193">Type de la collection.</span><span class="sxs-lookup"><span data-stu-id="72309-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="72309-194">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-194">**Nullable**</span></span>                                                            | <span data-ttu-id="72309-195">Non</span><span class="sxs-lookup"><span data-stu-id="72309-195">No</span></span>          | <span data-ttu-id="72309-196">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="72309-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="72309-197">> Dans le langage CSDL v1, une propriété de type complexe doit avoir `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="72309-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="72309-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="72309-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="72309-199">Non</span><span class="sxs-lookup"><span data-stu-id="72309-199">No</span></span>          | <span data-ttu-id="72309-200">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="72309-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="72309-202">Non</span><span class="sxs-lookup"><span data-stu-id="72309-202">No</span></span>          | <span data-ttu-id="72309-203">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="72309-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="72309-205">Non</span><span class="sxs-lookup"><span data-stu-id="72309-205">No</span></span>          | <span data-ttu-id="72309-206">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="72309-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="72309-207">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-207">**Precision**</span></span>                                                           | <span data-ttu-id="72309-208">Non</span><span class="sxs-lookup"><span data-stu-id="72309-208">No</span></span>          | <span data-ttu-id="72309-209">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="72309-210">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-210">**Scale**</span></span>                                                               | <span data-ttu-id="72309-211">Non</span><span class="sxs-lookup"><span data-stu-id="72309-211">No</span></span>          | <span data-ttu-id="72309-212">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="72309-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-213">**SRID**</span></span>                                                                | <span data-ttu-id="72309-214">Non</span><span class="sxs-lookup"><span data-stu-id="72309-214">No</span></span>          | <span data-ttu-id="72309-215">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-216">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="72309-217">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="72309-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="72309-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-218">**Unicode**</span></span>                                                             | <span data-ttu-id="72309-219">Non</span><span class="sxs-lookup"><span data-stu-id="72309-219">No</span></span>          | <span data-ttu-id="72309-220">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="72309-221">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-221">**Collation**</span></span>                                                           | <span data-ttu-id="72309-222">Non</span><span class="sxs-lookup"><span data-stu-id="72309-222">No</span></span>          | <span data-ttu-id="72309-223">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="72309-224">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="72309-225">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-226">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-227">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-227">Example</span></span>

<span data-ttu-id="72309-228">L’exemple suivant illustre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de **personne** types d’entités (comme spécifié par le **ElementType** attribut).</span><span class="sxs-lookup"><span data-stu-id="72309-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="72309-229">L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="72309-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="72309-230">L’exemple suivant montre une fonction définie par modèle qui utilise le **CollectionType** élément pour spécifier que la fonction accepte en tant que paramètre à une collection de **département** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="72309-231">ComplexType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="72309-232">Un **ComplexType** élément définit une structure de données composée de **EdmSimpleType** propriétés ou autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="72309-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="72309-233">Un type complexe peut être une propriété d'un type d'entité ou d'un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="72309-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="72309-234">Un type complexe est semblable à un type d'entité du fait qu'un type complexe définit des données.</span><span class="sxs-lookup"><span data-stu-id="72309-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="72309-235">Toutefois, il existe des différences clés entre les types complexes et les types d'entités :</span><span class="sxs-lookup"><span data-stu-id="72309-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="72309-236">Les types complexes n'ont pas d'identité (ni de clés), par conséquent, ils ne peuvent pas exister indépendamment.</span><span class="sxs-lookup"><span data-stu-id="72309-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="72309-237">Les types complexes peuvent uniquement exister en tant que propriétés de types d'entités ou d'autres types complexes.</span><span class="sxs-lookup"><span data-stu-id="72309-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="72309-238">Les types complexes ne peuvent pas participer aux associations.</span><span class="sxs-lookup"><span data-stu-id="72309-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="72309-239">Ni la fin d’une association peut être un type complexe, et par conséquent les propriétés de navigation ne peut pas être définies pour les types complexes.</span><span class="sxs-lookup"><span data-stu-id="72309-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="72309-240">Une propriété de type complexe ne peut pas avoir de valeur NULL, bien que les propriétés scalaires d'un type complexe puissent chacune être définie sur NULL.</span><span class="sxs-lookup"><span data-stu-id="72309-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="72309-241">Un **ComplexType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-242">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-243">Propriété (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="72309-244">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="72309-245">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ComplexType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="72309-246">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="72309-247">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-247">Is Required</span></span> | <span data-ttu-id="72309-248">Value</span><span class="sxs-lookup"><span data-stu-id="72309-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-249">Name</span><span class="sxs-lookup"><span data-stu-id="72309-249">Name</span></span>                                                                                                           | <span data-ttu-id="72309-250">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-250">Yes</span></span>         | <span data-ttu-id="72309-251">Nom du type complexe.</span><span class="sxs-lookup"><span data-stu-id="72309-251">The name of the complex type.</span></span> <span data-ttu-id="72309-252">Le nom d'un type complexe ne peut pas être identique au nom d'un autre type complexe, d'un type d'entité ou d'une association qui figure dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="72309-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="72309-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="72309-254">Non</span><span class="sxs-lookup"><span data-stu-id="72309-254">No</span></span>          | <span data-ttu-id="72309-255">Nom d'un autre type complexe qui est le type de base du type complexe en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="72309-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="72309-256">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="72309-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="72309-257">L'héritage pour les types complexes n'est pas pris en charge dans cette version.</span><span class="sxs-lookup"><span data-stu-id="72309-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="72309-258">Abstract</span><span class="sxs-lookup"><span data-stu-id="72309-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="72309-259">Non</span><span class="sxs-lookup"><span data-stu-id="72309-259">No</span></span>          | <span data-ttu-id="72309-260">**True** ou **False** (la valeur par défaut) selon que le type complexe est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="72309-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="72309-261">> Cet attribut n’est pas applicable dans CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="72309-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="72309-262">Les types complexes dans cette version ne peuvent pas être des types abstraits.</span><span class="sxs-lookup"><span data-stu-id="72309-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="72309-263">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ComplexType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="72309-264">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-265">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-266">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-266">Example</span></span>

<span data-ttu-id="72309-267">L’exemple suivant illustre un type complexe, **adresse**, avec le **EdmSimpleType** propriétés **StreetAddress**, **Ville**,  **StateOrProvince**, **pays**, et **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="72309-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="72309-268">Pour définir le type complexe **adresse** (ci-dessus) en tant que propriété d’un type d’entité, vous devez déclarer le type de propriété dans la définition de type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="72309-269">L’exemple suivant montre le **adresse** propriété comme un type complexe sur un type d’entité (**Publisher**) :</span><span class="sxs-lookup"><span data-stu-id="72309-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="72309-270">DefiningExpression, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="72309-271">Le **DefiningExpression** élément de langage de définition de schéma conceptuel (CSDL) contient une expression Entity SQL qui définit une fonction dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="72309-272">À des fins de validation, un **DefiningExpression** élément peut contenir du contenu arbitraire.</span><span class="sxs-lookup"><span data-stu-id="72309-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="72309-273">Toutefois, Entity Framework lève une exception lors de l’exécution si un **DefiningExpression** élément ne contient pas de valide Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="72309-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="72309-274">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-274">Applicable Attributes</span></span>

<span data-ttu-id="72309-275">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **DefiningExpression** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="72309-276">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-277">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="72309-278">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-278">Example</span></span>

<span data-ttu-id="72309-279">L’exemple suivant utilise un **DefiningExpression** élément pour définir une fonction qui retourne le nombre d’années depuis un ouvrage a été publié.</span><span class="sxs-lookup"><span data-stu-id="72309-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="72309-280">Le contenu de la **DefiningExpression** élément est écrit dans Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="72309-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="72309-281">Dependent, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="72309-282">Le **dépendants** élément dans le langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint et définit la terminaison dépendante d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="72309-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="72309-283">Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="72309-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="72309-284">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="72309-285">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="72309-286">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="72309-287">**PropertyRef** éléments sont utilisés pour spécifier les clés qui référencent la terminaison principale.</span><span class="sxs-lookup"><span data-stu-id="72309-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="72309-288">Le **dépendants** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-289">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="72309-290">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-291">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-291">Applicable Attributes</span></span>

<span data-ttu-id="72309-292">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="72309-293">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-293">Attribute Name</span></span> | <span data-ttu-id="72309-294">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-294">Is Required</span></span> | <span data-ttu-id="72309-295">Value</span><span class="sxs-lookup"><span data-stu-id="72309-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="72309-296">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="72309-296">**Role**</span></span>       | <span data-ttu-id="72309-297">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-297">Yes</span></span>         | <span data-ttu-id="72309-298">Nom du type d'entité au niveau de la terminaison dépendante de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-299">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **dépendants** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="72309-300">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-301">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-302">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-302">Example</span></span>

<span data-ttu-id="72309-303">L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="72309-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="72309-304">Le **PublisherId** propriété de la **livre** type d’entité compose la terminaison dépendante de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="72309-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="72309-305">Documentation, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="72309-306">Le **Documentation** élément dans le langage de définition de schéma conceptuel (CSDL) peut être utilisé pour fournir des informations relatives à un objet qui est défini dans un élément parent.</span><span class="sxs-lookup"><span data-stu-id="72309-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="72309-307">Dans un fichier .edmx, lorsque le **Documentation** élément est un enfant d’un élément qui apparaît sous la forme d’un objet sur l’aire de conception du Concepteur EF (par exemple, une entité, associations ou une propriété), le contenu de la **Documentation**  élément apparaîtra dans Visual Studio **propriétés** fenêtre de l’objet.</span><span class="sxs-lookup"><span data-stu-id="72309-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="72309-308">Le **Documentation** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-309">**Résumé**: une brève description de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="72309-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="72309-310">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="72309-310">(zero or one element)</span></span>
-   <span data-ttu-id="72309-311">**LongDescription**: une description détaillée de l’élément parent.</span><span class="sxs-lookup"><span data-stu-id="72309-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="72309-312">(zéro ou un élément).</span><span class="sxs-lookup"><span data-stu-id="72309-312">(zero or one element)</span></span>
-   <span data-ttu-id="72309-313">Éléments d’annotation.</span><span class="sxs-lookup"><span data-stu-id="72309-313">Annotation elements.</span></span> <span data-ttu-id="72309-314">(zéro, un ou plusieurs éléments).</span><span class="sxs-lookup"><span data-stu-id="72309-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-315">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-315">Applicable Attributes</span></span>

<span data-ttu-id="72309-316">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Documentation** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="72309-317">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-318">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="72309-319">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-319">Example</span></span>

<span data-ttu-id="72309-320">L’exemple suivant montre le **Documentation** en tant qu’un élément enfant d’un élément EntityType.</span><span class="sxs-lookup"><span data-stu-id="72309-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="72309-321">Si l’extrait de code ci-dessous ont été dans le langage CSDL contenu d’un .edmx de fichiers, le contenu de la **Résumé** et **LongDescription** éléments apparaîtrait dans Visual Studio **propriétés** fenêtre lorsque vous cliquez sur le `Customer` type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="72309-322">End, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-322">End Element (CSDL)</span></span>

<span data-ttu-id="72309-323">Le **fin** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément Association ou de l’élément AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="72309-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="72309-324">Dans chaque cas, le rôle de la **fin** élément est différent et les attributs applicables sont différents.</span><span class="sxs-lookup"><span data-stu-id="72309-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="72309-325">Élément End comme enfant de l'élément Association</span><span class="sxs-lookup"><span data-stu-id="72309-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="72309-326">Un **fin** élément (en tant qu’enfant de le **Association** élément) identifie le type d’entité sur une extrémité d’une association et le nombre d’instances de type d’entité qui peuvent exister à cette fin d’une association.</span><span class="sxs-lookup"><span data-stu-id="72309-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="72309-327">Les terminaisons d'association sont définies dans le cadre d'une association ; une association doit avoir exactement deux terminaisons d'association.</span><span class="sxs-lookup"><span data-stu-id="72309-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="72309-328">Il est possible d'accéder aux instances de type d'entité au niveau de la terminaison d'une association via les propriétés de navigation ou les clés étrangères si elles sont exposées sur un type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="72309-329">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-330">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-331">OnDelete (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="72309-332">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-333">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-333">Applicable Attributes</span></span>

<span data-ttu-id="72309-334">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="72309-335">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-335">Attribute Name</span></span>   | <span data-ttu-id="72309-336">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-336">Is Required</span></span> | <span data-ttu-id="72309-337">Value</span><span class="sxs-lookup"><span data-stu-id="72309-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-338">**Type**</span></span>         | <span data-ttu-id="72309-339">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-339">Yes</span></span>         | <span data-ttu-id="72309-340">Nom du type d'entité au niveau de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="72309-341">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="72309-341">**Role**</span></span>         | <span data-ttu-id="72309-342">Non</span><span class="sxs-lookup"><span data-stu-id="72309-342">No</span></span>          | <span data-ttu-id="72309-343">Nom de la terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-343">A name for the association end.</span></span> <span data-ttu-id="72309-344">Si aucun nom n'est fourni, le nom du type d'entité au niveau de la terminaison de l'association sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="72309-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="72309-345">**Multiplicité**</span><span class="sxs-lookup"><span data-stu-id="72309-345">**Multiplicity**</span></span> | <span data-ttu-id="72309-346">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-346">Yes</span></span>         | <span data-ttu-id="72309-347">**1**, **valeur 0.. 1**, ou **\*** selon le nombre d’instances de type d’entité qui peut être à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="72309-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="72309-348">**1** indique cette instance de type exactement une entité existe à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="72309-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="72309-349">**valeur 0.. 1** indique que zéro ou une instance de type d’entité existe au niveau de la terminaison d’association.</span><span class="sxs-lookup"><span data-stu-id="72309-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="72309-350">**\*** Indique que zéro, une ou plusieurs instances de type d’entité existent à la fin de l’association.</span><span class="sxs-lookup"><span data-stu-id="72309-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-351">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="72309-352">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-353">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-354">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-354">Example</span></span>

<span data-ttu-id="72309-355">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="72309-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="72309-356">Le **multiplicité** valeurs pour chaque **fin** de l’association indiquent que de nombreux **commandes** peut être associé un **client**, mais seul **client** peut être associé un **ordre**.</span><span class="sxs-lookup"><span data-stu-id="72309-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="72309-357">En outre, le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et qui ont été chargés dans ObjectContext sera if supprimé le **client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="72309-358">Élément End comme enfant de l'élément AssociationSet</span><span class="sxs-lookup"><span data-stu-id="72309-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="72309-359">Le **fin** élément spécifie une extrémité d’un ensemble d’associations.</span><span class="sxs-lookup"><span data-stu-id="72309-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="72309-360">Le **AssociationSet** élément doit contenir deux **fin** éléments.</span><span class="sxs-lookup"><span data-stu-id="72309-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="72309-361">Les informations contenues dans un **fin** élément est utilisé pour mapper une ensemble d’associations à une source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="72309-362">Un **fin** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-363">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-364">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="72309-365">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="72309-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="72309-366">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="72309-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-367">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-367">Applicable Attributes</span></span>

<span data-ttu-id="72309-368">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **fin** élément lorsqu’il est l’enfant d’un **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="72309-369">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-369">Attribute Name</span></span> | <span data-ttu-id="72309-370">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-370">Is Required</span></span> | <span data-ttu-id="72309-371">Value</span><span class="sxs-lookup"><span data-stu-id="72309-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="72309-372">**EntitySet**</span></span>  | <span data-ttu-id="72309-373">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-373">Yes</span></span>         | <span data-ttu-id="72309-374">Le nom de la **EntitySet** élément qui définit une extrémité du parent **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="72309-375">Le **EntitySet** élément doit être défini dans le même conteneur d’entités en tant que parent **AssociationSet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="72309-376">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="72309-376">**Role**</span></span>       | <span data-ttu-id="72309-377">Non</span><span class="sxs-lookup"><span data-stu-id="72309-377">No</span></span>          | <span data-ttu-id="72309-378">Nom de la terminaison de l'ensemble d'associations.</span><span class="sxs-lookup"><span data-stu-id="72309-378">The name of the association set end.</span></span> <span data-ttu-id="72309-379">Si le **rôle** attribut n’est pas utilisé, le nom de l’ensemble d’associations sera le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="72309-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="72309-380">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fin** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="72309-381">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-382">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-383">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-383">Example</span></span>

<span data-ttu-id="72309-384">L’exemple suivant montre un **EntityContainer** élément avec deux **AssociationSet** éléments, chacun avec deux **fin** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="72309-385">EntityContainer, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="72309-386">Le **EntityContainer** élément dans le langage de définition de schéma conceptuel (CSDL) est un conteneur logique pour les jeux d’entités, les ensembles d’associations et les importations de fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="72309-387">Un conteneur d’entités de modèle conceptuel est mappé à un conteneur d’entités du modèle via l’élément EntityContainerMapping stockage.</span><span class="sxs-lookup"><span data-stu-id="72309-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="72309-388">Un conteneur d'entités de modèle de stockage décrit la structure de la base de données : les jeux d'entités décrivent les tables, les ensembles d'associations décrivent les contraintes de clé étrangère et les importations de fonction décrivent les procédures stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="72309-389">Un **EntityContainer** élément peut avoir zéro ou un élément de la Documentation.</span><span class="sxs-lookup"><span data-stu-id="72309-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="72309-390">Si un **Documentation** élément est présent, il doit précéder tous les **EntitySet**, **AssociationSet**, et **FunctionImport** éléments.</span><span class="sxs-lookup"><span data-stu-id="72309-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="72309-391">Un **EntityContainer** élément peut avoir zéro ou plusieurs des éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-392">EntitySet ;</span><span class="sxs-lookup"><span data-stu-id="72309-392">EntitySet</span></span>
-   <span data-ttu-id="72309-393">AssociationSet ;</span><span class="sxs-lookup"><span data-stu-id="72309-393">AssociationSet</span></span>
-   <span data-ttu-id="72309-394">FunctionImport ;</span><span class="sxs-lookup"><span data-stu-id="72309-394">FunctionImport</span></span>
-   <span data-ttu-id="72309-395">éléments d'annotation.</span><span class="sxs-lookup"><span data-stu-id="72309-395">Annotation elements</span></span>

<span data-ttu-id="72309-396">Vous pouvez étendre un **EntityContainer** élément pour inclure le contenu d’un autre **EntityContainer** qui figure dans le même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="72309-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="72309-397">Pour inclure le contenu d’un autre **EntityContainer**, dans le référencement **EntityContainer** élément, définissez la valeur de la **étend** nom à l’attribut de la  **EntityContainer** élément que vous souhaitez inclure.</span><span class="sxs-lookup"><span data-stu-id="72309-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="72309-398">Tous les éléments enfants de l’élément inclus **EntityContainer** élément est traité comme éléments enfants de la référence **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-399">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-399">Applicable Attributes</span></span>

<span data-ttu-id="72309-400">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="72309-401">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-401">Attribute Name</span></span> | <span data-ttu-id="72309-402">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-402">Is Required</span></span> | <span data-ttu-id="72309-403">Value</span><span class="sxs-lookup"><span data-stu-id="72309-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="72309-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-404">**Name**</span></span>       | <span data-ttu-id="72309-405">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-405">Yes</span></span>         | <span data-ttu-id="72309-406">Nom du conteneur d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="72309-407">**Étend**</span><span class="sxs-lookup"><span data-stu-id="72309-407">**Extends**</span></span>    | <span data-ttu-id="72309-408">Non</span><span class="sxs-lookup"><span data-stu-id="72309-408">No</span></span>          | <span data-ttu-id="72309-409">Le nom d'un autre conteneur d'entités au sein du même espace de noms.</span><span class="sxs-lookup"><span data-stu-id="72309-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-410">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityContainer** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="72309-411">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-412">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-413">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-413">Example</span></span>

<span data-ttu-id="72309-414">L’exemple suivant montre un **EntityContainer** élément qui définit trois jeux d’entités et les deux ensembles d’associations.</span><span class="sxs-lookup"><span data-stu-id="72309-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="72309-415">EntitySet, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="72309-416">Le **EntitySet** élément dans le langage de définition de schéma conceptuel est un conteneur logique pour les instances d’un type d’entité et les instances de n’importe quel type dérivé de ce type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="72309-417">La relation entre un type d'entité et un jeu d'entités est analogue à la relation entre une ligne et une table dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="72309-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="72309-418">Comme une ligne, un type d'entité définit un jeu de données connexes et, comme une table, un jeu d'entités contient des instances de cette définition.</span><span class="sxs-lookup"><span data-stu-id="72309-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="72309-419">Un jeu d'entités fournit une construction pour le regroupement d'instances du type d'entité, afin qu'elles puissent être mappées aux structures de données associées dans une source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="72309-420">Plusieurs jeux d'entités peuvent être définis pour un type d'entité particulier.</span><span class="sxs-lookup"><span data-stu-id="72309-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-421">Le Concepteur EF ne prend pas en charge les modèles conceptuels qui contiennent des jeux d’entités multiples par type.</span><span class="sxs-lookup"><span data-stu-id="72309-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="72309-422">Le **EntitySet** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-423">Documentation, élément (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-424">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-425">Applicable Attributes</span></span>

<span data-ttu-id="72309-426">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="72309-427">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-427">Attribute Name</span></span> | <span data-ttu-id="72309-428">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-428">Is Required</span></span> | <span data-ttu-id="72309-429">Value</span><span class="sxs-lookup"><span data-stu-id="72309-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-430">**Name**</span></span>       | <span data-ttu-id="72309-431">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-431">Yes</span></span>         | <span data-ttu-id="72309-432">Nom du jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="72309-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="72309-433">**EntityType**</span></span> | <span data-ttu-id="72309-434">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-434">Yes</span></span>         | <span data-ttu-id="72309-435">Nom qualifié complet du type d'entité pour lequel le jeu d'entités contient des instances.</span><span class="sxs-lookup"><span data-stu-id="72309-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-436">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntitySet** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="72309-437">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-438">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-439">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-439">Example</span></span>

<span data-ttu-id="72309-440">L’exemple suivant montre un **EntityContainer** élément avec trois **EntitySet** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="72309-441">Il est possible de définir des jeux d'entités multiples par type (MEST).</span><span class="sxs-lookup"><span data-stu-id="72309-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="72309-442">L’exemple suivant définit un conteneur d’entités avec deux jeux d’entités pour le **livre** type d’entité :</span><span class="sxs-lookup"><span data-stu-id="72309-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="72309-443">EntityType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="72309-444">Le **EntityType** élément représente la structure d’un concept de niveau supérieur, tel qu’un client ou l’ordre, dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="72309-445">Un type d'entité est un modèle pour des instances de types d'entités dans une application.</span><span class="sxs-lookup"><span data-stu-id="72309-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="72309-446">Chaque modèle contient les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="72309-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="72309-447">Nom unique.</span><span class="sxs-lookup"><span data-stu-id="72309-447">A unique name.</span></span> <span data-ttu-id="72309-448">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="72309-448">(Required.)</span></span>
-   <span data-ttu-id="72309-449">Clé d'entité définie par une ou plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="72309-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="72309-450">(Obligatoire.)</span><span class="sxs-lookup"><span data-stu-id="72309-450">(Required.)</span></span>
-   <span data-ttu-id="72309-451">Propriétés pour contenir des données.</span><span class="sxs-lookup"><span data-stu-id="72309-451">Properties for containing data.</span></span> <span data-ttu-id="72309-452">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="72309-452">(Optional.)</span></span>
-   <span data-ttu-id="72309-453">Propriétés de navigation qui permettent de naviguer d'une terminaison d'une association à l'autre terminaison.</span><span class="sxs-lookup"><span data-stu-id="72309-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="72309-454">(Facultatif)</span><span class="sxs-lookup"><span data-stu-id="72309-454">(Optional.)</span></span>

<span data-ttu-id="72309-455">Dans une application, une instance d'un type d'entité représente un objet spécifique (tel qu'un client ou une commande spécifique).</span><span class="sxs-lookup"><span data-stu-id="72309-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="72309-456">Chaque instance d'un type d'entité doit avoir une clé d'entité unique dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="72309-457">Deux instances de type d'entité sont considérées égales seulement si elles sont du même type et si les valeurs de leurs clés d'entité sont identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="72309-458">Un **EntityType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-459">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-460">Clé (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="72309-461">Propriété (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="72309-462">NavigationProperty (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="72309-463">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-464">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-464">Applicable Attributes</span></span>

<span data-ttu-id="72309-465">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="72309-466">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="72309-467">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-467">Is Required</span></span> | <span data-ttu-id="72309-468">Value</span><span class="sxs-lookup"><span data-stu-id="72309-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="72309-470">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-470">Yes</span></span>         | <span data-ttu-id="72309-471">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="72309-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="72309-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="72309-473">Non</span><span class="sxs-lookup"><span data-stu-id="72309-473">No</span></span>          | <span data-ttu-id="72309-474">Nom d'un autre type d'entité qui est le type de base du type d'entité en cours de définition.</span><span class="sxs-lookup"><span data-stu-id="72309-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="72309-475">**Abstract**</span><span class="sxs-lookup"><span data-stu-id="72309-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="72309-476">Non</span><span class="sxs-lookup"><span data-stu-id="72309-476">No</span></span>          | <span data-ttu-id="72309-477">**True** ou **False**, selon que le type d’entité est un type abstrait.</span><span class="sxs-lookup"><span data-stu-id="72309-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="72309-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="72309-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="72309-479">Non</span><span class="sxs-lookup"><span data-stu-id="72309-479">No</span></span>          | <span data-ttu-id="72309-480">**True** ou **False** selon que le type d’entité est un type d’entité ouverte.</span><span class="sxs-lookup"><span data-stu-id="72309-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="72309-481">> Le **OpenType** attribut s’applique uniquement aux types d’entités qui sont définies dans les modèles conceptuels utilisés avec ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="72309-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="72309-482">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EntityType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="72309-483">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-484">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-485">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-485">Example</span></span>

<span data-ttu-id="72309-486">L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments et deux **NavigationProperty** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="72309-487">EnumType, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="72309-488">Le **EnumType** élément représente un type énuméré.</span><span class="sxs-lookup"><span data-stu-id="72309-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="72309-489">Un **EnumType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-490">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-491">Membre (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="72309-492">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-493">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-493">Applicable Attributes</span></span>

<span data-ttu-id="72309-494">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **EnumType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="72309-495">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-495">Attribute Name</span></span>     | <span data-ttu-id="72309-496">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-496">Is Required</span></span> | <span data-ttu-id="72309-497">Value</span><span class="sxs-lookup"><span data-stu-id="72309-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-498">**Name**</span></span>           | <span data-ttu-id="72309-499">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-499">Yes</span></span>         | <span data-ttu-id="72309-500">Nom du type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="72309-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="72309-501">**IsFlags**</span></span>        | <span data-ttu-id="72309-502">Non</span><span class="sxs-lookup"><span data-stu-id="72309-502">No</span></span>          | <span data-ttu-id="72309-503">**True** ou **False**, selon si le type enum peut être utilisé comme un jeu d’indicateurs.</span><span class="sxs-lookup"><span data-stu-id="72309-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="72309-504">La valeur par défaut est **False.**.</span><span class="sxs-lookup"><span data-stu-id="72309-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="72309-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="72309-505">**UnderlyingType**</span></span> | <span data-ttu-id="72309-506">Non</span><span class="sxs-lookup"><span data-stu-id="72309-506">No</span></span>          | <span data-ttu-id="72309-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** ou **Edm.SByte** définition de la plage de valeurs du type.</span><span class="sxs-lookup"><span data-stu-id="72309-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="72309-508">La valeur par défaut, le type sous-jacent d’éléments de l’énumération est **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="72309-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-509">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **EnumType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="72309-510">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-511">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-512">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-512">Example</span></span>

<span data-ttu-id="72309-513">L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="72309-514">Function, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-514">Function Element (CSDL)</span></span>

<span data-ttu-id="72309-515">Le **fonction** élément dans le langage de définition de schéma conceptuel (CSDL) est utilisé pour définir ou déclarer des fonctions dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="72309-516">Une fonction est définie à l’aide d’un élément DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="72309-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="72309-517">Un **fonction** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-518">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-519">Paramètre (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="72309-520">DefiningExpression (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="72309-521">ReturnType (fonction) (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="72309-522">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="72309-523">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (Function) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="72309-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="72309-524">Les types de retour possibles correspondent à tout type EdmSimpleType, type d'entité, type complexe, type de ligne ou type REF (ou à une collection de l'un de ces types).</span><span class="sxs-lookup"><span data-stu-id="72309-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-525">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-525">Applicable Attributes</span></span>

<span data-ttu-id="72309-526">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="72309-527">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-527">Attribute Name</span></span> | <span data-ttu-id="72309-528">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-528">Is Required</span></span> | <span data-ttu-id="72309-529">Value</span><span class="sxs-lookup"><span data-stu-id="72309-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="72309-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-530">**Name**</span></span>       | <span data-ttu-id="72309-531">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-531">Yes</span></span>         | <span data-ttu-id="72309-532">Nom de la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-532">The name of the function.</span></span>          |
| <span data-ttu-id="72309-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="72309-533">**ReturnType**</span></span> | <span data-ttu-id="72309-534">Non</span><span class="sxs-lookup"><span data-stu-id="72309-534">No</span></span>          | <span data-ttu-id="72309-535">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-536">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **fonction** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="72309-537">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-538">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-539">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-539">Example</span></span>

<span data-ttu-id="72309-540">L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années écoulées depuis l’embauche d’un formateur.</span><span class="sxs-lookup"><span data-stu-id="72309-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="72309-541">FunctionImport, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="72309-542">Le **FunctionImport** élément de langage de définition de schéma conceptuel (CSDL) représente une fonction qui est définie dans la source de données mais disponible pour les objets via le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="72309-543">Par exemple, un élément de la fonction dans le modèle de stockage peut être utilisé pour représenter une procédure stockée dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="72309-544">Un **FunctionImport** élément dans le modèle conceptuel représente la fonction correspondante dans une application Entity Framework et est mappé à la fonction de modèle de stockage à l’aide de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="72309-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="72309-545">Lorsque la fonction est appelée dans l'application, la procédure stockée correspondante est exécutée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="72309-546">Le **FunctionImport** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-547">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-548">Paramètre (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="72309-549">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="72309-550">ReturnType (FunctionImport) (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="72309-551">Un **paramètre** élément doit être défini pour chaque paramètre qui accepte la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="72309-552">Un retour de type pour une fonction doit être spécifié avec le le **ReturnType** élément (FunctionImport) ou le **ReturnType** attribut (voir ci-dessous), mais pas les deux.</span><span class="sxs-lookup"><span data-stu-id="72309-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="72309-553">La valeur de type de retour doit être une collection de type EdmSimpleType, EntityType ou ComplexType.</span><span class="sxs-lookup"><span data-stu-id="72309-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-554">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-554">Applicable Attributes</span></span>

<span data-ttu-id="72309-555">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="72309-556">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-556">Attribute Name</span></span>   | <span data-ttu-id="72309-557">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-557">Is Required</span></span> | <span data-ttu-id="72309-558">Value</span><span class="sxs-lookup"><span data-stu-id="72309-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-559">**Name**</span></span>         | <span data-ttu-id="72309-560">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-560">Yes</span></span>         | <span data-ttu-id="72309-561">Nom de la fonction importée.</span><span class="sxs-lookup"><span data-stu-id="72309-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="72309-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="72309-562">**ReturnType**</span></span>   | <span data-ttu-id="72309-563">Non</span><span class="sxs-lookup"><span data-stu-id="72309-563">No</span></span>          | <span data-ttu-id="72309-564">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-564">The type that the function returns.</span></span> <span data-ttu-id="72309-565">N'utilisez pas cet attribut si la fonction ne retourne pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="72309-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="72309-566">Sinon, la valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="72309-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="72309-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="72309-567">**EntitySet**</span></span>    | <span data-ttu-id="72309-568">Non</span><span class="sxs-lookup"><span data-stu-id="72309-568">No</span></span>          | <span data-ttu-id="72309-569">Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection.</span><span class="sxs-lookup"><span data-stu-id="72309-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="72309-570">Sinon, le **EntitySet** attribut ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="72309-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="72309-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="72309-571">**IsComposable**</span></span> | <span data-ttu-id="72309-572">Non</span><span class="sxs-lookup"><span data-stu-id="72309-572">No</span></span>          | <span data-ttu-id="72309-573">Si la valeur est définie sur true, la fonction est composable (Table-valued Function) et peut être utilisée dans une requête LINQ.</span><span class="sxs-lookup"><span data-stu-id="72309-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="72309-574">La valeur par défaut est **false**.</span><span class="sxs-lookup"><span data-stu-id="72309-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="72309-575">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="72309-576">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-577">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-578">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-578">Example</span></span>

<span data-ttu-id="72309-579">L’exemple suivant montre un **FunctionImport** élément qui accepte un paramètre et retourne une collection de types d’entités :</span><span class="sxs-lookup"><span data-stu-id="72309-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="72309-580">Key, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-580">Key Element (CSDL)</span></span>

<span data-ttu-id="72309-581">Le **clé** élément est un élément enfant de l’élément EntityType et définit un *clé d’entité* (une propriété ou un ensemble de propriétés qui déterminent l’identité d’un type d’entité).</span><span class="sxs-lookup"><span data-stu-id="72309-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="72309-582">Les propriétés qui composent une clé d'entité sont choisies au moment du design.</span><span class="sxs-lookup"><span data-stu-id="72309-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="72309-583">Les valeurs des propriétés de clé d'entité doivent identifier de façon unique une instance de type d'entité dans un jeu d'entités au moment de l'exécution.</span><span class="sxs-lookup"><span data-stu-id="72309-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="72309-584">Les propriétés qui composent une clé d'entité doivent être choisies pour garantir l'unicité des instances dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="72309-585">Le **clé** élément définit une clé d’entité en faisant référence à un ou plusieurs des propriétés d’un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="72309-586">Le **clé** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="72309-587">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="72309-588">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-589">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-589">Applicable Attributes</span></span>

<span data-ttu-id="72309-590">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **clé** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="72309-591">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-592">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="72309-593">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-593">Example</span></span>

<span data-ttu-id="72309-594">L’exemple ci-dessous définit un type d’entité nommé **livre**.</span><span class="sxs-lookup"><span data-stu-id="72309-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="72309-595">La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="72309-596">Le **ISBN** propriété est un bon choix pour la clé d’entité, car un International Standard Book nombre (ISBN) identifie de façon unique un livre.</span><span class="sxs-lookup"><span data-stu-id="72309-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="72309-597">L’exemple suivant montre un type d’entité (**auteur**) qui a une clé d’entité qui se compose de deux propriétés : **nom** et **adresse**.</span><span class="sxs-lookup"><span data-stu-id="72309-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="72309-598">À l’aide de **nom** et **adresse** pour l’entité de clé est un choix raisonnable, étant donné que deux auteurs portant le même nom sont peu susceptibles de vie à la même adresse.</span><span class="sxs-lookup"><span data-stu-id="72309-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="72309-599">Toutefois, ce choix pour une clé d'entité ne garantit pas vraiment l'unicité des clés d'entité dans un jeu d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="72309-600">Ajout d’une propriété, tel que **AuthorId**, qui peut être utilisé pour identifier de manière unique un auteur est recommandé dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="72309-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="72309-601">Member, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-601">Member Element (CSDL)</span></span>

<span data-ttu-id="72309-602">Le **membre** élément est un élément enfant de l’élément EnumType et définit un membre du type énuméré.</span><span class="sxs-lookup"><span data-stu-id="72309-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-603">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-603">Applicable Attributes</span></span>

<span data-ttu-id="72309-604">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="72309-605">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-605">Attribute Name</span></span> | <span data-ttu-id="72309-606">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-606">Is Required</span></span> | <span data-ttu-id="72309-607">Value</span><span class="sxs-lookup"><span data-stu-id="72309-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-608">**Name**</span></span>       | <span data-ttu-id="72309-609">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-609">Yes</span></span>         | <span data-ttu-id="72309-610">Nom du membre.</span><span class="sxs-lookup"><span data-stu-id="72309-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="72309-611">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="72309-611">**Value**</span></span>      | <span data-ttu-id="72309-612">Non</span><span class="sxs-lookup"><span data-stu-id="72309-612">No</span></span>          | <span data-ttu-id="72309-613">Valeur du membre.</span><span class="sxs-lookup"><span data-stu-id="72309-613">The value of the member.</span></span> <span data-ttu-id="72309-614">Par défaut, le premier membre a la valeur 0, et la valeur de chaque énumérateur successif est incrémentée de 1.</span><span class="sxs-lookup"><span data-stu-id="72309-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="72309-615">Il existe peut-être plusieurs membres avec les mêmes valeurs.</span><span class="sxs-lookup"><span data-stu-id="72309-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-616">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **FunctionImport** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="72309-617">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-618">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-619">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-619">Example</span></span>

<span data-ttu-id="72309-620">L’exemple suivant montre un **EnumType** élément avec trois **membre** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="72309-621">NavigationProperty, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="72309-622">Un **NavigationProperty** élément définit une propriété de navigation, qui fournit une référence à l’autre extrémité d’une association.</span><span class="sxs-lookup"><span data-stu-id="72309-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="72309-623">Contrairement aux propriétés définies par l’élément de propriété, les propriétés de navigation ne définissent pas la forme et les caractéristiques des données.</span><span class="sxs-lookup"><span data-stu-id="72309-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="72309-624">Elles fournissent un moyen d'explorer une association entre deux types d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="72309-625">Notez que les propriétés de navigation sont facultatives sur les deux types d'entités au niveau des terminaisons d'une association.</span><span class="sxs-lookup"><span data-stu-id="72309-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="72309-626">Si vous définissez une propriété de navigation sur un type d'entité au niveau de la terminaison d'une association, il n'est pas nécessaire de définir une propriété de navigation sur le type d'entité à l'autre terminaison de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="72309-627">Le type de données retourné par une propriété de navigation est déterminé par la multiplicité de sa terminaison d'association distante.</span><span class="sxs-lookup"><span data-stu-id="72309-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="72309-628">Par exemple, une propriété de navigation, **OrdersNavProp**, existe sur un **client** type d’entité et de naviguer d’une association un-à-plusieurs entre **client** et  **Commande**.</span><span class="sxs-lookup"><span data-stu-id="72309-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="72309-629">Étant donné que la terminaison d’association distante pour la propriété de navigation a une multiplicité plusieurs (\*), son type de données est une collection (de **ordre**).</span><span class="sxs-lookup"><span data-stu-id="72309-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="72309-630">De même, si une propriété de navigation, **CustomerNavProp**, existe sur le **ordre** type d’entité, son type de données serait **client** dans la mesure où la multiplicité de la terminaison distante est un (1).</span><span class="sxs-lookup"><span data-stu-id="72309-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="72309-631">Un **NavigationProperty** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-632">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-633">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-634">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-634">Applicable Attributes</span></span>

<span data-ttu-id="72309-635">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **NavigationProperty** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="72309-636">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-636">Attribute Name</span></span>   | <span data-ttu-id="72309-637">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-637">Is Required</span></span> | <span data-ttu-id="72309-638">Value</span><span class="sxs-lookup"><span data-stu-id="72309-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-639">**Name**</span></span>         | <span data-ttu-id="72309-640">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-640">Yes</span></span>         | <span data-ttu-id="72309-641">Nom de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="72309-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="72309-642">**Relation**</span><span class="sxs-lookup"><span data-stu-id="72309-642">**Relationship**</span></span> | <span data-ttu-id="72309-643">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-643">Yes</span></span>         | <span data-ttu-id="72309-644">Nom d'une association figurant dans l'étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="72309-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="72309-645">**ToRole**</span></span>       | <span data-ttu-id="72309-646">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-646">Yes</span></span>         | <span data-ttu-id="72309-647">Terminaison de l'association à laquelle la navigation prend fin.</span><span class="sxs-lookup"><span data-stu-id="72309-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="72309-648">La valeur de la **ToRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="72309-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="72309-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="72309-649">**FromRole**</span></span>     | <span data-ttu-id="72309-650">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-650">Yes</span></span>         | <span data-ttu-id="72309-651">Terminaison de l'association où la navigation commence.</span><span class="sxs-lookup"><span data-stu-id="72309-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="72309-652">La valeur de la **FromRole** attribut doit être identique à la valeur de l’un de le **rôle** attributs définis sur l’une des terminaisons d’association (définies dans l’élément AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="72309-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-653">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **NavigationProperty** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="72309-654">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-655">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-656">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-656">Example</span></span>

<span data-ttu-id="72309-657">L’exemple suivant définit un type d’entité (**livre**) avec deux propriétés de navigation (**PublishedBy** et **inscrite**) :</span><span class="sxs-lookup"><span data-stu-id="72309-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="72309-658">OnDelete, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="72309-659">Le **OnDelete** élément dans le langage de définition de schéma conceptuel (CSDL) définit le comportement qui est connecté avec une association.</span><span class="sxs-lookup"><span data-stu-id="72309-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="72309-660">Si le **Action** attribut a la valeur **Cascade** sur une extrémité d’une association, liées à des types d’entités à l’autre extrémité de l’association sont supprimés lorsque le type d’entité de la première terminaison est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="72309-661">Si l’association entre deux types d’entité est une relation clé primaire / clé primaire, un objet dépendant chargé est supprimé lorsque l’objet principal à l’autre extrémité de l’association est supprimé, indépendamment de la **OnDelete** spécification.</span><span class="sxs-lookup"><span data-stu-id="72309-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="72309-662">Le **OnDelete** élément affecte uniquement le comportement d’exécution d’une application ; elle n’affecte pas le comportement de la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="72309-663">Le comportement défini dans la source de données doit être le même que le comportement défini dans l'application.</span><span class="sxs-lookup"><span data-stu-id="72309-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="72309-664">Un **OnDelete** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-665">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-666">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-667">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-667">Applicable Attributes</span></span>

<span data-ttu-id="72309-668">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **OnDelete** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="72309-669">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-669">Attribute Name</span></span> | <span data-ttu-id="72309-670">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-670">Is Required</span></span> | <span data-ttu-id="72309-671">Value</span><span class="sxs-lookup"><span data-stu-id="72309-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-672">**Action**</span><span class="sxs-lookup"><span data-stu-id="72309-672">**Action**</span></span>     | <span data-ttu-id="72309-673">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-673">Yes</span></span>         | <span data-ttu-id="72309-674">**Cascade** ou **aucun**.</span><span class="sxs-lookup"><span data-stu-id="72309-674">**Cascade** or **None**.</span></span> <span data-ttu-id="72309-675">Si **Cascade**, types d’entités dépendants sont supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="72309-676">Si **aucun**, types d’entités dépendants ne sont pas supprimés lorsque le type d’entité principal est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-677">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="72309-678">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-679">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-680">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-680">Example</span></span>

<span data-ttu-id="72309-681">L’exemple suivant montre un **Association** élément qui définit le **CustomerOrders** association.</span><span class="sxs-lookup"><span data-stu-id="72309-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="72309-682">Le **OnDelete** élément indique que tous les **commandes** qui sont liées à un particulier **client** et ont été chargés dans ObjectContext seront supprimés lorsque le  **Client** est supprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="72309-683">Parameter, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="72309-684">Le **paramètre** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément FunctionImport ou l’élément Function.</span><span class="sxs-lookup"><span data-stu-id="72309-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="72309-685">Application de l'élément FunctionImport</span><span class="sxs-lookup"><span data-stu-id="72309-685">FunctionImport Element Application</span></span>

<span data-ttu-id="72309-686">Un **paramètre** élément (en tant qu’enfant de le **FunctionImport** élément) est utilisé pour définir les paramètres d’entrée et de sortie pour les importations de fonction qui sont déclarées dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="72309-687">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-688">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-689">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-690">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-690">Applicable Attributes</span></span>

<span data-ttu-id="72309-691">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="72309-692">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-692">Attribute Name</span></span> | <span data-ttu-id="72309-693">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-693">Is Required</span></span> | <span data-ttu-id="72309-694">Value</span><span class="sxs-lookup"><span data-stu-id="72309-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-695">**Name**</span></span>       | <span data-ttu-id="72309-696">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-696">Yes</span></span>         | <span data-ttu-id="72309-697">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="72309-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-698">**Type**</span></span>       | <span data-ttu-id="72309-699">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-699">Yes</span></span>         | <span data-ttu-id="72309-700">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-700">The parameter type.</span></span> <span data-ttu-id="72309-701">La valeur doit être un **EDMSimpleType** ou un type complexe qui est dans l’étendue du modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="72309-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="72309-702">**Mode**</span></span>       | <span data-ttu-id="72309-703">Non</span><span class="sxs-lookup"><span data-stu-id="72309-703">No</span></span>          | <span data-ttu-id="72309-704">**Dans**, **Out**, ou **InOut** selon que le paramètre est un paramètre d’entrée, sortie ou d’entrée/sortie.</span><span class="sxs-lookup"><span data-stu-id="72309-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="72309-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-705">**MaxLength**</span></span>  | <span data-ttu-id="72309-706">Non</span><span class="sxs-lookup"><span data-stu-id="72309-706">No</span></span>          | <span data-ttu-id="72309-707">Longueur maximale autorisée du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="72309-708">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-708">**Precision**</span></span>  | <span data-ttu-id="72309-709">Non</span><span class="sxs-lookup"><span data-stu-id="72309-709">No</span></span>          | <span data-ttu-id="72309-710">Précision du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="72309-711">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-711">**Scale**</span></span>      | <span data-ttu-id="72309-712">Non</span><span class="sxs-lookup"><span data-stu-id="72309-712">No</span></span>          | <span data-ttu-id="72309-713">Échelle du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="72309-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-714">**SRID**</span></span>       | <span data-ttu-id="72309-715">Non</span><span class="sxs-lookup"><span data-stu-id="72309-715">No</span></span>          | <span data-ttu-id="72309-716">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-717">Valide uniquement pour les paramètres des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="72309-718">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-719">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="72309-720">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-721">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-722">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-722">Example</span></span>

<span data-ttu-id="72309-723">L’exemple suivant montre un **FunctionImport** élément avec un **paramètre** élément enfant.</span><span class="sxs-lookup"><span data-stu-id="72309-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="72309-724">La fonction accepte un paramètre d'entrée et retourne une collection de types d'entités.</span><span class="sxs-lookup"><span data-stu-id="72309-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="72309-725">Application de l'élément Function</span><span class="sxs-lookup"><span data-stu-id="72309-725">Function Element Application</span></span>

<span data-ttu-id="72309-726">Un **paramètre** élément (en tant qu’enfant de le **fonction** élément) définit les paramètres pour les fonctions qui sont définies ou déclarées dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="72309-727">Le **paramètre** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-728">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="72309-729">CollectionType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="72309-730">ReferenceType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="72309-731">RowType (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="72309-732">Seul l’un de la **CollectionType**, **ReferenceType**, ou **RowType** éléments peuvent être un élément enfant d’un **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="72309-733">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="72309-734">Les éléments d'annotation doivent figurer après tous les autres éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="72309-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="72309-735">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="72309-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-736">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-736">Applicable Attributes</span></span>

<span data-ttu-id="72309-737">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="72309-738">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-738">Attribute Name</span></span>   | <span data-ttu-id="72309-739">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-739">Is Required</span></span> | <span data-ttu-id="72309-740">Value</span><span class="sxs-lookup"><span data-stu-id="72309-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-741">**Name**</span></span>         | <span data-ttu-id="72309-742">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-742">Yes</span></span>         | <span data-ttu-id="72309-743">Nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="72309-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-744">**Type**</span></span>         | <span data-ttu-id="72309-745">Non</span><span class="sxs-lookup"><span data-stu-id="72309-745">No</span></span>          | <span data-ttu-id="72309-746">Type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="72309-746">The parameter type.</span></span> <span data-ttu-id="72309-747">Un paramètre peut correspondre à l'un quelconque des types suivants (ou à des collections de ces types) :</span><span class="sxs-lookup"><span data-stu-id="72309-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="72309-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="72309-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="72309-749">type d'entité</span><span class="sxs-lookup"><span data-stu-id="72309-749">entity type</span></span> <br/> <span data-ttu-id="72309-750">type complexe</span><span class="sxs-lookup"><span data-stu-id="72309-750">complex type</span></span> <br/> <span data-ttu-id="72309-751">type de ligne</span><span class="sxs-lookup"><span data-stu-id="72309-751">row type</span></span> <br/> <span data-ttu-id="72309-752">type référence</span><span class="sxs-lookup"><span data-stu-id="72309-752">reference type</span></span>                             |
| <span data-ttu-id="72309-753">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-753">**Nullable**</span></span>     | <span data-ttu-id="72309-754">Non</span><span class="sxs-lookup"><span data-stu-id="72309-754">No</span></span>          | <span data-ttu-id="72309-755">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir un **null** valeur.</span><span class="sxs-lookup"><span data-stu-id="72309-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="72309-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="72309-756">**DefaultValue**</span></span> | <span data-ttu-id="72309-757">Non</span><span class="sxs-lookup"><span data-stu-id="72309-757">No</span></span>          | <span data-ttu-id="72309-758">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="72309-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-759">**MaxLength**</span></span>    | <span data-ttu-id="72309-760">Non</span><span class="sxs-lookup"><span data-stu-id="72309-760">No</span></span>          | <span data-ttu-id="72309-761">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="72309-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-762">**FixedLength**</span></span>  | <span data-ttu-id="72309-763">Non</span><span class="sxs-lookup"><span data-stu-id="72309-763">No</span></span>          | <span data-ttu-id="72309-764">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="72309-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="72309-765">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-765">**Precision**</span></span>    | <span data-ttu-id="72309-766">Non</span><span class="sxs-lookup"><span data-stu-id="72309-766">No</span></span>          | <span data-ttu-id="72309-767">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="72309-768">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-768">**Scale**</span></span>        | <span data-ttu-id="72309-769">Non</span><span class="sxs-lookup"><span data-stu-id="72309-769">No</span></span>          | <span data-ttu-id="72309-770">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="72309-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-771">**SRID**</span></span>         | <span data-ttu-id="72309-772">Non</span><span class="sxs-lookup"><span data-stu-id="72309-772">No</span></span>          | <span data-ttu-id="72309-773">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-774">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="72309-775">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="72309-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-776">**Unicode**</span></span>      | <span data-ttu-id="72309-777">Non</span><span class="sxs-lookup"><span data-stu-id="72309-777">No</span></span>          | <span data-ttu-id="72309-778">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="72309-779">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-779">**Collation**</span></span>    | <span data-ttu-id="72309-780">Non</span><span class="sxs-lookup"><span data-stu-id="72309-780">No</span></span>          | <span data-ttu-id="72309-781">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="72309-782">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **paramètre** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="72309-783">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-784">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-785">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-785">Example</span></span>

<span data-ttu-id="72309-786">L’exemple suivant montre un **fonction** élément qui utilise un **paramètre** élément enfant pour définir un paramètre de fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="72309-787">Principal, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="72309-788">Le **Principal** en langage de définition de schéma conceptuel (CSDL) est un élément enfant à l’élément ReferentialConstraint qui définit la terminaison principale d’une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="72309-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="72309-789">Un **ReferentialConstraint** élément définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="72309-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="72309-790">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="72309-791">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="72309-792">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="72309-793">**PropertyRef** éléments sont utilisés pour spécifier les clés référencées par la terminaison dépendante.</span><span class="sxs-lookup"><span data-stu-id="72309-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="72309-794">Le **Principal** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-795">PropertyRef (un ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="72309-796">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-797">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-797">Applicable Attributes</span></span>

<span data-ttu-id="72309-798">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="72309-799">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-799">Attribute Name</span></span> | <span data-ttu-id="72309-800">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-800">Is Required</span></span> | <span data-ttu-id="72309-801">Value</span><span class="sxs-lookup"><span data-stu-id="72309-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="72309-802">**Rôle**</span><span class="sxs-lookup"><span data-stu-id="72309-802">**Role**</span></span>       | <span data-ttu-id="72309-803">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-803">Yes</span></span>         | <span data-ttu-id="72309-804">Nom du type d'entité au niveau de la terminaison principale de l'association.</span><span class="sxs-lookup"><span data-stu-id="72309-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-805">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Principal** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="72309-806">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-807">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-808">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-808">Example</span></span>

<span data-ttu-id="72309-809">L’exemple suivant montre un **ReferentialConstraint** élément qui fait partie de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="72309-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="72309-810">Le **Id** propriété de la **Publisher** type d’entité compose la terminaison principale de la contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="72309-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="72309-811">Property, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-811">Property Element (CSDL)</span></span>

<span data-ttu-id="72309-812">Le **propriété** élément dans le langage de définition de schéma conceptuel (CSDL) peut être un enfant de l’élément EntityType, ComplexType, élément ou l’élément RowType.</span><span class="sxs-lookup"><span data-stu-id="72309-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="72309-813">Applications des éléments EntityType et ComplexType</span><span class="sxs-lookup"><span data-stu-id="72309-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="72309-814">**Propriété** éléments (en tant qu’enfants de **EntityType** ou **ComplexType** éléments) définissent la forme et les caractéristiques des données qui contient une instance de type d’entité ou d’une instance de type complexe .</span><span class="sxs-lookup"><span data-stu-id="72309-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="72309-815">Les propriétés dans un modèle conceptuel sont analogues aux propriétés qui sont définies sur une classe.</span><span class="sxs-lookup"><span data-stu-id="72309-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="72309-816">De même que les propriétés sur une classe définissent la forme de la classe et acheminent des informations sur les objets, les propriétés dans un modèle conceptuel définissent la forme d'un type d'entité et acheminent des informations sur les instances de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="72309-817">Le **propriété** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-818">Documentation, élément (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-819">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="72309-820">Les facettes suivantes peuvent être appliqués à un **propriété** élément : **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **précision**, **mise à l’échelle**, **Unicode**, **classement**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="72309-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="72309-821">Les facettes sont des attributs XML qui fournissent des informations sur la manière dont les valeurs de propriété sont stockées dans la banque de données.</span><span class="sxs-lookup"><span data-stu-id="72309-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-822">Facettes peuvent uniquement être appliqués aux propriétés de type **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="72309-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-823">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-823">Applicable Attributes</span></span>

<span data-ttu-id="72309-824">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="72309-825">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-825">Attribute Name</span></span>                                                         | <span data-ttu-id="72309-826">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-826">Is Required</span></span> | <span data-ttu-id="72309-827">Value</span><span class="sxs-lookup"><span data-stu-id="72309-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-828">**Name**</span></span>                                                               | <span data-ttu-id="72309-829">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-829">Yes</span></span>         | <span data-ttu-id="72309-830">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="72309-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-831">**Type**</span></span>                                                               | <span data-ttu-id="72309-832">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-832">Yes</span></span>         | <span data-ttu-id="72309-833">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-833">The type of the property value.</span></span> <span data-ttu-id="72309-834">Le type de valeur de propriété doit être un **EDMSimpleType** ou un type complexe (indiqué par un nom qualifié complet) qui se trouve dans la portée du modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="72309-835">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-835">**Nullable**</span></span>                                                           | <span data-ttu-id="72309-836">Non</span><span class="sxs-lookup"><span data-stu-id="72309-836">No</span></span>          | <span data-ttu-id="72309-837">**True** (la valeur par défaut) ou <strong>False</strong> selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="72309-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="72309-838">> Dans le langage CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="72309-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="72309-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="72309-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="72309-840">Non</span><span class="sxs-lookup"><span data-stu-id="72309-840">No</span></span>          | <span data-ttu-id="72309-841">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="72309-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="72309-843">Non</span><span class="sxs-lookup"><span data-stu-id="72309-843">No</span></span>          | <span data-ttu-id="72309-844">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="72309-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="72309-846">Non</span><span class="sxs-lookup"><span data-stu-id="72309-846">No</span></span>          | <span data-ttu-id="72309-847">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="72309-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="72309-848">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-848">**Precision**</span></span>                                                          | <span data-ttu-id="72309-849">Non</span><span class="sxs-lookup"><span data-stu-id="72309-849">No</span></span>          | <span data-ttu-id="72309-850">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="72309-851">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-851">**Scale**</span></span>                                                              | <span data-ttu-id="72309-852">Non</span><span class="sxs-lookup"><span data-stu-id="72309-852">No</span></span>          | <span data-ttu-id="72309-853">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="72309-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-854">**SRID**</span></span>                                                               | <span data-ttu-id="72309-855">Non</span><span class="sxs-lookup"><span data-stu-id="72309-855">No</span></span>          | <span data-ttu-id="72309-856">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-857">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="72309-858">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="72309-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-859">**Unicode**</span></span>                                                            | <span data-ttu-id="72309-860">Non</span><span class="sxs-lookup"><span data-stu-id="72309-860">No</span></span>          | <span data-ttu-id="72309-861">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="72309-862">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-862">**Collation**</span></span>                                                          | <span data-ttu-id="72309-863">Non</span><span class="sxs-lookup"><span data-stu-id="72309-863">No</span></span>          | <span data-ttu-id="72309-864">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="72309-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="72309-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="72309-866">Non</span><span class="sxs-lookup"><span data-stu-id="72309-866">No</span></span>          | <span data-ttu-id="72309-867">**Aucun** (la valeur par défaut) ou **fixe**.</span><span class="sxs-lookup"><span data-stu-id="72309-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="72309-868">Si la valeur est définie sur **fixe**, la valeur de propriété sera utilisée dans les contrôles d’accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="72309-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="72309-869">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="72309-870">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-871">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-872">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-872">Example</span></span>

<span data-ttu-id="72309-873">L’exemple suivant montre un **EntityType** élément avec trois **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="72309-874">L’exemple suivant montre un **ComplexType** élément avec cinq **propriété** éléments :</span><span class="sxs-lookup"><span data-stu-id="72309-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="72309-875">Application de l'élément RowType</span><span class="sxs-lookup"><span data-stu-id="72309-875">RowType Element Application</span></span>

<span data-ttu-id="72309-876">**Propriété** éléments (comme enfants d’un **RowType** élément) définissent la forme et les caractéristiques des données qui peuvent être passées à ou retournées à partir d’une fonction définie par le modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="72309-877">Le **propriété** élément peut avoir exactement un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="72309-878">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="72309-878">CollectionType</span></span>
-   <span data-ttu-id="72309-879">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="72309-879">ReferenceType</span></span>
-   <span data-ttu-id="72309-880">RowType ;</span><span class="sxs-lookup"><span data-stu-id="72309-880">RowType</span></span>

<span data-ttu-id="72309-881">Le **propriété** élément peut avoir n’importe quel nombre éléments d’annotation enfant.</span><span class="sxs-lookup"><span data-stu-id="72309-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-882">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="72309-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="72309-883">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-883">Applicable Attributes</span></span>

<span data-ttu-id="72309-884">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="72309-885">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-885">Attribute Name</span></span>                                                     | <span data-ttu-id="72309-886">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-886">Is Required</span></span> | <span data-ttu-id="72309-887">Value</span><span class="sxs-lookup"><span data-stu-id="72309-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-888">**Name**</span></span>                                                           | <span data-ttu-id="72309-889">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-889">Yes</span></span>         | <span data-ttu-id="72309-890">Nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="72309-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-891">**Type**</span></span>                                                           | <span data-ttu-id="72309-892">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-892">Yes</span></span>         | <span data-ttu-id="72309-893">Type de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="72309-894">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-894">**Nullable**</span></span>                                                       | <span data-ttu-id="72309-895">Non</span><span class="sxs-lookup"><span data-stu-id="72309-895">No</span></span>          | <span data-ttu-id="72309-896">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="72309-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="72309-897">> Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="72309-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="72309-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="72309-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="72309-899">Non</span><span class="sxs-lookup"><span data-stu-id="72309-899">No</span></span>          | <span data-ttu-id="72309-900">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="72309-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="72309-902">Non</span><span class="sxs-lookup"><span data-stu-id="72309-902">No</span></span>          | <span data-ttu-id="72309-903">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="72309-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="72309-905">Non</span><span class="sxs-lookup"><span data-stu-id="72309-905">No</span></span>          | <span data-ttu-id="72309-906">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="72309-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="72309-907">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-907">**Precision**</span></span>                                                      | <span data-ttu-id="72309-908">Non</span><span class="sxs-lookup"><span data-stu-id="72309-908">No</span></span>          | <span data-ttu-id="72309-909">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="72309-910">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-910">**Scale**</span></span>                                                          | <span data-ttu-id="72309-911">Non</span><span class="sxs-lookup"><span data-stu-id="72309-911">No</span></span>          | <span data-ttu-id="72309-912">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="72309-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-913">**SRID**</span></span>                                                           | <span data-ttu-id="72309-914">Non</span><span class="sxs-lookup"><span data-stu-id="72309-914">No</span></span>          | <span data-ttu-id="72309-915">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-916">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="72309-917">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="72309-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-918">**Unicode**</span></span>                                                        | <span data-ttu-id="72309-919">Non</span><span class="sxs-lookup"><span data-stu-id="72309-919">No</span></span>          | <span data-ttu-id="72309-920">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="72309-921">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-921">**Collation**</span></span>                                                      | <span data-ttu-id="72309-922">Non</span><span class="sxs-lookup"><span data-stu-id="72309-922">No</span></span>          | <span data-ttu-id="72309-923">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="72309-924">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **propriété** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="72309-925">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-926">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="72309-927">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-927">Example</span></span>

<span data-ttu-id="72309-928">L’exemple suivant **propriété** éléments utilisés pour définir la forme du type de retour d’une fonction définie par modèle.</span><span class="sxs-lookup"><span data-stu-id="72309-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="72309-929">PropertyRef, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="72309-930">Le **PropertyRef** élément dans le langage de définition de schéma conceptuel (CSDL) fait référence à une propriété d’un type d’entité pour indiquer que la propriété effectuera l’un des rôles suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="72309-931">Partie de la clé de l'entité (une propriété ou un jeu de propriétés d'un type d'entité qui détermine l'identité).</span><span class="sxs-lookup"><span data-stu-id="72309-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="72309-932">Un ou plusieurs **PropertyRef** éléments peuvent être utilisés pour définir une clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="72309-933">Terminaison dépendante ou principale d'une contrainte référentielle.</span><span class="sxs-lookup"><span data-stu-id="72309-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="72309-934">Le **PropertyRef** élément peut avoir uniquement des éléments d’annotation (zéro ou plus) en tant qu’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="72309-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-935">Les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="72309-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="72309-936">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-936">Applicable Attributes</span></span>

<span data-ttu-id="72309-937">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="72309-938">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-938">Attribute Name</span></span> | <span data-ttu-id="72309-939">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-939">Is Required</span></span> | <span data-ttu-id="72309-940">Value</span><span class="sxs-lookup"><span data-stu-id="72309-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="72309-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="72309-941">**Name**</span></span>       | <span data-ttu-id="72309-942">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-942">Yes</span></span>         | <span data-ttu-id="72309-943">Nom de la propriété référencée.</span><span class="sxs-lookup"><span data-stu-id="72309-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-944">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **PropertyRef** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="72309-945">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-946">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-947">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-947">Example</span></span>

<span data-ttu-id="72309-948">L’exemple ci-dessous définit un type d’entité (**livre**).</span><span class="sxs-lookup"><span data-stu-id="72309-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="72309-949">La clé d’entité est définie en référençant le **ISBN** propriété du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="72309-950">Dans l’exemple suivant, deux **PropertyRef** éléments sont utilisés pour indiquer que deux propriétés (**Id** et **PublisherId**) sont les terminaisons principales et dépendantes d’un référentiel contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="72309-951">Élément ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="72309-952">Le **ReferenceType** élément de langage de définition de schéma conceptuel (CSDL) spécifie une référence à un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="72309-953">Le **ReferenceType** élément peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="72309-954">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="72309-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="72309-955">Paramètre</span><span class="sxs-lookup"><span data-stu-id="72309-955">Parameter</span></span>
-   <span data-ttu-id="72309-956">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="72309-956">CollectionType</span></span>

<span data-ttu-id="72309-957">Le **ReferenceType** élément est utilisé lors de la définition d’un type de paramètre ou de retour pour une fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="72309-958">Un **ReferenceType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-959">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-960">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-961">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-961">Applicable Attributes</span></span>

<span data-ttu-id="72309-962">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **ReferenceType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="72309-963">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-963">Attribute Name</span></span> | <span data-ttu-id="72309-964">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-964">Is Required</span></span> | <span data-ttu-id="72309-965">Value</span><span class="sxs-lookup"><span data-stu-id="72309-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="72309-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-966">**Type**</span></span>       | <span data-ttu-id="72309-967">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-967">Yes</span></span>         | <span data-ttu-id="72309-968">Nom du type d'entité référencé.</span><span class="sxs-lookup"><span data-stu-id="72309-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-969">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReferenceType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="72309-970">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-971">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-972">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-972">Example</span></span>

<span data-ttu-id="72309-973">L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **paramètre** élément dans une fonction définie par modèle qui accepte une référence à un **personne** entité type :</span><span class="sxs-lookup"><span data-stu-id="72309-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="72309-974">L’exemple suivant montre le **ReferenceType** élément utilisé en tant qu’enfant d’un **ReturnType** élément (fonction) dans une fonction définie par modèle qui retourne une référence à un **personne**type d’entité :</span><span class="sxs-lookup"><span data-stu-id="72309-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="72309-975">ReferentialConstraint, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="72309-976">Un **ReferentialConstraint** élément dans le langage de définition de schéma conceptuel (CSDL) définit la fonctionnalité qui est similaire à une contrainte d’intégrité référentielle dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="72309-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="72309-977">De la même manière qu'une ou plusieurs colonnes d'une table de base de données peuvent référencer la clé primaire d'une autre table, une ou plusieurs propriétés d'un type d'entité peuvent référencer la clé d'entité d'un autre type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="72309-978">Le type d’entité référencé est appelé le *terminaison principale* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="72309-979">Le type d’entité qui fait référence à la terminaison principale est appelé le *terminaison dépendante* de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="72309-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="72309-980">Si une clé étrangère qui est exposée sur un type d’entité fait référence à une propriété sur un autre type d’entité, le **ReferentialConstraint** élément définit une association entre les deux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="72309-981">Étant donné que le **ReferentialConstraint** élément fournit des informations sur la manière dont deux types d’entités sont liées, aucun élément AssociationSetMapping correspondant n’est nécessaire dans le langage de spécification de mappage (MSL).</span><span class="sxs-lookup"><span data-stu-id="72309-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="72309-982">Une association entre deux types d’entités qui n’ont pas de clés étrangères exposées doit avoir un correspondant **AssociationSetMapping** élément afin de mapper les informations d’association à la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="72309-983">Si une clé étrangère n’est pas exposée sur un type d’entité, le **ReferentialConstraint** élément peut uniquement définir une contrainte de clé primaire / clé primaire entre le type d’entité et un autre type d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="72309-984">Un **ReferentialConstraint** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-985">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-986">Principal (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="72309-987">Dépendante (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="72309-988">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-989">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-989">Applicable Attributes</span></span>

<span data-ttu-id="72309-990">Le **ReferentialConstraint** élément peut avoir n’importe quel nombre d’attributs d’annotation (attributs XML personnalisés).</span><span class="sxs-lookup"><span data-stu-id="72309-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="72309-991">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-992">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="72309-993">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-993">Example</span></span>

<span data-ttu-id="72309-994">L’exemple suivant montre un **ReferentialConstraint** élément utilisé dans le cadre de la définition de la **PublishedBy** association.</span><span class="sxs-lookup"><span data-stu-id="72309-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="72309-995">Élément ReturnType (fonction) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="72309-996">Le **ReturnType** (Function), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément de la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="72309-997">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="72309-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="72309-998">Retourner les types peuvent être les **EdmSimpleType**, type d’entité, type complexe, type de ligne, type ref ou une collection d’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="72309-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="72309-999">Le type de retour d’une fonction peut être spécifié avec soit le **Type** attribut de la **ReturnType** élément (fonction), ou avec l’un des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="72309-1000">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="72309-1000">CollectionType</span></span>
-   <span data-ttu-id="72309-1001">ReferenceType ;</span><span class="sxs-lookup"><span data-stu-id="72309-1001">ReferenceType</span></span>
-   <span data-ttu-id="72309-1002">RowType ;</span><span class="sxs-lookup"><span data-stu-id="72309-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="72309-1003">Un modèle ne sera pas validé si vous spécifiez une fonction de type de retour avec à la fois le **Type** attribut de la **ReturnType** élément (Function) et l’autre des éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="72309-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1004">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1004">Applicable Attributes</span></span>

<span data-ttu-id="72309-1005">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="72309-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="72309-1006">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-1006">Attribute Name</span></span> | <span data-ttu-id="72309-1007">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-1007">Is Required</span></span> | <span data-ttu-id="72309-1008">Value</span><span class="sxs-lookup"><span data-stu-id="72309-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="72309-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="72309-1009">**ReturnType**</span></span> | <span data-ttu-id="72309-1010">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1010">No</span></span>          | <span data-ttu-id="72309-1011">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-1012">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="72309-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="72309-1013">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1014">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-1015">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1015">Example</span></span>

<span data-ttu-id="72309-1016">L’exemple suivant utilise un **fonction** élément pour définir une fonction qui retourne le nombre d’années un livre a été imprimé.</span><span class="sxs-lookup"><span data-stu-id="72309-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="72309-1017">Notez que le type de retour est spécifié par le **Type** attribut d’un **ReturnType** élément (Function).</span><span class="sxs-lookup"><span data-stu-id="72309-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="72309-1018">ReturnType (FunctionImport) élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="72309-1019">Le **ReturnType** (FunctionImport), élément de langage de définition de schéma conceptuel (CSDL) spécifie le type de retour pour une fonction qui est défini dans un élément FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="72309-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="72309-1020">Une fonction de type de retour peut également être spécifié avec un **ReturnType** attribut.</span><span class="sxs-lookup"><span data-stu-id="72309-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="72309-1021">Retourner les types peuvent être n’importe quelle collection de type d’entité, type complexe, ou **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="72309-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="72309-1022">Le type de retour d’une fonction est spécifié avec la **Type** attribut de la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1023">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1023">Applicable Attributes</span></span>

<span data-ttu-id="72309-1024">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="72309-1025">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-1025">Attribute Name</span></span> | <span data-ttu-id="72309-1026">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-1026">Is Required</span></span> | <span data-ttu-id="72309-1027">Value</span><span class="sxs-lookup"><span data-stu-id="72309-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-1028">**Type**</span></span>       | <span data-ttu-id="72309-1029">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1029">No</span></span>          | <span data-ttu-id="72309-1030">Type retourné par la fonction.</span><span class="sxs-lookup"><span data-stu-id="72309-1030">The type that the function returns.</span></span> <span data-ttu-id="72309-1031">La valeur doit être une collection de ComplexType, d’EntityType ou d’EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="72309-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="72309-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="72309-1032">**EntitySet**</span></span>  | <span data-ttu-id="72309-1033">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1033">No</span></span>          | <span data-ttu-id="72309-1034">Si la fonction retourne une collection d’entités de types, la valeur de la **EntitySet** doit être la jeu d’entités auquel appartient la collection.</span><span class="sxs-lookup"><span data-stu-id="72309-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="72309-1035">Sinon, le **EntitySet** attribut ne doit pas être utilisé.</span><span class="sxs-lookup"><span data-stu-id="72309-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-1036">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **ReturnType** (FunctionImport) élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="72309-1037">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1038">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-1039">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1039">Example</span></span>

<span data-ttu-id="72309-1040">L’exemple suivant utilise un **FunctionImport** qui retourne la documentation et les serveurs de publication.</span><span class="sxs-lookup"><span data-stu-id="72309-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="72309-1041">Notez que la fonction retourne deux jeux de résultats et par conséquent deux **ReturnType** (FunctionImport) éléments sont spécifiés.</span><span class="sxs-lookup"><span data-stu-id="72309-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="72309-1042">Élément RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="72309-1043">Un **RowType** élément dans le langage de définition de schéma conceptuel (CSDL) définit une structure sans nom comme paramètre ou type de retour pour une fonction définie dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="72309-1044">Un **RowType** élément peut être l’enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="72309-1045">CollectionType ;</span><span class="sxs-lookup"><span data-stu-id="72309-1045">CollectionType</span></span>
-   <span data-ttu-id="72309-1046">Paramètre</span><span class="sxs-lookup"><span data-stu-id="72309-1046">Parameter</span></span>
-   <span data-ttu-id="72309-1047">ReturnType (fonction)</span><span class="sxs-lookup"><span data-stu-id="72309-1047">ReturnType (Function)</span></span>

<span data-ttu-id="72309-1048">Un **RowType** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-1049">Propriété (un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="72309-1049">Property (one or more)</span></span>
-   <span data-ttu-id="72309-1050">Éléments d’annotation (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="72309-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1051">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1051">Applicable Attributes</span></span>

<span data-ttu-id="72309-1052">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **RowType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="72309-1053">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1054">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="72309-1055">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1055">Example</span></span>

<span data-ttu-id="72309-1056">L’exemple suivant montre une fonction définie par modèle qui utilise un **CollectionType** élément pour spécifier que la fonction retourne une collection de lignes (tel que spécifié dans le **RowType** élément).</span><span class="sxs-lookup"><span data-stu-id="72309-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="72309-1057">Schema, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="72309-1058">Le **schéma** élément est l’élément racine d’une définition de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="72309-1059">Il contient des définitions pour les objets, fonctions et conteneurs qui composent un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="72309-1060">Le **schéma** élément peut contenir zéro ou plusieurs des éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="72309-1061">Using</span><span class="sxs-lookup"><span data-stu-id="72309-1061">Using</span></span>
-   <span data-ttu-id="72309-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="72309-1062">EntityContainer</span></span>
-   <span data-ttu-id="72309-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="72309-1063">EntityType</span></span>
-   <span data-ttu-id="72309-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="72309-1064">EnumType</span></span>
-   <span data-ttu-id="72309-1065">Association</span><span class="sxs-lookup"><span data-stu-id="72309-1065">Association</span></span>
-   <span data-ttu-id="72309-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="72309-1066">ComplexType</span></span>
-   <span data-ttu-id="72309-1067">Fonction</span><span class="sxs-lookup"><span data-stu-id="72309-1067">Function</span></span>

<span data-ttu-id="72309-1068">Un **schéma** élément peut contenir zéro ou un élément d’Annotation.</span><span class="sxs-lookup"><span data-stu-id="72309-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-1069">Le **fonction** élément et les éléments d’annotation sont autorisés uniquement dans CSDL v2 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="72309-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="72309-1070">Le **schéma** élément utilise le **Namespace** attribut permettant de définir l’espace de noms pour le type d’entité, type complexe et objets d’association dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="72309-1071">Dans un espace de noms, deux objets ne peuvent pas avoir le même nom.</span><span class="sxs-lookup"><span data-stu-id="72309-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="72309-1072">Espaces de noms peuvent couvrir plusieurs **schéma** éléments et plusieurs fichiers .csdl.</span><span class="sxs-lookup"><span data-stu-id="72309-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="72309-1073">Un espace de noms du modèle conceptuel est différent de l’espace de noms XML de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="72309-1074">Un espace de noms de modèle conceptuel (tel que défini par le **Namespace** attribut) est un conteneur logique pour les types d’entités, types complexes et types d’associations.</span><span class="sxs-lookup"><span data-stu-id="72309-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="72309-1075">L’espace de noms XML (indiqué par le **xmlns** attribut) d’un **schéma** élément est l’espace de noms par défaut pour les éléments enfants et attributs de la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="72309-1076">Espaces de noms XML sous la forme http://schemas.microsoft.com/ado/YYYY/MM/edm (où AAAA et MM représentent une année et mois respectivement) sont réservés pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="72309-1077">Des éléments et attributs personnalisés ne peuvent pas être dans des espaces de noms de cette forme.</span><span class="sxs-lookup"><span data-stu-id="72309-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1078">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1078">Applicable Attributes</span></span>

<span data-ttu-id="72309-1079">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="72309-1080">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-1080">Attribute Name</span></span> | <span data-ttu-id="72309-1081">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-1081">Is Required</span></span> | <span data-ttu-id="72309-1082">Value</span><span class="sxs-lookup"><span data-stu-id="72309-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-1083">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="72309-1083">**Namespace**</span></span>  | <span data-ttu-id="72309-1084">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1084">Yes</span></span>         | <span data-ttu-id="72309-1085">Espace de noms du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="72309-1086">La valeur de la **Namespace** attribut est utilisé pour former le nom qualifié complet d’un type.</span><span class="sxs-lookup"><span data-stu-id="72309-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="72309-1087">Par exemple, si un **EntityType** nommé *client* figure dans l’espace de noms Simple.Example.Model, le nom qualifié complet de le **EntityType** est SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="72309-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="72309-1088">Les chaînes suivantes ne peut pas être utilisés comme valeur pour le **Namespace** attribut : **système**, **temporaires**, ou **Edm**.</span><span class="sxs-lookup"><span data-stu-id="72309-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="72309-1089">La valeur de la **Namespace** attribut ne peut pas être identique à la valeur de la **Namespace** attribut dans l’élément de schéma SSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="72309-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="72309-1090">**Alias**</span></span>      | <span data-ttu-id="72309-1091">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1091">No</span></span>          | <span data-ttu-id="72309-1092">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="72309-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="72309-1093">Par exemple, si un **EntityType** nommé *client* est dans l’espace de noms Simple.Example.Model et la valeur de la **Alias** attribut est *modèle*, vous pouvez utiliser modèle.client comme nom qualifié complet de le **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="72309-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="72309-1094">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="72309-1095">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1096">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-1097">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1097">Example</span></span>

<span data-ttu-id="72309-1098">L’exemple suivant montre un **schéma** élément contenant un **EntityContainer** élément, deux **EntityType** éléments et l’autre **Association** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="72309-1099">Élément TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="72309-1100">Le **TypeRef** élément dans le langage de définition de schéma conceptuel (CSDL) fournit une référence à un type nommé existant.</span><span class="sxs-lookup"><span data-stu-id="72309-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="72309-1101">Le **TypeRef** élément peut être un enfant de l’élément CollectionType, qui est utilisé pour spécifier qu’une fonction a une collection comme un paramètre ou type de retour.</span><span class="sxs-lookup"><span data-stu-id="72309-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="72309-1102">Un **TypeRef** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="72309-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="72309-1103">Documentation (zéro ou un élément)</span><span class="sxs-lookup"><span data-stu-id="72309-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="72309-1104">Éléments d’annotation (zéro ou plusieurs éléments)</span><span class="sxs-lookup"><span data-stu-id="72309-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1105">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1105">Applicable Attributes</span></span>

<span data-ttu-id="72309-1106">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **TypeRef** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="72309-1107">Notez que le **DefaultValue**, **MaxLength**, **FixedLength**, **précision**, **mise à l’échelle**,  **Unicode**, et **classement** attributs sont uniquement applicables à **types Edmsimpletype**.</span><span class="sxs-lookup"><span data-stu-id="72309-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="72309-1108">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="72309-1109">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-1109">Is Required</span></span> | <span data-ttu-id="72309-1110">Value</span><span class="sxs-lookup"><span data-stu-id="72309-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="72309-1111">**Type**</span></span>                                                           | <span data-ttu-id="72309-1112">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1112">No</span></span>          | <span data-ttu-id="72309-1113">Nom du type référencé.</span><span class="sxs-lookup"><span data-stu-id="72309-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="72309-1114">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="72309-1115">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1115">No</span></span>          | <span data-ttu-id="72309-1116">**True** (la valeur par défaut) ou **False** selon si la propriété peut avoir une valeur null.</span><span class="sxs-lookup"><span data-stu-id="72309-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="72309-1117">> Dans CSDL v1 doit avoir une propriété de type complexe `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="72309-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="72309-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="72309-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="72309-1119">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1119">No</span></span>          | <span data-ttu-id="72309-1120">Valeur par défaut de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="72309-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="72309-1122">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1122">No</span></span>          | <span data-ttu-id="72309-1123">Longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="72309-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="72309-1125">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1125">No</span></span>          | <span data-ttu-id="72309-1126">**True** ou **False** selon si la valeur de propriété sera stockée en tant que chaîne de longueur fixe.</span><span class="sxs-lookup"><span data-stu-id="72309-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="72309-1127">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-1127">**Precision**</span></span>                                                      | <span data-ttu-id="72309-1128">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1128">No</span></span>          | <span data-ttu-id="72309-1129">Précision de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="72309-1130">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-1130">**Scale**</span></span>                                                          | <span data-ttu-id="72309-1131">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1131">No</span></span>          | <span data-ttu-id="72309-1132">Échelle de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="72309-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-1133">**SRID**</span></span>                                                           | <span data-ttu-id="72309-1134">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1134">No</span></span>          | <span data-ttu-id="72309-1135">Identificateur de référence spatiale système.</span><span class="sxs-lookup"><span data-stu-id="72309-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="72309-1136">Valide uniquement pour les propriétés des types spatiaux.</span><span class="sxs-lookup"><span data-stu-id="72309-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="72309-1137">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="72309-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="72309-1139">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1139">No</span></span>          | <span data-ttu-id="72309-1140">**True** ou **False** selon si la valeur de propriété sera stockée sous forme de chaîne Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="72309-1141">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-1141">**Collation**</span></span>                                                      | <span data-ttu-id="72309-1142">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1142">No</span></span>          | <span data-ttu-id="72309-1143">Chaîne qui spécifie l'ordre de tri à utiliser dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="72309-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="72309-1144">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **CollectionType** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="72309-1145">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1146">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-1147">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1147">Example</span></span>

<span data-ttu-id="72309-1148">L’exemple suivant montre une fonction définie par modèle qui utilise le **TypeRef** élément (en tant qu’enfant d’un **CollectionType** élément) pour spécifier que la fonction accepte une collection de  **Département** types d’entité.</span><span class="sxs-lookup"><span data-stu-id="72309-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="72309-1149">Using, élément (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="72309-1150">Le **Using** élément dans le langage de définition de schéma conceptuel (CSDL) importe le contenu d’un modèle conceptuel qui existe dans un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="72309-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="72309-1151">En définissant la valeur de la **Namespace** attribut, vous pouvez faire référence à des types d’entités, types complexes et les types d’associations sont définies dans un autre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="72309-1152">Plusieurs **Using** élément peut être un enfant d’un **schéma** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-1153">Le **Using** élément dans le langage CSDL ne fonctionne pas exactement comme un **à l’aide de** instruction dans un langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="72309-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="72309-1154">En important un espace de noms avec un **à l’aide de** instruction dans un langage de programmation, vous n’affectez pas les objets dans l’espace de noms d’origine.</span><span class="sxs-lookup"><span data-stu-id="72309-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="72309-1155">Dans le langage CSDL, un espace de noms importé peut contenir un type d'entité dérivé d'un type d'entité figurant dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="72309-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="72309-1156">Cela peut affecter les jeux d'entités déclarés dans l'espace de noms d'origine.</span><span class="sxs-lookup"><span data-stu-id="72309-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="72309-1157">Le **Using** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="72309-1158">Documentation (zéro ou un élément autorisé)</span><span class="sxs-lookup"><span data-stu-id="72309-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="72309-1159">Éléments d’annotation (zéro ou plusieurs éléments autorisés)</span><span class="sxs-lookup"><span data-stu-id="72309-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="72309-1160">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1160">Applicable Attributes</span></span>

<span data-ttu-id="72309-1161">Le tableau ci-dessous décrit les attributs peuvent être appliqués à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="72309-1162">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="72309-1162">Attribute Name</span></span> | <span data-ttu-id="72309-1163">Requis</span><span class="sxs-lookup"><span data-stu-id="72309-1163">Is Required</span></span> | <span data-ttu-id="72309-1164">Value</span><span class="sxs-lookup"><span data-stu-id="72309-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="72309-1165">**Espace de noms**</span><span class="sxs-lookup"><span data-stu-id="72309-1165">**Namespace**</span></span>  | <span data-ttu-id="72309-1166">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1166">Yes</span></span>         | <span data-ttu-id="72309-1167">Nom de l'espace de noms importé.</span><span class="sxs-lookup"><span data-stu-id="72309-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="72309-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="72309-1168">**Alias**</span></span>      | <span data-ttu-id="72309-1169">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1169">Yes</span></span>         | <span data-ttu-id="72309-1170">Identificateur utilisé à la place du nom de l'espace de noms.</span><span class="sxs-lookup"><span data-stu-id="72309-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="72309-1171">Bien que cet attribut soit obligatoire, il n'est pas nécessaire qu'il soit utilisé à la place du nom de l'espace de noms pour qualifier les noms d'objets.</span><span class="sxs-lookup"><span data-stu-id="72309-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="72309-1172">N’importe quel nombre d’attributs d’annotation (attributs XML personnalisés) peut être appliqué à la **Using** élément.</span><span class="sxs-lookup"><span data-stu-id="72309-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="72309-1173">Toutefois, les attributs personnalisés ne peuvent pas appartenir à un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="72309-1174">Les noms qualifiés complets de deux attributs personnalisés quelconques ne peuvent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="72309-1175">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1175">Example</span></span>

<span data-ttu-id="72309-1176">L’exemple suivant montre le **Using** élément utilisé pour importer un espace de noms qui est défini ailleurs.</span><span class="sxs-lookup"><span data-stu-id="72309-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="72309-1177">Notez que l’espace de noms pour le **schéma** élément indiqué est `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="72309-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="72309-1178">Le `Address` propriété sur le `Publisher` **EntityType** est un type complexe qui est défini dans le `ExtendedBooksModel` espace de noms (importé avec le **Using** élément).</span><span class="sxs-lookup"><span data-stu-id="72309-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="72309-1179">Attributs d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="72309-1180">Les attributs d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des attributs XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="72309-1181">En plus d'avoir une structure XML valide, les attributs d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="72309-1182">Les attributs d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="72309-1183">Plusieurs attributs d'annotation peuvent être appliqués à un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="72309-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="72309-1184">Les noms qualifiés complets de deux attributs d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="72309-1185">Les attributs d'annotation peuvent être utilisés pour fournir des métadonnées supplémentaires sur des éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="72309-1186">Métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="72309-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="72309-1187">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1187">Example</span></span>

<span data-ttu-id="72309-1188">L’exemple suivant montre un **EntityType** élément avec un attribut d’annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="72309-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="72309-1189">L'exemple fait également apparaître un élément d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="72309-1190">Le code suivant récupère les métadonnées dans l'attribut d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="72309-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="72309-1191">Le code ci-dessus suppose que le fichier `School.csdl` se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="72309-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="72309-1192">Éléments d'annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="72309-1193">Les éléments d'annotation dans le langage CSDL (Conceptual Schema Definition Language) sont des éléments XML personnalisés dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="72309-1194">En plus d'avoir une structure XML valide, les éléments d'annotation doivent satisfaire les critères suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="72309-1195">Les éléments d'annotation ne doivent pas figurer dans un espace de noms XML réservé pour le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="72309-1196">Plusieurs éléments d'annotation peuvent être des enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="72309-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="72309-1197">Les noms qualifiés complets de deux éléments d'annotation ne doivent pas être identiques.</span><span class="sxs-lookup"><span data-stu-id="72309-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="72309-1198">Les éléments d'annotation doivent apparaître après tous les autres éléments enfants d'un élément CSDL donné.</span><span class="sxs-lookup"><span data-stu-id="72309-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="72309-1199">Les éléments d'annotation permettent de fournir des métadonnées supplémentaires sur les éléments dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="72309-1200">À compter de .NET Framework version 4, les métadonnées contenues dans les éléments d’annotation sont accessibles lors de l’exécution à l’aide des classes dans l’espace de noms System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="72309-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="72309-1201">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1201">Example</span></span>

<span data-ttu-id="72309-1202">L’exemple suivant montre un **EntityType** élément avec un élément d’annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="72309-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="72309-1203">L'exemple fait également apparaître un attribut d'annotation appliqué à l'élément de type d'entité.</span><span class="sxs-lookup"><span data-stu-id="72309-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="72309-1204">Le code suivant récupère les métadonnées dans l'élément d'annotation et les écrit dans la console :</span><span class="sxs-lookup"><span data-stu-id="72309-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="72309-1205">Le code ci-dessus suppose que le fichier School.csdl se trouve dans le répertoire de sortie du projet et que vous avez ajouté les instructions `Imports` et `Using` suivantes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="72309-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="72309-1206">Types de modèles conceptuels (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="72309-1207">Langage de définition de schéma conceptuel (CSDL) prend en charge un ensemble de types de données primitif abstrait, appelé **types Edmsimpletype**, qui définissent des propriétés dans un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="72309-1208">**Types Edmsimpletype** sont des proxys pour les types de données primitifs sont prises en charge dans le stockage ou d’un environnement d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="72309-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="72309-1209">Le tableau suivant répertorie les types de données primitifs pris en charge par CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="72309-1210">Le tableau répertorie également les facettes peuvent être appliquées à chaque **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="72309-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="72309-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="72309-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="72309-1212">Description</span><span class="sxs-lookup"><span data-stu-id="72309-1212">Description</span></span>                                                | <span data-ttu-id="72309-1213">Facettes applicables</span><span class="sxs-lookup"><span data-stu-id="72309-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="72309-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="72309-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="72309-1215">Contient des données binaires.</span><span class="sxs-lookup"><span data-stu-id="72309-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="72309-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="72309-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="72309-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="72309-1218">Contient la valeur **true** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="72309-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="72309-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="72309-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="72309-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="72309-1221">Contient une valeur d'entier 8 bits non signé.</span><span class="sxs-lookup"><span data-stu-id="72309-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="72309-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="72309-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="72309-1224">Représente une date et une heure.</span><span class="sxs-lookup"><span data-stu-id="72309-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="72309-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="72309-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="72309-1227">Contient une date et une heure en tant que décalage en minutes par rapport à l'heure GMT.</span><span class="sxs-lookup"><span data-stu-id="72309-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="72309-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="72309-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="72309-1230">Contient une valeur numérique avec une précision et une échelle fixes.</span><span class="sxs-lookup"><span data-stu-id="72309-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="72309-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="72309-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="72309-1233">Contient un nombre avec une précision de 15 chiffres à virgule flottante</span><span class="sxs-lookup"><span data-stu-id="72309-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="72309-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="72309-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="72309-1236">Contient un nombre à virgule flottante avec une précision de 7 chiffres.</span><span class="sxs-lookup"><span data-stu-id="72309-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="72309-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="72309-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="72309-1239">Contient un identificateur unique de 16 octets.</span><span class="sxs-lookup"><span data-stu-id="72309-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="72309-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="72309-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="72309-1242">Contient une valeur d'entier 16 bits signé.</span><span class="sxs-lookup"><span data-stu-id="72309-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="72309-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="72309-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="72309-1245">Contient une valeur d'entier 32 bits signé.</span><span class="sxs-lookup"><span data-stu-id="72309-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="72309-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="72309-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="72309-1248">Contient une valeur d'entier 64 bits signé.</span><span class="sxs-lookup"><span data-stu-id="72309-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="72309-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="72309-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="72309-1251">Contient une valeur d'entier 8 bits signé.</span><span class="sxs-lookup"><span data-stu-id="72309-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="72309-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="72309-1253">**Edm.String**</span></span>                   | <span data-ttu-id="72309-1254">Contient des données caractères.</span><span class="sxs-lookup"><span data-stu-id="72309-1254">Contains character data.</span></span>                                   | <span data-ttu-id="72309-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="72309-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="72309-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="72309-1257">Contient une heure.</span><span class="sxs-lookup"><span data-stu-id="72309-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="72309-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="72309-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="72309-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="72309-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="72309-1260">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="72309-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="72309-1262">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="72309-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="72309-1264">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="72309-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="72309-1266">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="72309-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="72309-1268">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="72309-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="72309-1270">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="72309-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="72309-1272">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="72309-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="72309-1274">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="72309-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="72309-1276">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="72309-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="72309-1278">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="72309-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="72309-1280">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="72309-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="72309-1282">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="72309-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="72309-1284">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="72309-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="72309-1286">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="72309-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="72309-1288">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="72309-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="72309-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="72309-1290">Nullable, par défaut, le SRID</span><span class="sxs-lookup"><span data-stu-id="72309-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="72309-1291">Facettes (CSDL)</span><span class="sxs-lookup"><span data-stu-id="72309-1291">Facets (CSDL)</span></span>

<span data-ttu-id="72309-1292">Les facettes dans le langage CSDL (Conceptual Schema Definition Language) représentent des contraintes sur les propriétés de types d'entités et de types complexes.</span><span class="sxs-lookup"><span data-stu-id="72309-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="72309-1293">Les facettes apparaissent comme des attributs XML sur les éléments CSDL suivants :</span><span class="sxs-lookup"><span data-stu-id="72309-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="72309-1294">Propriété</span><span class="sxs-lookup"><span data-stu-id="72309-1294">Property</span></span>
-   <span data-ttu-id="72309-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="72309-1295">TypeRef</span></span>
-   <span data-ttu-id="72309-1296">Paramètre</span><span class="sxs-lookup"><span data-stu-id="72309-1296">Parameter</span></span>

<span data-ttu-id="72309-1297">Le tableau ci-dessous décrit les facettes prises en charge dans le langage CSDL.</span><span class="sxs-lookup"><span data-stu-id="72309-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="72309-1298">Toutes les facettes sont facultatives.</span><span class="sxs-lookup"><span data-stu-id="72309-1298">All facets are optional.</span></span> <span data-ttu-id="72309-1299">Certaines facettes répertoriées ci-dessous sont utilisées par Entity Framework lors de la génération d’une base de données à partir d’un modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="72309-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="72309-1300">Pour plus d’informations sur les types de données dans un modèle conceptuel, consultez Types de modèle conceptuel (CSDL).</span><span class="sxs-lookup"><span data-stu-id="72309-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="72309-1301">Facette</span><span class="sxs-lookup"><span data-stu-id="72309-1301">Facet</span></span>               | <span data-ttu-id="72309-1302">Description</span><span class="sxs-lookup"><span data-stu-id="72309-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="72309-1303">S'applique à</span><span class="sxs-lookup"><span data-stu-id="72309-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="72309-1304">Utilisée pour la génération de base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-1304">Used for the database generation</span></span> | <span data-ttu-id="72309-1305">Utilisée par le runtime.</span><span class="sxs-lookup"><span data-stu-id="72309-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="72309-1306">**classement**</span><span class="sxs-lookup"><span data-stu-id="72309-1306">**Collation**</span></span>       | <span data-ttu-id="72309-1307">Spécifie la table de classement ou ordre de tri à utiliser lors de l'exécution d'opérations de comparaison et de tri sur des valeurs de la propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="72309-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="72309-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="72309-1309">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1309">Yes</span></span>                              | <span data-ttu-id="72309-1310">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1310">No</span></span>                  |
| <span data-ttu-id="72309-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="72309-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="72309-1312">Indique que la valeur de la propriété doit être utilisée pour des contrôles d'accès concurrentiel optimiste.</span><span class="sxs-lookup"><span data-stu-id="72309-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="72309-1313">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="72309-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="72309-1314">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1314">No</span></span>                               | <span data-ttu-id="72309-1315">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1315">Yes</span></span>                 |
| <span data-ttu-id="72309-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="72309-1316">**Default**</span></span>         | <span data-ttu-id="72309-1317">Spécifie la valeur par défaut de la propriété si aucune valeur n'est fournie en cas d'instanciation.</span><span class="sxs-lookup"><span data-stu-id="72309-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="72309-1318">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="72309-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="72309-1319">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1319">Yes</span></span>                              | <span data-ttu-id="72309-1320">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1320">Yes</span></span>                 |
| <span data-ttu-id="72309-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="72309-1321">**FixedLength**</span></span>     | <span data-ttu-id="72309-1322">Spécifie si la longueur de la valeur de propriété peut varier.</span><span class="sxs-lookup"><span data-stu-id="72309-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="72309-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="72309-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="72309-1324">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1324">Yes</span></span>                              | <span data-ttu-id="72309-1325">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1325">No</span></span>                  |
| <span data-ttu-id="72309-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="72309-1326">**MaxLength**</span></span>       | <span data-ttu-id="72309-1327">Spécifie la longueur maximale de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="72309-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="72309-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="72309-1329">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1329">Yes</span></span>                              | <span data-ttu-id="72309-1330">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1330">No</span></span>                  |
| <span data-ttu-id="72309-1331">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="72309-1331">**Nullable**</span></span>        | <span data-ttu-id="72309-1332">Spécifie si la propriété peut avoir un **null** valeur.</span><span class="sxs-lookup"><span data-stu-id="72309-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="72309-1333">Tous les **EDMSimpleType** propriétés</span><span class="sxs-lookup"><span data-stu-id="72309-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="72309-1334">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1334">Yes</span></span>                              | <span data-ttu-id="72309-1335">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1335">Yes</span></span>                 |
| <span data-ttu-id="72309-1336">**Précision**</span><span class="sxs-lookup"><span data-stu-id="72309-1336">**Precision**</span></span>       | <span data-ttu-id="72309-1337">Pour les propriétés de type **décimal**, spécifie le nombre de chiffres d’une valeur de propriété peut avoir.</span><span class="sxs-lookup"><span data-stu-id="72309-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="72309-1338">Pour les propriétés de type **temps**, **DateTime**, et **DateTimeOffset**, spécifie le nombre de chiffres pour la partie fractionnaire des secondes de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="72309-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="72309-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="72309-1340">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1340">Yes</span></span>                              | <span data-ttu-id="72309-1341">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1341">No</span></span>                  |
| <span data-ttu-id="72309-1342">**Échelle**</span><span class="sxs-lookup"><span data-stu-id="72309-1342">**Scale**</span></span>           | <span data-ttu-id="72309-1343">Spécifie le nombre de chiffres à droite de la virgule décimale pour la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="72309-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="72309-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="72309-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="72309-1345">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1345">Yes</span></span>                              | <span data-ttu-id="72309-1346">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1346">No</span></span>                  |
| <span data-ttu-id="72309-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="72309-1347">**SRID**</span></span>            | <span data-ttu-id="72309-1348">Spécifie l’ID système Spatial référence système.</span><span class="sxs-lookup"><span data-stu-id="72309-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="72309-1349">Pour plus d’informations, consultez [SRID](http://en.wikipedia.org/wiki/SRID) et [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="72309-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="72309-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="72309-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="72309-1351">Non</span><span class="sxs-lookup"><span data-stu-id="72309-1351">No</span></span>                               | <span data-ttu-id="72309-1352">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1352">Yes</span></span>                 |
| <span data-ttu-id="72309-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="72309-1353">**Unicode**</span></span>         | <span data-ttu-id="72309-1354">Indique si la valeur de propriété est stockée au format Unicode.</span><span class="sxs-lookup"><span data-stu-id="72309-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="72309-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="72309-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="72309-1356">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1356">Yes</span></span>                              | <span data-ttu-id="72309-1357">Oui</span><span class="sxs-lookup"><span data-stu-id="72309-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="72309-1358">Lors de la génération d’une base de données à partir d’un modèle conceptuel, l’Assistant génération de base de données reconnaîtra la valeur de la **StoreGeneratedPattern** attribut sur un **propriété** élément si elle est la suivante espace de noms : http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="72309-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="72309-1359">Les valeurs prises en charge pour l’attribut sont **identité** et **calculé**.</span><span class="sxs-lookup"><span data-stu-id="72309-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="72309-1360">La valeur **identité** produira une colonne de base de données avec une valeur d’identité générée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="72309-1361">La valeur **calculé** produira une colonne avec une valeur qui est calculée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="72309-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="72309-1362">Exemple</span><span class="sxs-lookup"><span data-stu-id="72309-1362">Example</span></span>

<span data-ttu-id="72309-1363">L'exemple suivant illustre l'application de facettes aux propriétés d'un type d'entité :</span><span class="sxs-lookup"><span data-stu-id="72309-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
