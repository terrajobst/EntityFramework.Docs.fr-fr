---
title: Spécification MSL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121163"
---
# <a name="msl-specification"></a><span data-ttu-id="2023a-102">Spécification MSL</span><span class="sxs-lookup"><span data-stu-id="2023a-102">MSL Specification</span></span>
<span data-ttu-id="2023a-103">Langage de spécification de mappage (MSL) est un langage basé sur XML qui décrit le mappage entre le modèle conceptuel et le modèle de stockage d’une application Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2023a-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="2023a-104">Dans une application Entity Framework, les métadonnées de mappage sont chargée à partir d’un fichier .msl (écrit en MSL) au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="2023a-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="2023a-105">Entity Framework utilise les métadonnées de mappage lors de l’exécution pour traduire des requêtes sur le modèle conceptuel en commandes spécifiques au magasin.</span><span class="sxs-lookup"><span data-stu-id="2023a-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="2023a-106">Entity Framework Designer (Concepteur d’EF) stocke des informations de mappage dans un fichier .edmx au moment du design.</span><span class="sxs-lookup"><span data-stu-id="2023a-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="2023a-107">Au moment de la génération, le Concepteur d’entités utilise les informations dans un fichier .edmx pour créer le fichier .msl qui est nécessaire par Entity Framework lors de l’exécution</span><span class="sxs-lookup"><span data-stu-id="2023a-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="2023a-108">Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="2023a-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="2023a-109">Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez [spécification CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="2023a-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="2023a-110">Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="2023a-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="2023a-111">Les versions de code MSL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="2023a-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="2023a-112">Version MSL</span><span class="sxs-lookup"><span data-stu-id="2023a-112">MSL Version</span></span> | <span data-ttu-id="2023a-113">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="2023a-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="2023a-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="2023a-114">MSL v1</span></span>      | <span data-ttu-id="2023a-115">urn : schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="2023a-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="2023a-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="2023a-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="2023a-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="2023a-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="2023a-118">Élément Alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-118">Alias Element (MSL)</span></span>

<span data-ttu-id="2023a-119">Le **Alias** élément dans la spécification du langage MSL (mapping) est un enfant de l’élément de mappage qui est utilisé pour définir des alias pour des noms de modèle de stockage et le modèle conceptuels.</span><span class="sxs-lookup"><span data-stu-id="2023a-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="2023a-120">Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="2023a-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="2023a-121">Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez l’élément Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="2023a-122">Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez l’élément de schéma (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="2023a-123">Le **Alias** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="2023a-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-124">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-124">Applicable Attributes</span></span>

<span data-ttu-id="2023a-125">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **Alias** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="2023a-126">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-126">Attribute Name</span></span> | <span data-ttu-id="2023a-127">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-127">Is Required</span></span> | <span data-ttu-id="2023a-128">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="2023a-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="2023a-129">**Key**</span></span>        | <span data-ttu-id="2023a-130">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-130">Yes</span></span>         | <span data-ttu-id="2023a-131">L’alias pour l’espace de noms spécifié par le **valeur** attribut.</span><span class="sxs-lookup"><span data-stu-id="2023a-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="2023a-132">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="2023a-132">**Value**</span></span>      | <span data-ttu-id="2023a-133">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-133">Yes</span></span>         | <span data-ttu-id="2023a-134">L’espace de noms pour lequel la valeur de la **clé** élément est un alias.</span><span class="sxs-lookup"><span data-stu-id="2023a-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="2023a-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-135">Example</span></span>

<span data-ttu-id="2023a-136">L’exemple suivant montre un **Alias** élément qui définit un alias, `c`, pour les types qui sont définis dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="2023a-137">AssociationEnd, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="2023a-138">Le **AssociationEnd** élément dans la spécification du langage MSL (mapping) est utilisé lorsque les fonctions de modification d’un type d’entité dans le modèle conceptuel sont mappées aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="2023a-139">Si une modification de procédure stockée accepte un paramètre dont la valeur est conservée dans une propriété d’association, le **AssociationEnd** élément est mappé à la valeur de propriété pour le paramètre.</span><span class="sxs-lookup"><span data-stu-id="2023a-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="2023a-140">Pour plus d'informations, voir l'exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2023a-140">For more information, see the example below.</span></span>

<span data-ttu-id="2023a-141">Pour plus d’informations sur le mappage des fonctions de modification de types d’entités à des procédures stockées, consultez ModificationFunctionMapping élément (MSL) et procédure pas à pas : mappage d’une entité aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="2023a-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="2023a-142">Le **AssociationEnd** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-144">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-144">Applicable Attributes</span></span>

<span data-ttu-id="2023a-145">Le tableau suivant décrit les attributs qui s’appliquent à la **AssociationEnd** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="2023a-146">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-146">Attribute Name</span></span>     | <span data-ttu-id="2023a-147">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-147">Is Required</span></span> | <span data-ttu-id="2023a-148">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="2023a-149">**AssociationSet**</span></span> | <span data-ttu-id="2023a-150">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-150">Yes</span></span>         | <span data-ttu-id="2023a-151">Nom de l'association mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="2023a-152">**From**</span><span class="sxs-lookup"><span data-stu-id="2023a-152">**From**</span></span>           | <span data-ttu-id="2023a-153">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-153">Yes</span></span>         | <span data-ttu-id="2023a-154">La valeur de la **FromRole** attribut de la propriété de navigation qui correspond à l’association mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="2023a-155">Pour plus d’informations, consultez l’élément NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="2023a-156">**To**</span><span class="sxs-lookup"><span data-stu-id="2023a-156">**To**</span></span>             | <span data-ttu-id="2023a-157">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-157">Yes</span></span>         | <span data-ttu-id="2023a-158">La valeur de la **ToRole** attribut de la propriété de navigation qui correspond à l’association mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="2023a-159">Pour plus d’informations, consultez l’élément NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="2023a-160">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-160">Example</span></span>

<span data-ttu-id="2023a-161">Considérons le type d'entité de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="2023a-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="2023a-162">Considérons également la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="2023a-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="2023a-163">Pour mapper la fonction de mise à jour de la `Course` entité à cette procédure stockée, vous devez fournir une valeur pour le **DepartmentID** paramètre.</span><span class="sxs-lookup"><span data-stu-id="2023a-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="2023a-164">La valeur pour `DepartmentID` ne correspond pas à une propriété sur le type d'entité ; elle est contenue dans une association indépendante dont le mappage est indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="2023a-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="2023a-165">Le code suivant illustre la **AssociationEnd** élément utilisé pour mapper le **DepartmentID** propriété de la **FK\_cours\_département** association à la **UpdateCourse** procédure stockée (dans lequel la fonction de mise à jour de la **cours** type d’entité est mappé) :</span><span class="sxs-lookup"><span data-stu-id="2023a-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="2023a-166">AssociationSetMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-167">Le **AssociationSetMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une association dans les colonnes de table et au modèle conceptuels dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="2023a-168">Les associations dans le modèle conceptuel sont des types dont les propriétés représentent des colonnes de clé primaire et de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="2023a-169">Le **AssociationSetMapping** élément utilise deux éléments EndProperty pour définir les mappages entre les propriétés de type d’association et les colonnes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="2023a-170">Vous pouvez placer des conditions sur ces mappages avec l’élément Condition.</span><span class="sxs-lookup"><span data-stu-id="2023a-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="2023a-171">Mapper l’insert, update et les fonctions de suppression pour les associations à des procédures stockées dans la base de données avec l’élément ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="2023a-172">Définir des mappages en lecture seule entre les associations et les colonnes de table à l’aide d’une chaîne de Entity SQL dans un QueryView (élément).</span><span class="sxs-lookup"><span data-stu-id="2023a-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-173">Si une contrainte référentielle est définie pour une association dans le modèle conceptuel, l’association ne doit-elle pas être mappées avec un **AssociationSetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="2023a-174">Si un **AssociationSetMapping** élément est présent pour une association qui a une contrainte référentielle, les mappages définis dans le **AssociationSetMapping** élément sera ignoré.</span><span class="sxs-lookup"><span data-stu-id="2023a-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="2023a-175">Pour plus d’informations, consultez élément ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="2023a-176">Le **AssociationSetMapping** élément peut avoir les éléments enfants suivants</span><span class="sxs-lookup"><span data-stu-id="2023a-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="2023a-177">QueryView (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="2023a-178">EndProperty (zéro ou deux)</span><span class="sxs-lookup"><span data-stu-id="2023a-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="2023a-179">Condition (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="2023a-180">ModificationFunctionMapping (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-181">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-181">Applicable Attributes</span></span>

<span data-ttu-id="2023a-182">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **AssociationSetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="2023a-183">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-183">Attribute Name</span></span>     | <span data-ttu-id="2023a-184">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-184">Is Required</span></span> | <span data-ttu-id="2023a-185">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-186">**Name**</span></span>           | <span data-ttu-id="2023a-187">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-187">Yes</span></span>         | <span data-ttu-id="2023a-188">Nom de l'ensemble d'associations du modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="2023a-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="2023a-189">**TypeName**</span></span>       | <span data-ttu-id="2023a-190">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-190">No</span></span>          | <span data-ttu-id="2023a-191">Nom qualifié par un espace de noms du type d'association du modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="2023a-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="2023a-192">**StoreEntitySet**</span></span> | <span data-ttu-id="2023a-193">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-193">No</span></span>          | <span data-ttu-id="2023a-194">Nom de la table mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="2023a-195">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-195">Example</span></span>

<span data-ttu-id="2023a-196">L’exemple suivant montre un **AssociationSetMapping** élément dans lequel le **FK\_cours\_département** ensemble d’associations dans le modèle conceptuel sont mappée à la  **Cours** table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="2023a-197">Mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans l’enfant **EndProperty** éléments.</span><span class="sxs-lookup"><span data-stu-id="2023a-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="2023a-198">ComplexProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="2023a-199">Un **ComplexProperty** élément dans la spécification du langage MSL (mapping) définit le mappage entre une propriété de type complexe sur un modèle conceptuel entity type et la table colonnes dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="2023a-200">Les mappages de colonnes de propriété sont spécifiés dans les éléments ScalarProperty enfants.</span><span class="sxs-lookup"><span data-stu-id="2023a-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="2023a-201">Le **ComplexType** élément de propriété peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-202">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="2023a-203">**ComplexProperty** (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="2023a-204">ComplextTypeMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="2023a-205">Condition (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-206">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-206">Applicable Attributes</span></span>

<span data-ttu-id="2023a-207">Le tableau suivant décrit les attributs qui s’appliquent à la **ComplexProperty** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="2023a-208">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-208">Attribute Name</span></span> | <span data-ttu-id="2023a-209">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-209">Is Required</span></span> | <span data-ttu-id="2023a-210">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-211">**Name**</span></span>       | <span data-ttu-id="2023a-212">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-212">Yes</span></span>         | <span data-ttu-id="2023a-213">Nom de la propriété complexe d'un type d'entité dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="2023a-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="2023a-214">**TypeName**</span></span>   | <span data-ttu-id="2023a-215">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-215">No</span></span>          | <span data-ttu-id="2023a-216">Nom qualifié par un espace de noms du type de propriété de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="2023a-217">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-217">Example</span></span>

<span data-ttu-id="2023a-218">L’exemple suivant est basé sur le modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-218">The following example is based on the School model.</span></span> <span data-ttu-id="2023a-219">Le type complexe suivant a été ajouté au modèle conceptuel :</span><span class="sxs-lookup"><span data-stu-id="2023a-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="2023a-220">Le **LastName** et **FirstName** propriétés de la **personne** type d’entité ont été remplacées par une propriété complexe, **nom**:</span><span class="sxs-lookup"><span data-stu-id="2023a-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="2023a-221">Le MSL suivant indique le **ComplexProperty** élément utilisé pour mapper le **nom** propriété aux colonnes dans la base de données sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="2023a-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="2023a-222">ComplexTypeMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-223">Le **ComplexTypeMapping** élément dans la spécification du langage MSL (mapping) est un enfant de l’élément ResultMapping et définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans sous-jacent base de données lorsque les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="2023a-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="2023a-224">L'importation de fonction retourne un type complexe conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="2023a-225">Les noms de colonne retournés par la procédure stockée ne correspondent pas exactement aux noms de propriété du type complexe.</span><span class="sxs-lookup"><span data-stu-id="2023a-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="2023a-226">Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type complexe est basé sur les noms de colonne et de propriété.</span><span class="sxs-lookup"><span data-stu-id="2023a-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="2023a-227">Si les noms de colonnes ne correspondent pas exactement les noms de propriété, vous devez utiliser le **ComplexTypeMapping** élément pour définir le mappage.</span><span class="sxs-lookup"><span data-stu-id="2023a-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="2023a-228">Pour obtenir un exemple du mappage par défaut, consultez l’élément FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="2023a-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="2023a-229">Le **ComplexTypeMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-230">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-231">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-231">Applicable Attributes</span></span>

<span data-ttu-id="2023a-232">Le tableau suivant décrit les attributs qui s’appliquent à la **ComplexTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="2023a-233">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-233">Attribute Name</span></span> | <span data-ttu-id="2023a-234">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-234">Is Required</span></span> | <span data-ttu-id="2023a-235">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="2023a-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="2023a-236">**TypeName**</span></span>   | <span data-ttu-id="2023a-237">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-237">Yes</span></span>         | <span data-ttu-id="2023a-238">Nom qualifié par un espace de noms du type complexe mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-239">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-239">Example</span></span>

<span data-ttu-id="2023a-240">Considérons la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="2023a-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="2023a-241">De même, considérons le type complexe de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="2023a-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="2023a-242">Pour créer une importation de fonction qui retourne des instances du type complexe précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un **ComplexTypeMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="2023a-243">Condition, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-243">Condition Element (MSL)</span></span>

<span data-ttu-id="2023a-244">Le **Condition** élément dans la spécification du langage MSL (mapping) impose des conditions sur les mappages entre le modèle conceptuel et la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="2023a-245">Le mappage est défini dans un nœud XML est valides si toutes les conditions, en tant qu’enfant spécifié dans **Condition** éléments, sont remplies.</span><span class="sxs-lookup"><span data-stu-id="2023a-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="2023a-246">À défaut, le mappage n'est pas valide.</span><span class="sxs-lookup"><span data-stu-id="2023a-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="2023a-247">Par exemple, si un élément MappingFragment contient un ou plusieurs **Condition** éléments enfants, le mappage défini dans le **MappingFragment** nœud n’est valide que si toutes les conditions de l’enfant  **Condition** éléments sont remplies.</span><span class="sxs-lookup"><span data-stu-id="2023a-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="2023a-248">Chaque condition peut s’appliquer à un un **nom** (le nom d’une propriété d’entité de modèle conceptuel, spécifié par le **nom** attribut), ou un **ColumnName** (le nom d’une colonne dans la base de données spécifiée par le **ColumnName** attribut).</span><span class="sxs-lookup"><span data-stu-id="2023a-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="2023a-249">Lorsque le **nom** attribut est défini, la condition est vérifiée par rapport à une valeur de propriété d’entité.</span><span class="sxs-lookup"><span data-stu-id="2023a-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="2023a-250">Lorsque le **ColumnName** attribut est défini, la condition est vérifiée par rapport à une valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="2023a-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="2023a-251">Une des seules la **nom** ou **ColumnName** attribut peut être spécifié dans un **Condition** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-252">Lorsque le **Condition** élément est utilisé dans un élément FunctionImportMapping uniquement le **nom** attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="2023a-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="2023a-253">Le **Condition** élément peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="2023a-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="2023a-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-255">ComplexProperty</span></span>
-   <span data-ttu-id="2023a-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-256">EntitySetMapping</span></span>
-   <span data-ttu-id="2023a-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="2023a-257">MappingFragment</span></span>
-   <span data-ttu-id="2023a-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-258">EntityTypeMapping</span></span>

<span data-ttu-id="2023a-259">Le **Condition** élément ne peut avoir aucun élément enfant.</span><span class="sxs-lookup"><span data-stu-id="2023a-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-260">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-260">Applicable Attributes</span></span>

<span data-ttu-id="2023a-261">Le tableau suivant décrit les attributs qui s’appliquent à la **Condition** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="2023a-262">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-262">Attribute Name</span></span> | <span data-ttu-id="2023a-263">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-263">Is Required</span></span> | <span data-ttu-id="2023a-264">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="2023a-265">**ColumnName**</span></span> | <span data-ttu-id="2023a-266">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-266">No</span></span>          | <span data-ttu-id="2023a-267">Nom de la colonne de table dont la valeur est utilisée pour évaluer la condition.</span><span class="sxs-lookup"><span data-stu-id="2023a-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="2023a-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="2023a-268">**IsNull**</span></span>     | <span data-ttu-id="2023a-269">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-269">No</span></span>          | <span data-ttu-id="2023a-270">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="2023a-270">**True** or **False**.</span></span> <span data-ttu-id="2023a-271">Si la valeur est **True** et la valeur de colonne est **null**, ou si la valeur est **False** et la valeur de colonne n’est pas **null**, la condition est vraie .</span><span class="sxs-lookup"><span data-stu-id="2023a-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="2023a-272">Sinon, la condition n'est pas vérifiée (False).</span><span class="sxs-lookup"><span data-stu-id="2023a-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="2023a-273">Le **IsNull** et **valeur** les attributs ne peuvent pas être utilisés en même temps.</span><span class="sxs-lookup"><span data-stu-id="2023a-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="2023a-274">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="2023a-274">**Value**</span></span>      | <span data-ttu-id="2023a-275">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-275">No</span></span>          | <span data-ttu-id="2023a-276">Valeur à laquelle la valeur de colonne est comparée.</span><span class="sxs-lookup"><span data-stu-id="2023a-276">The value with which the column value is compared.</span></span> <span data-ttu-id="2023a-277">Si les valeurs sont identiques, la condition est vérifiée (True).</span><span class="sxs-lookup"><span data-stu-id="2023a-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="2023a-278">Sinon, la condition n'est pas vérifiée (False).</span><span class="sxs-lookup"><span data-stu-id="2023a-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="2023a-279">Le **IsNull** et **valeur** les attributs ne peuvent pas être utilisés en même temps.</span><span class="sxs-lookup"><span data-stu-id="2023a-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="2023a-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-280">**Name**</span></span>       | <span data-ttu-id="2023a-281">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-281">No</span></span>          | <span data-ttu-id="2023a-282">Nom de la propriété d'entité de modèle conceptuel dont la valeur est utilisée pour évaluer la condition.</span><span class="sxs-lookup"><span data-stu-id="2023a-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="2023a-283">Cet attribut n’est pas applicable si la **Condition** élément est utilisé dans un élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="2023a-284">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-284">Example</span></span>

<span data-ttu-id="2023a-285">L’exemple suivant **Condition** éléments en tant qu’enfants de **MappingFragment** éléments.</span><span class="sxs-lookup"><span data-stu-id="2023a-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="2023a-286">Lorsque **HireDate** n’est pas null et **EnrollmentDate** est null, les données sont mappées entre le **SchoolModel.Instructor** type et le **PersonID**et **HireDate** colonnes de la **personne** table.</span><span class="sxs-lookup"><span data-stu-id="2023a-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="2023a-287">Lorsque **EnrollmentDate** n’est pas null et **HireDate** est null, les données sont mappées entre le **SchoolModel.Student** type et le **PersonID** et **inscription** colonnes de la **personne** table.</span><span class="sxs-lookup"><span data-stu-id="2023a-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="2023a-288">DeleteFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="2023a-289">Le **DeleteFunction** élément dans la spécification du langage MSL (mapping) est mappé à la fonction de suppression d’un type d’entité ou une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="2023a-290">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="2023a-291">Pour plus d’informations, consultez la fonction élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-292">Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="2023a-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="2023a-293">Application de DeleteFunction à EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="2023a-294">Lorsqu’il est appliqué à l’élément EntityTypeMapping, le **DeleteFunction** élément est mappé à la fonction de suppression d’un type d’entité dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="2023a-295">Le **DeleteFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à un **EntityTypeMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="2023a-296">AssociationEnd (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="2023a-297">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="2023a-298">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2023a-299">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-299">Applicable Attributes</span></span>

<span data-ttu-id="2023a-300">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **DeleteFunction** élément lorsqu’il est appliqué à un **EntityTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="2023a-301">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-301">Attribute Name</span></span>            | <span data-ttu-id="2023a-302">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-302">Is Required</span></span> | <span data-ttu-id="2023a-303">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-304">**FunctionName**</span></span>          | <span data-ttu-id="2023a-305">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-305">Yes</span></span>         | <span data-ttu-id="2023a-306">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="2023a-307">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="2023a-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="2023a-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="2023a-309">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-309">No</span></span>          | <span data-ttu-id="2023a-310">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="2023a-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="2023a-311">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-311">Example</span></span>

<span data-ttu-id="2023a-312">L’exemple suivant est basé sur le modèle School et montre la **DeleteFunction** élément de mappage de la fonction de suppression de la **personne** type d’entité à la **DeletePerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="2023a-313">Le **DeletePerson** procédure stockée est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="2023a-314">Application de DeleteFunction à AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="2023a-315">Lorsqu’il est appliqué à l’élément AssociationSetMapping, le **DeleteFunction** élément est mappé à la fonction de suppression d’une association dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="2023a-316">Le **DeleteFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à la **AssociationSetMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="2023a-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2023a-318">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-318">Applicable Attributes</span></span>

<span data-ttu-id="2023a-319">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **DeleteFunction** élément lorsqu’il est appliqué à la **AssociationSetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="2023a-320">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-320">Attribute Name</span></span>            | <span data-ttu-id="2023a-321">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-321">Is Required</span></span> | <span data-ttu-id="2023a-322">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-323">**FunctionName**</span></span>          | <span data-ttu-id="2023a-324">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-324">Yes</span></span>         | <span data-ttu-id="2023a-325">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="2023a-326">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="2023a-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="2023a-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="2023a-328">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-328">No</span></span>          | <span data-ttu-id="2023a-329">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="2023a-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="2023a-330">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-330">Example</span></span>

<span data-ttu-id="2023a-331">L’exemple suivant est basé sur le modèle School et montre la **DeleteFunction** élément utilisé pour mapper la fonction de suppression de la **CourseInstructor** association à la  **DeleteCourseInstructor** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="2023a-332">Le **DeleteCourseInstructor** procédure stockée est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="2023a-333">EndProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="2023a-334">Le **EndProperty** élément dans la spécification du langage MSL (mapping) définit le mappage entre une terminaison ou une fonction de modification d’une association de modèle conceptuel et la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="2023a-335">Le mappage de colonne de la propriété est spécifié dans un élément de ScalarProperty enfant.</span><span class="sxs-lookup"><span data-stu-id="2023a-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="2023a-336">Quand un **EndProperty** élément est utilisé pour définir le mappage pour la fin d’une association de modèle conceptuel, c’est un enfant d’un élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="2023a-337">Lorsque le **EndProperty** élément est utilisé pour définir le mappage d’une fonction de modification d’une association de modèle conceptuel, c’est un enfant d’un élément InsertFunction ou d’élément DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="2023a-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="2023a-338">Le **EndProperty** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-339">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-340">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-340">Applicable Attributes</span></span>

<span data-ttu-id="2023a-341">Le tableau suivant décrit les attributs qui s’appliquent à la **EndProperty** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="2023a-342">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-342">Attribute Name</span></span> | <span data-ttu-id="2023a-343">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-343">Is Required</span></span> | <span data-ttu-id="2023a-344">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="2023a-345">Name</span><span class="sxs-lookup"><span data-stu-id="2023a-345">Name</span></span>           | <span data-ttu-id="2023a-346">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-346">Yes</span></span>         | <span data-ttu-id="2023a-347">Nom de la terminaison d'association mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-348">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-348">Example</span></span>

<span data-ttu-id="2023a-349">L’exemple suivant montre un **AssociationSetMapping** élément dans lequel le **FK\_cours\_département** association dans le modèle conceptuel est mappée à la **Cours** table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="2023a-350">Mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans l’enfant **EndProperty** éléments.</span><span class="sxs-lookup"><span data-stu-id="2023a-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-351">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-351">Example</span></span>

<span data-ttu-id="2023a-352">L’exemple suivant montre le **EndProperty** élément les fonctions insert et delete d’une association de mappage (**CourseInstructor**) aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="2023a-353">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="2023a-354">EntityContainerMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-355">Le **EntityContainerMapping** élément dans la spécification du langage MSL (mapping) mappe le conteneur d’entités dans le modèle conceptuel au conteneur d’entités du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="2023a-356">Le **EntityContainerMapping** élément est un enfant de l’élément de mappage.</span><span class="sxs-lookup"><span data-stu-id="2023a-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="2023a-357">Le **EntityContainerMapping** élément peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="2023a-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2023a-358">EntitySetMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="2023a-359">AssociationSetMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="2023a-360">FunctionImportMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-361">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-361">Applicable Attributes</span></span>

<span data-ttu-id="2023a-362">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntityContainerMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="2023a-363">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-363">Attribute Name</span></span>            | <span data-ttu-id="2023a-364">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-364">Is Required</span></span> | <span data-ttu-id="2023a-365">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="2023a-366">**StorageModelContainer**</span></span> | <span data-ttu-id="2023a-367">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-367">Yes</span></span>         | <span data-ttu-id="2023a-368">Nom du conteneur d'entités de modèle de stockage mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="2023a-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="2023a-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="2023a-370">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-370">Yes</span></span>         | <span data-ttu-id="2023a-371">Nom du conteneur d'entités de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="2023a-372">**Generateupdateviews comme**</span><span class="sxs-lookup"><span data-stu-id="2023a-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="2023a-373">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-373">No</span></span>          | <span data-ttu-id="2023a-374">**True** ou **False**.</span><span class="sxs-lookup"><span data-stu-id="2023a-374">**True** or **False**.</span></span> <span data-ttu-id="2023a-375">Si **False**, aucune vue de la mise à jour n’est générés.</span><span class="sxs-lookup"><span data-stu-id="2023a-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="2023a-376">Cet attribut doit être défini **False** lorsque vous avez un mappage en lecture seule qui n’est pas valide, car les données ne peuvent pas effectuer un aller-retour avec succès.</span><span class="sxs-lookup"><span data-stu-id="2023a-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="2023a-377">La valeur par défaut est **True**.</span><span class="sxs-lookup"><span data-stu-id="2023a-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-378">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-378">Example</span></span>

<span data-ttu-id="2023a-379">L’exemple suivant montre un **EntityContainerMapping** élément qui mappe le **SchoolModelEntities** container (conteneur d’entités du modèle conceptuel) à la  **SchoolModelStoreContainer** conteneur (le modèle entité conteneur de stockage) :</span><span class="sxs-lookup"><span data-stu-id="2023a-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="2023a-380">EntitySetMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-381">Le **EntitySetMapping** élément dans les mappages de langage MSL (Mapping) spécification tous les types dans une entité de modèle conceptuel est définis à l’entité de mappage définit le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="2023a-382">Une jeu d’entités dans le modèle conceptuel sont un conteneur logique pour les instances d’entités du même type (et les types dérivés).</span><span class="sxs-lookup"><span data-stu-id="2023a-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="2023a-383">Une jeu d’entités dans le modèle de stockage représente une table ou vue dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="2023a-384">Le jeu d’entités de modèle conceptuel est spécifié par la valeur de la **nom** attribut de la **EntitySetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="2023a-385">La mappée vers une table ou la vue est spécifié par le **StoreEntitySet** attribut dans chaque élément de MappingFragment enfant ou dans le **EntitySetMapping** élément lui-même.</span><span class="sxs-lookup"><span data-stu-id="2023a-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="2023a-386">Le **EntitySetMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-387">EntityTypeMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="2023a-388">QueryView (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="2023a-389">MappingFragment (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-390">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-390">Applicable Attributes</span></span>

<span data-ttu-id="2023a-391">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntitySetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="2023a-392">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-392">Attribute Name</span></span>           | <span data-ttu-id="2023a-393">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-393">Is Required</span></span> | <span data-ttu-id="2023a-394">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-395">**Name**</span></span>                 | <span data-ttu-id="2023a-396">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-396">Yes</span></span>         | <span data-ttu-id="2023a-397">Nom du jeu d'entités de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="2023a-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="2023a-398">**TypeName** **1**</span></span>       | <span data-ttu-id="2023a-399">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-399">No</span></span>          | <span data-ttu-id="2023a-400">Nom du type d'entité de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="2023a-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="2023a-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="2023a-402">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-402">No</span></span>          | <span data-ttu-id="2023a-403">Nom du jeu d'entités de modèle de stockage de destination du mappage.</span><span class="sxs-lookup"><span data-stu-id="2023a-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="2023a-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="2023a-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="2023a-405">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-405">No</span></span>          | <span data-ttu-id="2023a-406">**True** ou **False** selon si seules des lignes distinctes sont retournées.</span><span class="sxs-lookup"><span data-stu-id="2023a-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="2023a-407">Si cet attribut est défini **True**, le **generateupdateviews comme** attribut de l’élément EntityContainerMapping doit être définie sur **False**.</span><span class="sxs-lookup"><span data-stu-id="2023a-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="2023a-408">**1** le **TypeName** et **StoreEntitySet** attributs peuvent être utilisés à la place les éléments enfant EntityTypeMapping et MappingFragment pour mapper un type d’entité unique à une seule table.</span><span class="sxs-lookup"><span data-stu-id="2023a-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="2023a-409">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-409">Example</span></span>

<span data-ttu-id="2023a-410">L’exemple suivant montre un **EntitySetMapping** élément qui mappe les trois types (un type de base et deux types dérivés) dans le **cours** jeu d’entités du modèle conceptuel à trois tables différentes dans le base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="2023a-411">Les tables sont spécifiées par le **StoreEntitySet** attribut dans chaque **MappingFragment** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="2023a-412">EntityTypeMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-413">Le **EntityTypeMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre un type d’entité dans le modèle conceptuel et les tables ou vues dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="2023a-414">Pour plus d’informations sur les types d’entité de modèle conceptuel et les tables de base de données ou les vues sous-jacentes, consultez l’élément EntityType (CSDL) et élément EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="2023a-415">Le type d’entité de modèle conceptuel mappé est spécifié par le **TypeName** attribut de la **EntityTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="2023a-416">La table ou vue qui est mappé est spécifié par le **StoreEntitySet** attribut de l’élément MappingFragment enfant.</span><span class="sxs-lookup"><span data-stu-id="2023a-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="2023a-417">Le ModificationFunctionMapping, élément enfant peut être utilisé pour mapper l’insertion, mise à jour ou supprimer des fonctions de types d’entité aux procédures stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="2023a-418">Le **EntityTypeMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-419">MappingFragment (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="2023a-420">ModificationFunctionMapping (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="2023a-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-421">ScalarProperty</span></span>
-   <span data-ttu-id="2023a-422">Condition</span><span class="sxs-lookup"><span data-stu-id="2023a-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-423">**MappingFragment** et **ModificationFunctionMapping** éléments ne peut pas être des éléments enfants de la **EntityTypeMapping** élément en même temps.</span><span class="sxs-lookup"><span data-stu-id="2023a-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="2023a-424">Le **ScalarProperty** et **Condition** éléments peuvent être uniquement les éléments enfants de la **EntityTypeMapping** élément lorsqu’il est utilisé dans un élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-425">Applicable Attributes</span></span>

<span data-ttu-id="2023a-426">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **EntityTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="2023a-427">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-427">Attribute Name</span></span> | <span data-ttu-id="2023a-428">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-428">Is Required</span></span> | <span data-ttu-id="2023a-429">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="2023a-430">**TypeName**</span></span>   | <span data-ttu-id="2023a-431">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-431">Yes</span></span>         | <span data-ttu-id="2023a-432">Nom qualifié par un espace de noms du type d'entité de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="2023a-433">Si le type correspond à un type abstrait ou dérivé, la valeur doit être `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="2023a-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-434">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-434">Example</span></span>

<span data-ttu-id="2023a-435">L’exemple suivant montre un élément EntitySetMapping avec deux enfants **EntityTypeMapping** éléments.</span><span class="sxs-lookup"><span data-stu-id="2023a-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="2023a-436">Dans la première **EntityTypeMapping** élément, le **SchoolModel.Person** type d’entité est mappé à la **personne** table.</span><span class="sxs-lookup"><span data-stu-id="2023a-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="2023a-437">Dans la seconde **EntityTypeMapping** élément, la fonctionnalité de mise à jour de la **SchoolModel.Person** type est mappé à une procédure stockée, **UpdatePerson**, dans la base de données .</span><span class="sxs-lookup"><span data-stu-id="2023a-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-438">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-438">Example</span></span>

<span data-ttu-id="2023a-439">L'exemple suivant illustre le mappage d'une hiérarchie de types dont le type racine est abstrait.</span><span class="sxs-lookup"><span data-stu-id="2023a-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="2023a-440">Notez l’utilisation de la `IsOfType` syntaxe pour le **TypeName** attributs.</span><span class="sxs-lookup"><span data-stu-id="2023a-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="2023a-441">FunctionImportMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-442">Le **FunctionImportMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée ou fonction dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="2023a-443">Les importations de fonction doivent être déclarées dans le modèle conceptuel et les procédures stockées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="2023a-444">Pour plus d’informations, consultez l’élément FunctionImport (CSDL) et élément (fonction) (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-445">Par défaut, si une importation de fonction retourne un type d'entité ou un type complexe de modèle conceptuel, les noms des colonnes retournés par la procédure stockée sous-jacente doivent correspondre exactement aux noms des propriétés sur le type de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="2023a-446">Si les noms de colonnes ne correspondent pas exactement les noms de propriété, le mappage doit être défini dans un élément ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="2023a-447">Le **FunctionImportMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-448">ResultMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-449">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-449">Applicable Attributes</span></span>

<span data-ttu-id="2023a-450">Le tableau suivant décrit les attributs qui s’appliquent à la **FunctionImportMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="2023a-451">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-451">Attribute Name</span></span>         | <span data-ttu-id="2023a-452">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-452">Is Required</span></span> | <span data-ttu-id="2023a-453">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-454">**FunctionImportName se**</span><span class="sxs-lookup"><span data-stu-id="2023a-454">**FunctionImportName**</span></span> | <span data-ttu-id="2023a-455">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-455">Yes</span></span>         | <span data-ttu-id="2023a-456">Nom de l'importation de fonction dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="2023a-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-457">**FunctionName**</span></span>       | <span data-ttu-id="2023a-458">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-458">Yes</span></span>         | <span data-ttu-id="2023a-459">Nom qualifié par un espace de noms de la fonction dans le modèle de stockage mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-460">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-460">Example</span></span>

<span data-ttu-id="2023a-461">L’exemple suivant est basé sur le modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-461">The following example is based on the School model.</span></span> <span data-ttu-id="2023a-462">Considérez la fonction suivante dans le modèle de stockage :</span><span class="sxs-lookup"><span data-stu-id="2023a-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="2023a-463">Considérez également cette importation de fonction dans le modèle conceptuel :</span><span class="sxs-lookup"><span data-stu-id="2023a-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="2023a-464">L’exemple suivant affiche un **FunctionImportMapping** élément utilisé pour mapper la fonction et l’importation de fonction ci-dessus entre eux :</span><span class="sxs-lookup"><span data-stu-id="2023a-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="2023a-465">InsertFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="2023a-466">Le **InsertFunction** élément dans la spécification du langage MSL (mapping) est mappé à la fonction d’insertion d’un type d’entité ou une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="2023a-467">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="2023a-468">Pour plus d’informations, consultez la fonction élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-469">Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="2023a-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="2023a-470">Le **InsertFunction** élément peut être un enfant de l’élément ModificationFunctionMapping et appliqué à l’élément EntityTypeMapping ou de l’élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="2023a-471">Application d'InsertFunction à EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="2023a-472">Lorsqu’il est appliqué à l’élément EntityTypeMapping, le **InsertFunction** élément est mappé à la fonction d’insertion d’un type d’entité dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="2023a-473">Le **InsertFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à un **EntityTypeMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="2023a-474">AssociationEnd (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="2023a-475">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="2023a-476">ResultBinding (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="2023a-477">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2023a-478">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-478">Applicable Attributes</span></span>

<span data-ttu-id="2023a-479">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **InsertFunction** élément lorsqu’il est appliqué à un **EntityTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="2023a-480">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-480">Attribute Name</span></span>            | <span data-ttu-id="2023a-481">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-481">Is Required</span></span> | <span data-ttu-id="2023a-482">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-483">**FunctionName**</span></span>          | <span data-ttu-id="2023a-484">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-484">Yes</span></span>         | <span data-ttu-id="2023a-485">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="2023a-486">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="2023a-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="2023a-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="2023a-488">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-488">No</span></span>          | <span data-ttu-id="2023a-489">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="2023a-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="2023a-490">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-490">Example</span></span>

<span data-ttu-id="2023a-491">L’exemple suivant est basé sur le modèle School et montre la **InsertFunction** élément utilisé pour mapper la fonction d’insertion du type d’entité Person à la **InsertPerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="2023a-492">Le **InsertPerson** procédure stockée est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="2023a-493">Application d'InsertFunction à AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="2023a-494">Lorsqu’il est appliqué à l’élément AssociationSetMapping, le **InsertFunction** élément est mappé à la fonction d’insertion d’une association dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="2023a-495">Le **InsertFunction** élément peut avoir les éléments enfants suivants lorsqu’il est appliqué à la **AssociationSetMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="2023a-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2023a-497">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-497">Applicable Attributes</span></span>

<span data-ttu-id="2023a-498">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **InsertFunction** élément lorsqu’il est appliqué à la **AssociationSetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="2023a-499">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-499">Attribute Name</span></span>            | <span data-ttu-id="2023a-500">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-500">Is Required</span></span> | <span data-ttu-id="2023a-501">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-502">**FunctionName**</span></span>          | <span data-ttu-id="2023a-503">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-503">Yes</span></span>         | <span data-ttu-id="2023a-504">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="2023a-505">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="2023a-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="2023a-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="2023a-507">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-507">No</span></span>          | <span data-ttu-id="2023a-508">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="2023a-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="2023a-509">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-509">Example</span></span>

<span data-ttu-id="2023a-510">L’exemple suivant est basé sur le modèle School et montre la **InsertFunction** élément utilisé pour mapper la fonction d’insertion de la **CourseInstructor** association à la  **InsertCourseInstructor** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="2023a-511">Le **InsertCourseInstructor** procédure stockée est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="2023a-512">Élément de mappage (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="2023a-513">Le **mappage** élément dans la spécification du langage MSL (mapping) contient des informations de mappage des objets qui sont définis dans un modèle conceptuel à une base de données (comme décrit dans un modèle de stockage).</span><span class="sxs-lookup"><span data-stu-id="2023a-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="2023a-514">Pour plus d’informations, consultez la spécification CSDL et SSDL Specification.</span><span class="sxs-lookup"><span data-stu-id="2023a-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="2023a-515">Le **mappage** élément est l’élément racine pour une spécification de mappage.</span><span class="sxs-lookup"><span data-stu-id="2023a-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="2023a-516">L’espace de noms XML pour les spécifications de mappage est http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="2023a-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="2023a-517">L'élément de mappage peut avoir les éléments enfants suivants (dans l'ordre répertorié) :</span><span class="sxs-lookup"><span data-stu-id="2023a-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2023a-518">Alias (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="2023a-519">EntityContainerMapping (exactement un élément)</span><span class="sxs-lookup"><span data-stu-id="2023a-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="2023a-520">Les noms de types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="2023a-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="2023a-521">Pour plus d’informations sur le nom d’espace de noms de modèle conceptuel, consultez l’élément Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="2023a-522">Pour plus d’informations sur le nom d’espace de noms de modèle stockage, consultez l’élément de schéma (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="2023a-523">Alias pour les espaces de noms qui sont utilisés en MSL peuvent être définis avec l’élément de l’Alias.</span><span class="sxs-lookup"><span data-stu-id="2023a-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-524">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-524">Applicable Attributes</span></span>

<span data-ttu-id="2023a-525">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à la **mappage** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="2023a-526">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-526">Attribute Name</span></span> | <span data-ttu-id="2023a-527">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-527">Is Required</span></span> | <span data-ttu-id="2023a-528">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="2023a-529">**Espace**</span><span class="sxs-lookup"><span data-stu-id="2023a-529">**Space**</span></span>      | <span data-ttu-id="2023a-530">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-530">Yes</span></span>         | <span data-ttu-id="2023a-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="2023a-531">**C-S**.</span></span> <span data-ttu-id="2023a-532">Il s'agit d'une valeur fixe qui ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="2023a-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-533">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-533">Example</span></span>

<span data-ttu-id="2023a-534">L’exemple suivant montre un **mappage** élément basé sur une partie du modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="2023a-535">Pour plus d’informations sur le modèle School, consultez le Guide de démarrage rapide (Entity Framework) :</span><span class="sxs-lookup"><span data-stu-id="2023a-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="2023a-536">MappingFragment, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="2023a-537">Le **MappingFragment** élément dans la spécification du langage MSL (mapping) définit le mappage entre les propriétés d’un type d’entité de modèle conceptuel et une table ou vue dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="2023a-538">Pour plus d’informations sur les types d’entité de modèle conceptuel et les tables de base de données ou les vues sous-jacentes, consultez l’élément EntityType (CSDL) et élément EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="2023a-539">Le **MappingFragment** peut être un élément enfant de l’élément EntityTypeMapping ou l’élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="2023a-540">Le **MappingFragment** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-541">ComplexType (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="2023a-542">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="2023a-543">Condition (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-544">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-544">Applicable Attributes</span></span>

<span data-ttu-id="2023a-545">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **MappingFragment** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="2023a-546">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-546">Attribute Name</span></span>          | <span data-ttu-id="2023a-547">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-547">Is Required</span></span> | <span data-ttu-id="2023a-548">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="2023a-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="2023a-550">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-550">Yes</span></span>         | <span data-ttu-id="2023a-551">Nom de la table ou de la vue mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="2023a-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="2023a-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="2023a-553">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-553">No</span></span>          | <span data-ttu-id="2023a-554">**True** ou **False** selon si seules des lignes distinctes sont retournées.</span><span class="sxs-lookup"><span data-stu-id="2023a-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="2023a-555">Si cet attribut est défini **True**, le **generateupdateviews comme** attribut de l’élément EntityContainerMapping doit être définie sur **False**.</span><span class="sxs-lookup"><span data-stu-id="2023a-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-556">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-556">Example</span></span>

<span data-ttu-id="2023a-557">L’exemple suivant montre un **MappingFragment** élément comme enfant d’un **EntityTypeMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="2023a-558">Dans cet exemple, les propriétés de la **cours** type dans le modèle conceptuel sont mappées aux colonnes de la **cours** table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-559">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-559">Example</span></span>

<span data-ttu-id="2023a-560">L’exemple suivant montre un **MappingFragment** élément comme enfant d’un **EntitySetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="2023a-561">Comme dans l’exemple ci-dessus, les propriétés de la **cours** type dans le modèle conceptuel sont mappées aux colonnes de la **cours** table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="2023a-562">ModificationFunctionMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-563">Le **ModificationFunctionMapping** élément dans la spécification du langage MSL (mapping) mappe l’insertion, mise à jour et supprimer des fonctions d’un type d’entité de modèle conceptuel aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="2023a-564">Le **ModificationFunctionMapping** élément peut également mapper l’insertion et supprimer des fonctions pour les associations plusieurs-à-plusieurs dans le modèle conceptuel aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="2023a-565">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="2023a-566">Pour plus d’informations, consultez la fonction élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-567">Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="2023a-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="2023a-568">Si les fonctions de modification pour une entité dans une hiérarchie d'héritage sont mappées aux procédures stockées, les fonctions de modification de tous les types dans la hiérarchie doivent être mappées aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="2023a-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="2023a-569">Le **ModificationFunctionMapping** élément peut être un enfant de l’élément AssociationSetMapping ou de l’élément EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="2023a-570">Le **ModificationFunctionMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-571">DeleteFunction (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="2023a-572">InsertFunction (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="2023a-573">UpdateFunction (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="2023a-574">Aucun attribut n’est applicable à la **ModificationFunctionMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="2023a-575">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-575">Example</span></span>

<span data-ttu-id="2023a-576">L’exemple suivant montre l’entité de mappage du jeu du **personnes** jeu d’entités dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="2023a-577">Outre le mappage de colonne pour le **personne** type d’entité, le mappage de l’insertion, mise à jour et supprimer des fonctions de la **personne** type sont affichés.</span><span class="sxs-lookup"><span data-stu-id="2023a-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="2023a-578">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-579">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-579">Example</span></span>

<span data-ttu-id="2023a-580">L’exemple suivant illustre l’association de définir le mappage pour le **CourseInstructor** ensemble d’associations dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="2023a-581">Outre le mappage de colonne pour le **CourseInstructor** association, le mappage des fonctions insert et delete de la **CourseInstructor** association sont affichés.</span><span class="sxs-lookup"><span data-stu-id="2023a-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="2023a-582">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="2023a-583">QueryView, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="2023a-584">Le **QueryView** élément dans la spécification du langage MSL (mapping) définit un mappage en lecture seule entre un type d’entité ou une association dans le modèle conceptuel et une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="2023a-585">Le mappage est défini avec une requête Entity SQL qui est évaluée par rapport au modèle de stockage et vous exprimez le jeu en termes d’une entité ou une association dans le modèle conceptuel de résultats.</span><span class="sxs-lookup"><span data-stu-id="2023a-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="2023a-586">Les affichages des requêtes étant en lecture seule, les types qu'ils définissent ne peuvent pas être mis à jour au moyen des commandes de mise à jour standard.</span><span class="sxs-lookup"><span data-stu-id="2023a-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="2023a-587">Les mises à jour de ces types peuvent être effectuées au moyen de fonctions de modification.</span><span class="sxs-lookup"><span data-stu-id="2023a-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="2023a-588">Pour plus d’informations, consultez Comment : mappage des fonctions de Modification aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="2023a-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-589">Dans le **QueryView** élément, les expressions Entity SQL qui contiennent **GroupBy**, agrégats de groupe ou les propriétés de navigation ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="2023a-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="2023a-590">Le **QueryView** élément peut être un enfant de l’élément EntitySetMapping ou AssociationSetMapping, élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="2023a-591">Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une entité dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="2023a-592">Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une association dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-593">Si le **AssociationSetMapping** élément concerne une association avec une contrainte référentielle, les **AssociationSetMapping** élément est ignoré.</span><span class="sxs-lookup"><span data-stu-id="2023a-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="2023a-594">Pour plus d’informations, consultez élément ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="2023a-595">Le **QueryView** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="2023a-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-596">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-596">Applicable Attributes</span></span>

<span data-ttu-id="2023a-597">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **QueryView** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="2023a-598">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-598">Attribute Name</span></span> | <span data-ttu-id="2023a-599">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-599">Is Required</span></span> | <span data-ttu-id="2023a-600">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="2023a-601">**TypeName**</span></span>   | <span data-ttu-id="2023a-602">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-602">No</span></span>          | <span data-ttu-id="2023a-603">Nom du type de modèle conceptuel mappé par l'affichage des requêtes.</span><span class="sxs-lookup"><span data-stu-id="2023a-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-604">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-604">Example</span></span>

<span data-ttu-id="2023a-605">L’exemple suivant montre le **QueryView** élément en tant qu’enfant de le **EntitySetMapping** élément et définit un mappage de vue de requête pour le **département** type d’entité dans le Modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="2023a-606">Étant donné que la requête retourne uniquement un sous-ensemble des membres de la **département** type du modèle de stockage, le **département** type dans le modèle School a été modifié en fonction de ce mappage comme suit :</span><span class="sxs-lookup"><span data-stu-id="2023a-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-607">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-607">Example</span></span>

<span data-ttu-id="2023a-608">L’exemple suivant montre le **QueryView** élément comme enfant d’un **AssociationSetMapping** élément et définit un mappage en lecture seule pour le `FK_Course_Department` association dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="2023a-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="2023a-609">Commentaires</span><span class="sxs-lookup"><span data-stu-id="2023a-609">Comments</span></span>

<span data-ttu-id="2023a-610">Vous pouvez définir des affichages des requêtes pour activer les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="2023a-611">Définir une entité du modèle conceptuel qui n'inclut pas toutes les propriétés de l'entité dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="2023a-612">Cela inclut les propriétés qui n’ont pas de valeurs par défaut et ne gèrent pas **null** valeurs.</span><span class="sxs-lookup"><span data-stu-id="2023a-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="2023a-613">Mapper des colonnes calculées du modèle de stockage aux propriétés de types d'entités du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="2023a-614">Définir un mappage dans lequel les conditions utilisées pour partitionner des entités du modèle conceptuel ne sont pas basées sur l'égalité.</span><span class="sxs-lookup"><span data-stu-id="2023a-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="2023a-615">Lorsque vous spécifiez un mappage conditionnel à l’aide de la **Condition** élément, la condition fournie doit être égale à la valeur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="2023a-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="2023a-616">Pour plus d’informations, consultez Condition élément (MSL).</span><span class="sxs-lookup"><span data-stu-id="2023a-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="2023a-617">Mapper la même colonne du modèle de stockage à plusieurs types du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="2023a-618">Mapper plusieurs types à la même table.</span><span class="sxs-lookup"><span data-stu-id="2023a-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="2023a-619">Définir des associations dans le modèle conceptuel qui ne sont pas basées sur des clés étrangères du schéma relationnel.</span><span class="sxs-lookup"><span data-stu-id="2023a-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="2023a-620">Utiliser une logique métier personnalisée pour définir la valeur de propriétés du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="2023a-621">Par exemple, vous pouvez la mapper la valeur de chaîne « T » dans la source de données à une valeur **true**, une valeur booléenne, dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="2023a-622">Définir des filtres conditionnels pour les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="2023a-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="2023a-623">Appliquer moins de restrictions sur les données dans le modèle conceptuel que dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="2023a-624">Par exemple, vous pouvez rendre une propriété dans le modèle conceptuel nullable même si la colonne à laquelle elle est mappée ne prend pas en charge **null**valeurs.</span><span class="sxs-lookup"><span data-stu-id="2023a-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="2023a-625">Vous devez tenir compte des points suivants lorsque vous définissez des affichages des requêtes pour les entités :</span><span class="sxs-lookup"><span data-stu-id="2023a-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="2023a-626">Les affichages des requêtes sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="2023a-626">Query views are read-only.</span></span> <span data-ttu-id="2023a-627">Les mises à jour des entités ne peuvent être effectuées qu'au moyen de fonctions de modification.</span><span class="sxs-lookup"><span data-stu-id="2023a-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="2023a-628">Lorsque vous définissez un type d'entité par un affichage des requêtes, vous devez également définir toutes les entités associées par les affichages des requêtes.</span><span class="sxs-lookup"><span data-stu-id="2023a-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="2023a-629">Lorsque vous mappez une association plusieurs-à-plusieurs à une entité du modèle de stockage qui représente une table de liens dans le schéma relationnel, vous devez définir un **QueryView** élément dans le **AssociationSetMapping** élément pour cette table de liens.</span><span class="sxs-lookup"><span data-stu-id="2023a-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="2023a-630">Les affichages des requêtes doivent être définis pour tous les types d'une hiérarchie des types.</span><span class="sxs-lookup"><span data-stu-id="2023a-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="2023a-631">Pour ce faire, vous pouvez procéder de différentes façons :</span><span class="sxs-lookup"><span data-stu-id="2023a-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="2023a-632">Avec un seul **QueryView** élément qui spécifie une seule requête Entity SQL qui retourne une union de tous les types d’entités dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="2023a-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="2023a-633">Avec un seul **QueryView** élément qui spécifie une requête Entity SQL unique qui utilise l’opérateur de cas pour retourner un type d’entité spécifique dans la hiérarchie selon une condition spécifique.</span><span class="sxs-lookup"><span data-stu-id="2023a-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="2023a-634">Avec un autre **QueryView** élément pour un type spécifique dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="2023a-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="2023a-635">Dans ce cas, utilisez le **TypeName** attribut de la **QueryView** élément pour spécifier le type d’entité pour chaque vue.</span><span class="sxs-lookup"><span data-stu-id="2023a-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="2023a-636">Lorsqu’un affichage des requêtes est défini, vous ne pouvez pas spécifier le **StorageSetName** d’attribut sur le **EntitySetMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="2023a-637">Lorsqu’un affichage des requêtes est défini, le **EntitySetMapping**élément ne peut pas contenir également **propriété** mappages.</span><span class="sxs-lookup"><span data-stu-id="2023a-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="2023a-638">Élément ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="2023a-639">Le **ResultBinding** élément dans la spécification du langage MSL (mapping) mappe les valeurs de colonne sont retournées par les procédures stockées aux propriétés d’entités dans le modèle conceptuel lorsque les fonctions de modification de type entité sont mappées à stockée procédures dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="2023a-640">Par exemple, lorsque la valeur d’une colonne d’identité est retournée par une instruction insert procédure stockée, le **ResultBinding** élément est mappé à la valeur renvoyée à une propriété de type d’entité dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="2023a-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="2023a-641">Le **ResultBinding** élément peut être l’enfant de l’élément InsertFunction ou l’élément UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="2023a-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="2023a-642">Le **ResultBinding** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="2023a-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-643">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-643">Applicable Attributes</span></span>

<span data-ttu-id="2023a-644">Le tableau suivant décrit les attributs qui s’appliquent à la **ResultBinding** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="2023a-645">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-645">Attribute Name</span></span> | <span data-ttu-id="2023a-646">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-646">Is Required</span></span> | <span data-ttu-id="2023a-647">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-648">**Name**</span></span>       | <span data-ttu-id="2023a-649">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-649">Yes</span></span>         | <span data-ttu-id="2023a-650">Nom de la propriété d'entité dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="2023a-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="2023a-651">**ColumnName**</span></span> | <span data-ttu-id="2023a-652">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-652">Yes</span></span>         | <span data-ttu-id="2023a-653">Nom de la colonne mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="2023a-654">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-654">Example</span></span>

<span data-ttu-id="2023a-655">L’exemple suivant est basé sur le modèle School et montre un **InsertFunction** élément utilisé pour mapper la fonction d’insertion de la **personne** type d’entité à la **InsertPerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="2023a-656">(Le **InsertPerson** procédure stockée est indiquée ci-dessous et est déclarée dans le modèle de stockage.) Un **ResultBinding** élément est utilisé pour mapper une valeur de colonne qui est retournée par la procédure stockée (**NewPersonID**) à une propriété de type d’entité (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="2023a-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="2023a-657">Transact-SQL suivante décrit le **InsertPerson** procédure stockée :</span><span class="sxs-lookup"><span data-stu-id="2023a-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="2023a-658">Élément ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="2023a-659">Le **ResultMapping** élément dans la spécification du langage MSL (mapping) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans la base de données sous-jacente lorsque les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="2023a-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="2023a-660">L'importation de fonction retourne un type d'entité de modèle conceptuel ou le type complexe.</span><span class="sxs-lookup"><span data-stu-id="2023a-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="2023a-661">Les noms des colonnes retournés par la procédure stockée ne correspondent pas exactement aux noms des propriétés sur le type d'entité ou le type complexe.</span><span class="sxs-lookup"><span data-stu-id="2023a-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="2023a-662">Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type d'entité ou un type complexe est basé sur les noms de colonne et de propriété.</span><span class="sxs-lookup"><span data-stu-id="2023a-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="2023a-663">Si les noms de colonnes ne correspondent pas exactement les noms de propriété, vous devez utiliser le **ResultMapping** élément pour définir le mappage.</span><span class="sxs-lookup"><span data-stu-id="2023a-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="2023a-664">Pour obtenir un exemple du mappage par défaut, consultez l’élément FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="2023a-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="2023a-665">Le **ResultMapping** élément est un élément enfant de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="2023a-666">Le **ResultMapping** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-667">EntityTypeMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="2023a-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-668">ComplexTypeMapping</span></span>

<span data-ttu-id="2023a-669">Aucun attribut n’est applicable à la **ResultMapping** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="2023a-670">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-670">Example</span></span>

<span data-ttu-id="2023a-671">Considérons la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="2023a-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="2023a-672">De même, considérons le type d'entité de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="2023a-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="2023a-673">Pour créer une importation de fonction qui retourne des instances du type d’entité précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un **ResultMapping** élément :</span><span class="sxs-lookup"><span data-stu-id="2023a-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="2023a-674">ScalarProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="2023a-675">Le **ScalarProperty** élément dans la spécification du langage MSL (mapping) est mappé à une propriété sur un type d’entité de modèle conceptuel, un type complexe ou une association à une colonne de table ou un paramètre de procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="2023a-676">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="2023a-677">Pour plus d’informations, consultez la fonction élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="2023a-678">Le **ScalarProperty** élément peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="2023a-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="2023a-679">MappingFragment</span></span>
-   <span data-ttu-id="2023a-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="2023a-680">InsertFunction</span></span>
-   <span data-ttu-id="2023a-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="2023a-681">UpdateFunction</span></span>
-   <span data-ttu-id="2023a-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="2023a-682">DeleteFunction</span></span>
-   <span data-ttu-id="2023a-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-683">EndProperty</span></span>
-   <span data-ttu-id="2023a-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="2023a-684">ComplexProperty</span></span>
-   <span data-ttu-id="2023a-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="2023a-685">ResultMapping</span></span>

<span data-ttu-id="2023a-686">En tant qu’enfant de le **MappingFragment**, **ComplexProperty**, ou **EndProperty** élément, le **ScalarProperty** élément est mappé à une propriété dans le modèle conceptuel à une colonne dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="2023a-687">En tant qu’enfant de le **InsertFunction**, **UpdateFunction**, ou **DeleteFunction** élément, le **ScalarProperty** élément est mappé à une propriété dans le modèle conceptuel à un paramètre de procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="2023a-688">Le **ScalarProperty** élément ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="2023a-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-689">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-689">Applicable Attributes</span></span>

<span data-ttu-id="2023a-690">Les attributs qui s’appliquent à la **ScalarProperty** élément diffèrent en fonction du rôle de l’élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="2023a-691">Le tableau suivant décrit les attributs qui sont applicables lorsque le **ScalarProperty** élément est utilisé pour mapper une propriété de modèle conceptuel à une colonne dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="2023a-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="2023a-692">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-692">Attribute Name</span></span> | <span data-ttu-id="2023a-693">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-693">Is Required</span></span> | <span data-ttu-id="2023a-694">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="2023a-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-695">**Name**</span></span>       | <span data-ttu-id="2023a-696">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-696">Yes</span></span>         | <span data-ttu-id="2023a-697">Nom de la propriété de modèle conceptuel mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="2023a-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="2023a-698">**ColumnName**</span></span> | <span data-ttu-id="2023a-699">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-699">Yes</span></span>         | <span data-ttu-id="2023a-700">Nom de la colonne de table mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="2023a-701">Le tableau suivant décrit les attributs qui s’appliquent à la **ScalarProperty** élément lorsqu’il est utilisé pour mapper une propriété de modèle conceptuel à un paramètre de procédure stockée :</span><span class="sxs-lookup"><span data-stu-id="2023a-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="2023a-702">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-702">Attribute Name</span></span>    | <span data-ttu-id="2023a-703">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-703">Is Required</span></span> | <span data-ttu-id="2023a-704">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="2023a-705">**Name**</span></span>          | <span data-ttu-id="2023a-706">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-706">Yes</span></span>         | <span data-ttu-id="2023a-707">Nom de la propriété de modèle conceptuel mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="2023a-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="2023a-708">**ParameterName**</span></span> | <span data-ttu-id="2023a-709">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-709">Yes</span></span>         | <span data-ttu-id="2023a-710">Nom du paramètre mappé.</span><span class="sxs-lookup"><span data-stu-id="2023a-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="2023a-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="2023a-711">**Version**</span></span>       | <span data-ttu-id="2023a-712">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-712">No</span></span>          | <span data-ttu-id="2023a-713">**Actuel** ou **d’origine** selon que la valeur actuelle ou la valeur d’origine de la propriété doit être utilisée pour les contrôles d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="2023a-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="2023a-714">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-714">Example</span></span>

<span data-ttu-id="2023a-715">L’exemple suivant montre le **ScalarProperty** élément utilisé de deux manières :</span><span class="sxs-lookup"><span data-stu-id="2023a-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="2023a-716">Pour mapper les propriétés de la **personne** type d’entité aux colonnes de la **personne**table.</span><span class="sxs-lookup"><span data-stu-id="2023a-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="2023a-717">Pour mapper les propriétés de la **personne** type d’entité pour les paramètres de la **UpdatePerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="2023a-718">Les procédures stockées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="2023a-719">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-719">Example</span></span>

<span data-ttu-id="2023a-720">L’exemple suivant montre le **ScalarProperty** élément utilisé pour mapper l’insertion et de fonctions de suppression d’une association de modèle conceptuel aux procédures stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2023a-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="2023a-721">Les procédures stockées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="2023a-722">UpdateFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="2023a-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="2023a-723">Le **UpdateFunction** élément dans la spécification du langage MSL (mapping) mappe la fonction de mise à jour d’un type d’entité dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2023a-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="2023a-724">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="2023a-725">Pour plus d’informations, consultez la fonction élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2023a-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="2023a-726">Si vous ne mappez pas les trois INSERT, update ou delete opérations d’un type d’entité aux procédures stockées, les opérations non mappées échouent lors de l’exécution et une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="2023a-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="2023a-727">Le **UpdateFunction** élément peut être un enfant de l’élément ModificationFunctionMapping et appliqué à l’élément EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="2023a-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="2023a-728">Le **UpdateFunction** élément peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="2023a-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="2023a-729">AssociationEnd (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="2023a-730">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="2023a-731">ResultBinding (zéro ou 1)</span><span class="sxs-lookup"><span data-stu-id="2023a-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="2023a-732">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="2023a-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2023a-733">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="2023a-733">Applicable Attributes</span></span>

<span data-ttu-id="2023a-734">Le tableau suivant décrit les attributs qui peuvent être appliqués à la **UpdateFunction** élément.</span><span class="sxs-lookup"><span data-stu-id="2023a-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="2023a-735">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="2023a-735">Attribute Name</span></span>            | <span data-ttu-id="2023a-736">Requis</span><span class="sxs-lookup"><span data-stu-id="2023a-736">Is Required</span></span> | <span data-ttu-id="2023a-737">Value</span><span class="sxs-lookup"><span data-stu-id="2023a-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2023a-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="2023a-738">**FunctionName**</span></span>          | <span data-ttu-id="2023a-739">Oui</span><span class="sxs-lookup"><span data-stu-id="2023a-739">Yes</span></span>         | <span data-ttu-id="2023a-740">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de mise à jour est mappée.</span><span class="sxs-lookup"><span data-stu-id="2023a-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="2023a-741">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="2023a-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="2023a-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="2023a-743">Non</span><span class="sxs-lookup"><span data-stu-id="2023a-743">No</span></span>          | <span data-ttu-id="2023a-744">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="2023a-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="2023a-745">Exemple</span><span class="sxs-lookup"><span data-stu-id="2023a-745">Example</span></span>

<span data-ttu-id="2023a-746">L’exemple suivant est basé sur le modèle School et montre la **UpdateFunction** élément utilisé pour mapper la fonction de mise à jour de la **personne** type d’entité à la **UpdatePerson** procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="2023a-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="2023a-747">Le **UpdatePerson** procédure stockée est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="2023a-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
