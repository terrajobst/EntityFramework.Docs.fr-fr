---
title: Spécification MSL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182553"
---
# <a name="msl-specification"></a><span data-ttu-id="95933-102">Spécification MSL</span><span class="sxs-lookup"><span data-stu-id="95933-102">MSL Specification</span></span>
<span data-ttu-id="95933-103">Le langage MSL (Mapping Specification Language) est un langage basé sur XML qui décrit le mappage entre le modèle conceptuel et le modèle de stockage d’une application Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="95933-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="95933-104">Dans une application Entity Framework, les métadonnées de mappage sont chargées à partir d’un fichier. MSL (écrit en MSL) au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="95933-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="95933-105">Entity Framework utilise les métadonnées de mappage au moment de l’exécution pour traduire les requêtes sur le modèle conceptuel en commandes spécifiques au stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="95933-106">Le Entity Framework Designer (concepteur EF) stocke les informations de mappage dans un fichier. edmx au moment de la conception.</span><span class="sxs-lookup"><span data-stu-id="95933-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="95933-107">Au moment de la génération, le Entity Designer utilise les informations d’un fichier. edmx pour créer le fichier. MSL requis par Entity Framework au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="95933-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="95933-108">Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="95933-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="95933-109">Pour plus d’informations sur le nom de l’espace de noms du modèle conceptuel, consultez [spécification CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="95933-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="95933-110">Pour plus d’informations sur le nom de l’espace de noms du modèle de stockage, consultez la [spécification SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="95933-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="95933-111">Les versions de MSL sont différenciées par les espaces de noms XML.</span><span class="sxs-lookup"><span data-stu-id="95933-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="95933-112">Version MSL</span><span class="sxs-lookup"><span data-stu-id="95933-112">MSL Version</span></span> | <span data-ttu-id="95933-113">Espace de noms XML</span><span class="sxs-lookup"><span data-stu-id="95933-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="95933-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="95933-114">MSL v1</span></span>      | <span data-ttu-id="95933-115">urn : schemas-microsoft-com : Windows : Storage : Mapping : CS</span><span class="sxs-lookup"><span data-stu-id="95933-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="95933-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="95933-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="95933-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="95933-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="95933-118">Élément Alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-118">Alias Element (MSL)</span></span>

<span data-ttu-id="95933-119">L’élément d' **alias** en Mapping Specification Language (MSL) est un enfant de l’élément de mappage utilisé pour définir des alias pour les espaces de noms du modèle conceptuel et du modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="95933-120">Les noms de tous les types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="95933-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="95933-121">Pour plus d’informations sur le nom de l’espace de noms du modèle conceptuel, consultez Schema, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="95933-122">Pour plus d’informations sur le nom de l’espace de noms du modèle de stockage, consultez Schema, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="95933-123">L’élément **alias** ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-124">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-124">Applicable Attributes</span></span>

<span data-ttu-id="95933-125">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **alias** .</span><span class="sxs-lookup"><span data-stu-id="95933-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="95933-126">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-126">Attribute Name</span></span> | <span data-ttu-id="95933-127">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-127">Is Required</span></span> | <span data-ttu-id="95933-128">Value</span><span class="sxs-lookup"><span data-stu-id="95933-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="95933-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="95933-129">**Key**</span></span>        | <span data-ttu-id="95933-130">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-130">Yes</span></span>         | <span data-ttu-id="95933-131">Alias de l’espace de noms spécifié par l’attribut **value** .</span><span class="sxs-lookup"><span data-stu-id="95933-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="95933-132">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="95933-132">**Value**</span></span>      | <span data-ttu-id="95933-133">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-133">Yes</span></span>         | <span data-ttu-id="95933-134">Espace de noms pour lequel la valeur de l’élément **Key** est un alias.</span><span class="sxs-lookup"><span data-stu-id="95933-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="95933-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-135">Example</span></span>

<span data-ttu-id="95933-136">L’exemple suivant montre un élément d' **alias** qui définit un alias, `c`, pour les types définis dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="95933-137">AssociationEnd, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="95933-138">L’élément **AssociationEnd** en Mapping Specification Language (MSL) est utilisé lorsque les fonctions de modification d’un type d’entité dans le modèle conceptuel sont mappées aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="95933-139">Si une procédure stockée de modification accepte un paramètre dont la valeur est conservée dans une propriété Association, l’élément **AssociationEnd** mappe la valeur de la propriété au paramètre.</span><span class="sxs-lookup"><span data-stu-id="95933-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="95933-140">Pour plus d'informations, voir l'exemple ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="95933-140">For more information, see the example below.</span></span>

<span data-ttu-id="95933-141">Pour plus d’informations sur le mappage des fonctions de modification de types d’entité aux procédures stockées, consultez l’élément ModificationFunctionMapping (MSL) et la procédure pas à pas : Mappage d’une entité à des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="95933-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="95933-142">L’élément **AssociationEnd** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="95933-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-144">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-144">Applicable Attributes</span></span>

<span data-ttu-id="95933-145">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="95933-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="95933-146">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-146">Attribute Name</span></span>     | <span data-ttu-id="95933-147">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-147">Is Required</span></span> | <span data-ttu-id="95933-148">Value</span><span class="sxs-lookup"><span data-stu-id="95933-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="95933-149">**AssociationSet**</span></span> | <span data-ttu-id="95933-150">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-150">Yes</span></span>         | <span data-ttu-id="95933-151">Nom de l'association mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="95933-152">**From**</span><span class="sxs-lookup"><span data-stu-id="95933-152">**From**</span></span>           | <span data-ttu-id="95933-153">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-153">Yes</span></span>         | <span data-ttu-id="95933-154">Valeur de l’attribut **FromRole** de la propriété de navigation qui correspond à l’Association qui est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="95933-155">Pour plus d’informations, consultez NavigationProperty, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="95933-156">**Pour**</span><span class="sxs-lookup"><span data-stu-id="95933-156">**To**</span></span>             | <span data-ttu-id="95933-157">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-157">Yes</span></span>         | <span data-ttu-id="95933-158">Valeur de l’attribut **ToRole** de la propriété de navigation qui correspond à l’Association qui est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="95933-159">Pour plus d’informations, consultez NavigationProperty, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="95933-160">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-160">Example</span></span>

<span data-ttu-id="95933-161">Considérons le type d'entité de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="95933-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="95933-162">Considérons également la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="95933-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="95933-163">Pour mapper la fonction de mise à jour de l’entité `Course` à cette procédure stockée, vous devez fournir une valeur au paramètre **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="95933-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="95933-164">La valeur pour `DepartmentID` ne correspond pas à une propriété sur le type d'entité ; elle est contenue dans une association indépendante dont le mappage est indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="95933-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="95933-165">Le code suivant montre l’élément **AssociationEnd** utilisé pour mapper la propriété **DepartmentID** de l’Association **FK @ no__t-3Course @ no__t-4Department** à la procédure stockée **UpdateCourse** (à laquelle la fonction de mise à jour de l’élément Le type d’entité **course** est mappé) :</span><span class="sxs-lookup"><span data-stu-id="95933-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="95933-166">AssociationSetMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="95933-167">L’élément **AssociationSetMapping** en Mapping Specification Language (MSL) définit le mappage entre une association dans le modèle conceptuel et les colonnes de table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="95933-168">Les associations dans le modèle conceptuel sont des types dont les propriétés représentent des colonnes de clé primaire et de clé étrangère dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="95933-169">L’élément **AssociationSetMapping** utilise deux éléments EndProperty pour définir les mappages entre les propriétés de type d’association et les colonnes de la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="95933-170">Vous pouvez placer des conditions sur ces mappages avec l’élément condition.</span><span class="sxs-lookup"><span data-stu-id="95933-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="95933-171">Mappez les fonctions Insert, Update et Delete pour les associations à des procédures stockées dans la base de données avec l’élément ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="95933-172">Définir des mappages en lecture seule entre des associations et des colonnes de table à l’aide d’une chaîne de Entity SQL dans un élément QueryView.</span><span class="sxs-lookup"><span data-stu-id="95933-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="95933-173">Si une contrainte référentielle est définie pour une association dans le modèle conceptuel, l’Association n’a pas besoin d’être mappée avec un élément **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="95933-174">Si un élément **AssociationSetMapping** est présent pour une association qui a une contrainte référentielle, les mappages définis dans l’élément **AssociationSetMapping** seront ignorés.</span><span class="sxs-lookup"><span data-stu-id="95933-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="95933-175">Pour plus d’informations, consultez ReferentialConstraint, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="95933-176">L’élément **AssociationSetMapping** peut avoir les éléments enfants suivants</span><span class="sxs-lookup"><span data-stu-id="95933-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="95933-177">QueryView (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="95933-178">EndProperty (zéro ou deux)</span><span class="sxs-lookup"><span data-stu-id="95933-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="95933-179">Condition (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="95933-180">ModificationFunctionMapping (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-181">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-181">Applicable Attributes</span></span>

<span data-ttu-id="95933-182">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="95933-183">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-183">Attribute Name</span></span>     | <span data-ttu-id="95933-184">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-184">Is Required</span></span> | <span data-ttu-id="95933-185">Value</span><span class="sxs-lookup"><span data-stu-id="95933-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-186">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-186">**Name**</span></span>           | <span data-ttu-id="95933-187">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-187">Yes</span></span>         | <span data-ttu-id="95933-188">Nom de l'ensemble d'associations du modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="95933-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="95933-189">**TypeName**</span></span>       | <span data-ttu-id="95933-190">Non</span><span class="sxs-lookup"><span data-stu-id="95933-190">No</span></span>          | <span data-ttu-id="95933-191">Nom qualifié par un espace de noms du type d'association du modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="95933-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="95933-192">**StoreEntitySet**</span></span> | <span data-ttu-id="95933-193">Non</span><span class="sxs-lookup"><span data-stu-id="95933-193">No</span></span>          | <span data-ttu-id="95933-194">Nom de la table mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="95933-195">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-195">Example</span></span>

<span data-ttu-id="95933-196">L’exemple suivant montre un élément **AssociationSetMapping** dans lequel l’Association **FK @ no__t-2Course @ no__t-3Department** définie dans le modèle conceptuel est mappée à la table **course** de la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="95933-197">Les mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans les éléments **EndProperty** enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="95933-198">ComplexProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="95933-199">Un élément **ComplexProperty** en Mapping Specification Language (MSL) définit le mappage entre une propriété de type complexe sur un type d’entité de modèle conceptuel et des colonnes de table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="95933-200">Les mappages de colonne-propriété sont spécifiés dans les éléments ScalarProperty enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="95933-201">L’élément de propriété **complexType** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-202">ScalarProperty (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="95933-203">**ComplexProperty** (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="95933-204">ComplextTypeMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="95933-205">Condition (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-206">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-206">Applicable Attributes</span></span>

<span data-ttu-id="95933-207">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="95933-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="95933-208">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-208">Attribute Name</span></span> | <span data-ttu-id="95933-209">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-209">Is Required</span></span> | <span data-ttu-id="95933-210">Value</span><span class="sxs-lookup"><span data-stu-id="95933-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-211">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-211">**Name**</span></span>       | <span data-ttu-id="95933-212">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-212">Yes</span></span>         | <span data-ttu-id="95933-213">Nom de la propriété complexe d'un type d'entité dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="95933-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="95933-214">**TypeName**</span></span>   | <span data-ttu-id="95933-215">Non</span><span class="sxs-lookup"><span data-stu-id="95933-215">No</span></span>          | <span data-ttu-id="95933-216">Nom qualifié par un espace de noms du type de propriété de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="95933-217">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-217">Example</span></span>

<span data-ttu-id="95933-218">L’exemple suivant est basé sur le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-218">The following example is based on the School model.</span></span> <span data-ttu-id="95933-219">Le type complexe suivant a été ajouté au modèle conceptuel :</span><span class="sxs-lookup"><span data-stu-id="95933-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="95933-220">Les propriétés **LastName** et **FirstName** du type d’entité **Person** ont été remplacées par une propriété complexe, **Name**:</span><span class="sxs-lookup"><span data-stu-id="95933-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="95933-221">Le MSL suivant montre l’élément **ComplexProperty** utilisé pour mapper la propriété **Name** aux colonnes de la base de données sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="95933-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="95933-222">ComplexTypeMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="95933-223">L’élément **ComplexTypeMapping,** en Mapping Specification Language (MSL) est un enfant de l’élément ResultMapping et définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans la base de données sous-jacente lorsque les éléments suivants sont vraies :</span><span class="sxs-lookup"><span data-stu-id="95933-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="95933-224">L'importation de fonction retourne un type complexe conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="95933-225">Les noms de colonne retournés par la procédure stockée ne correspondent pas exactement aux noms de propriété du type complexe.</span><span class="sxs-lookup"><span data-stu-id="95933-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="95933-226">Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type complexe est basé sur les noms de colonne et de propriété.</span><span class="sxs-lookup"><span data-stu-id="95933-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="95933-227">Si les noms de colonne ne correspondent pas exactement aux noms de propriété, vous devez utiliser l’élément **ComplexTypeMapping,** pour définir le mappage.</span><span class="sxs-lookup"><span data-stu-id="95933-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="95933-228">Pour obtenir un exemple du mappage par défaut, consultez élément FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="95933-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="95933-229">L’élément **ComplexTypeMapping,** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-230">ScalarProperty (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-231">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-231">Applicable Attributes</span></span>

<span data-ttu-id="95933-232">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **ComplexTypeMapping,** .</span><span class="sxs-lookup"><span data-stu-id="95933-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="95933-233">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-233">Attribute Name</span></span> | <span data-ttu-id="95933-234">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-234">Is Required</span></span> | <span data-ttu-id="95933-235">Value</span><span class="sxs-lookup"><span data-stu-id="95933-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="95933-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="95933-236">**TypeName**</span></span>   | <span data-ttu-id="95933-237">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-237">Yes</span></span>         | <span data-ttu-id="95933-238">Nom qualifié par un espace de noms du type complexe mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-239">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-239">Example</span></span>

<span data-ttu-id="95933-240">Considérons la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="95933-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="95933-241">De même, considérons le type complexe de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="95933-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="95933-242">Afin de créer une importation de fonction qui retourne des instances du type complexe précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un élément **ComplexTypeMapping,** :</span><span class="sxs-lookup"><span data-stu-id="95933-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="95933-243">Condition, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-243">Condition Element (MSL)</span></span>

<span data-ttu-id="95933-244">L’élément **condition** en Mapping Specification Language (MSL) place des conditions sur les mappages entre le modèle conceptuel et la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="95933-245">Le mappage défini dans un nœud XML est valide si toutes les conditions, comme spécifié dans les éléments de **condition** enfants, sont remplies.</span><span class="sxs-lookup"><span data-stu-id="95933-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="95933-246">À défaut, le mappage n'est pas valide.</span><span class="sxs-lookup"><span data-stu-id="95933-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="95933-247">Par exemple, si un élément MappingFragment contient un ou plusieurs éléments enfants **condition** , le mappage défini dans le nœud **MappingFragment** n’est valide que si toutes les conditions des éléments de **condition** enfants sont remplies.</span><span class="sxs-lookup"><span data-stu-id="95933-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="95933-248">Chaque condition peut s’appliquer à un **nom** (le nom d’une propriété d’entité de modèle conceptuel, spécifié par l’attribut **Name** ) ou à un **ColumnName** (le nom d’une colonne dans la base de données, spécifié par l’attribut **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="95933-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="95933-249">Lorsque l’attribut **Name** est défini, la condition est vérifiée par rapport à une valeur de propriété d’entité.</span><span class="sxs-lookup"><span data-stu-id="95933-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="95933-250">Lorsque l’attribut **ColumnName** est défini, la condition est vérifiée par rapport à une valeur de colonne.</span><span class="sxs-lookup"><span data-stu-id="95933-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="95933-251">Seul l’un des attributs **Name** ou **ColumnName** peut être spécifié dans un élément **condition** .</span><span class="sxs-lookup"><span data-stu-id="95933-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="95933-252">Lorsque l’élément **condition** est utilisé dans un élément FunctionImportMapping, seul l’attribut **Name** n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="95933-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="95933-253">L’élément **condition** peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="95933-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="95933-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="95933-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="95933-255">ComplexProperty</span></span>
-   <span data-ttu-id="95933-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="95933-256">EntitySetMapping</span></span>
-   <span data-ttu-id="95933-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="95933-257">MappingFragment</span></span>
-   <span data-ttu-id="95933-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="95933-258">EntityTypeMapping</span></span>

<span data-ttu-id="95933-259">L’élément **condition** ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-260">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-260">Applicable Attributes</span></span>

<span data-ttu-id="95933-261">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **condition** :</span><span class="sxs-lookup"><span data-stu-id="95933-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="95933-262">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-262">Attribute Name</span></span> | <span data-ttu-id="95933-263">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-263">Is Required</span></span> | <span data-ttu-id="95933-264">Value</span><span class="sxs-lookup"><span data-stu-id="95933-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="95933-265">**ColumnName**</span></span> | <span data-ttu-id="95933-266">Non</span><span class="sxs-lookup"><span data-stu-id="95933-266">No</span></span>          | <span data-ttu-id="95933-267">Nom de la colonne de table dont la valeur est utilisée pour évaluer la condition.</span><span class="sxs-lookup"><span data-stu-id="95933-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="95933-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="95933-268">**IsNull**</span></span>     | <span data-ttu-id="95933-269">Non</span><span class="sxs-lookup"><span data-stu-id="95933-269">No</span></span>          | <span data-ttu-id="95933-270">**True** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="95933-270">**True** or **False**.</span></span> <span data-ttu-id="95933-271">Si la valeur est **true** et que la valeur de la colonne est **null**, ou si la valeur est **false** et que la valeur de la colonne n’est pas **null**, la condition est true.</span><span class="sxs-lookup"><span data-stu-id="95933-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="95933-272">Sinon, la condition n'est pas vérifiée (False).</span><span class="sxs-lookup"><span data-stu-id="95933-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="95933-273">Les attributs **IsNull** et **value** ne peuvent pas être utilisés en même temps.</span><span class="sxs-lookup"><span data-stu-id="95933-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="95933-274">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="95933-274">**Value**</span></span>      | <span data-ttu-id="95933-275">Non</span><span class="sxs-lookup"><span data-stu-id="95933-275">No</span></span>          | <span data-ttu-id="95933-276">Valeur à laquelle la valeur de colonne est comparée.</span><span class="sxs-lookup"><span data-stu-id="95933-276">The value with which the column value is compared.</span></span> <span data-ttu-id="95933-277">Si les valeurs sont identiques, la condition est vérifiée (True).</span><span class="sxs-lookup"><span data-stu-id="95933-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="95933-278">Sinon, la condition n'est pas vérifiée (False).</span><span class="sxs-lookup"><span data-stu-id="95933-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="95933-279">Les attributs **IsNull** et **value** ne peuvent pas être utilisés en même temps.</span><span class="sxs-lookup"><span data-stu-id="95933-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="95933-280">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-280">**Name**</span></span>       | <span data-ttu-id="95933-281">Non</span><span class="sxs-lookup"><span data-stu-id="95933-281">No</span></span>          | <span data-ttu-id="95933-282">Nom de la propriété d'entité de modèle conceptuel dont la valeur est utilisée pour évaluer la condition.</span><span class="sxs-lookup"><span data-stu-id="95933-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="95933-283">Cet attribut n’est pas applicable si l’élément **condition** est utilisé dans un élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="95933-284">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-284">Example</span></span>

<span data-ttu-id="95933-285">L’exemple suivant montre des éléments de **condition** en tant qu’enfants d’éléments **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="95933-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="95933-286">Lorsque **HireDate** n’a pas la valeur null et que **EnrollmentDate** a la valeur null, les données sont mappées entre le type **SchoolModel. Instructor** et les colonnes **PersonID** et **HireDate** de la table **Person** .</span><span class="sxs-lookup"><span data-stu-id="95933-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="95933-287">Lorsque **EnrollmentDate** n’a pas la valeur null et **HireDate** a la valeur null, les données sont mappées entre le type **SchoolModel. Student** et les colonnes **PersonID** et **inscription** de la table **Person** .</span><span class="sxs-lookup"><span data-stu-id="95933-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="95933-288">DeleteFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="95933-289">L’élément **DeleteFunction** en Mapping Specification Language (MSL) mappe la fonction Delete d’un type d’entité ou d’une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="95933-290">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="95933-291">Pour plus d’informations, consultez Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="95933-292">Si vous ne mappez pas les trois opérations d’insertion, de mise à jour ou de suppression d’un type d’entité à des procédures stockées, les opérations non mappées échouent si elles sont exécutées au moment de l’exécution et qu’une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="95933-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="95933-293">Application de DeleteFunction à EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="95933-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="95933-294">Lorsqu’il est appliqué à l’élément EntityTypeMapping, l’élément **DeleteFunction** mappe la fonction Delete d’un type d’entité dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="95933-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="95933-295">L’élément **DeleteFunction** peut avoir les éléments enfants suivants lorsqu’il est appliqué à un élément **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="95933-296">AssociationEnd (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="95933-297">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="95933-298">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="95933-299">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-299">Applicable Attributes</span></span>

<span data-ttu-id="95933-300">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **DeleteFunction** lorsqu’il est appliqué à un élément **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="95933-301">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-301">Attribute Name</span></span>            | <span data-ttu-id="95933-302">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-302">Is Required</span></span> | <span data-ttu-id="95933-303">Value</span><span class="sxs-lookup"><span data-stu-id="95933-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-304">**FunctionName**</span></span>          | <span data-ttu-id="95933-305">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-305">Yes</span></span>         | <span data-ttu-id="95933-306">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="95933-307">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="95933-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="95933-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="95933-309">Non</span><span class="sxs-lookup"><span data-stu-id="95933-309">No</span></span>          | <span data-ttu-id="95933-310">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95933-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="95933-311">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-311">Example</span></span>

<span data-ttu-id="95933-312">L’exemple suivant est basé sur le modèle School et montre l’élément **DeleteFunction** qui mappe la fonction Delete du type d’entité **Person** à la procédure stockée **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="95933-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="95933-313">La procédure stockée **DeletePerson** est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="95933-314">Application de DeleteFunction à AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="95933-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="95933-315">Lorsqu’il est appliqué à l’élément AssociationSetMapping, l’élément **DeleteFunction** mappe la fonction Delete d’une association dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="95933-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="95933-316">L’élément **DeleteFunction** peut avoir les éléments enfants suivants lorsqu’il est appliqué à l’élément **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="95933-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="95933-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="95933-318">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-318">Applicable Attributes</span></span>

<span data-ttu-id="95933-319">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **DeleteFunction** lorsqu’il est appliqué à l’élément **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="95933-320">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-320">Attribute Name</span></span>            | <span data-ttu-id="95933-321">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-321">Is Required</span></span> | <span data-ttu-id="95933-322">Value</span><span class="sxs-lookup"><span data-stu-id="95933-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-323">**FunctionName**</span></span>          | <span data-ttu-id="95933-324">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-324">Yes</span></span>         | <span data-ttu-id="95933-325">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de suppression est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="95933-326">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="95933-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="95933-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="95933-328">Non</span><span class="sxs-lookup"><span data-stu-id="95933-328">No</span></span>          | <span data-ttu-id="95933-329">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95933-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="95933-330">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-330">Example</span></span>

<span data-ttu-id="95933-331">L’exemple suivant est basé sur le modèle School et montre l’élément **DeleteFunction** utilisé pour mapper la fonction Delete de l’Association **CourseInstructor** à la procédure stockée **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="95933-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="95933-332">La procédure stockée **DeleteCourseInstructor** est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="95933-333">EndProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="95933-334">L’élément **EndProperty** en Mapping Specification Language (MSL) définit le mappage entre une terminaison ou une fonction de modification d’une association de modèle conceptuel et la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="95933-335">Le mappage de colonne-propriété est spécifié dans un élément ScalarProperty enfant.</span><span class="sxs-lookup"><span data-stu-id="95933-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="95933-336">Lorsqu’un élément **EndProperty** est utilisé pour définir le mappage de la fin d’une association de modèle conceptuel, il est un enfant d’un élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="95933-337">Lorsque l’élément **EndProperty** est utilisé pour définir le mappage d’une fonction de modification d’une association de modèle conceptuel, il est un enfant d’un élément InsertFunction ou d’un élément DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="95933-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="95933-338">L’élément **EndProperty** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-339">ScalarProperty (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-340">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-340">Applicable Attributes</span></span>

<span data-ttu-id="95933-341">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="95933-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="95933-342">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-342">Attribute Name</span></span> | <span data-ttu-id="95933-343">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-343">Is Required</span></span> | <span data-ttu-id="95933-344">Value</span><span class="sxs-lookup"><span data-stu-id="95933-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="95933-345">Name</span><span class="sxs-lookup"><span data-stu-id="95933-345">Name</span></span>           | <span data-ttu-id="95933-346">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-346">Yes</span></span>         | <span data-ttu-id="95933-347">Nom de la terminaison d'association mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-348">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-348">Example</span></span>

<span data-ttu-id="95933-349">L’exemple suivant montre un élément **AssociationSetMapping** dans lequel l’Association **FK @ no__t-2Course @ no__t-3Department** dans le modèle conceptuel est mappée à la table **course** de la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="95933-350">Les mappages entre les propriétés de type d’association et les colonnes de table sont spécifiés dans les éléments **EndProperty** enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-351">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-351">Example</span></span>

<span data-ttu-id="95933-352">L’exemple suivant montre l’élément **EndProperty** qui mappe les fonctions d’insertion et de suppression d’une association (**CourseInstructor**) à des procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="95933-353">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="95933-354">EntityContainerMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="95933-355">L’élément **EntityContainerMapping** en Mapping Specification Language (MSL) mappe le conteneur d’entités du modèle conceptuel au conteneur d’entités dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="95933-356">L’élément **EntityContainerMapping** est un enfant de l’élément Mapping.</span><span class="sxs-lookup"><span data-stu-id="95933-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="95933-357">L’élément **EntityContainerMapping** peut avoir les éléments enfants suivants (dans l’ordre indiqué) :</span><span class="sxs-lookup"><span data-stu-id="95933-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="95933-358">EntitySetMapping (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="95933-359">AssociationSetMapping (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="95933-360">FunctionImportMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-361">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-361">Applicable Attributes</span></span>

<span data-ttu-id="95933-362">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="95933-363">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-363">Attribute Name</span></span>            | <span data-ttu-id="95933-364">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-364">Is Required</span></span> | <span data-ttu-id="95933-365">Value</span><span class="sxs-lookup"><span data-stu-id="95933-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="95933-366">**StorageModelContainer**</span></span> | <span data-ttu-id="95933-367">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-367">Yes</span></span>         | <span data-ttu-id="95933-368">Nom du conteneur d'entités de modèle de stockage mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="95933-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="95933-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="95933-370">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-370">Yes</span></span>         | <span data-ttu-id="95933-371">Nom du conteneur d'entités de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="95933-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="95933-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="95933-373">Non</span><span class="sxs-lookup"><span data-stu-id="95933-373">No</span></span>          | <span data-ttu-id="95933-374">**True** ou **false**.</span><span class="sxs-lookup"><span data-stu-id="95933-374">**True** or **False**.</span></span> <span data-ttu-id="95933-375">Si la **valeur est false**, aucune vue de mise à jour n’est générée.</span><span class="sxs-lookup"><span data-stu-id="95933-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="95933-376">Cet attribut doit avoir la valeur **false** lorsque vous avez un mappage en lecture seule qui serait non valide, car les données ne peuvent pas aller-retour.</span><span class="sxs-lookup"><span data-stu-id="95933-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="95933-377">La valeur par défaut est **true**.</span><span class="sxs-lookup"><span data-stu-id="95933-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-378">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-378">Example</span></span>

<span data-ttu-id="95933-379">L’exemple suivant montre un élément **EntityContainerMapping** qui mappe le conteneur **SchoolModelEntities** (le conteneur d’entités de modèle conceptuel) au conteneur **SchoolModelStoreContainer** (entité de modèle de stockage). conteneur) :</span><span class="sxs-lookup"><span data-stu-id="95933-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="95933-380">EntitySetMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="95933-381">L’élément **EntitySetMapping** en Mapping Specification Language (MSL) mappe tous les types dans un jeu d’entités de modèle conceptuel aux jeux d’entités dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="95933-382">Un jeu d’entités dans le modèle conceptuel est un conteneur logique pour les instances d’entités du même type (et les types dérivés).</span><span class="sxs-lookup"><span data-stu-id="95933-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="95933-383">Un jeu d’entités dans le modèle de stockage représente une table ou une vue dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="95933-384">Le jeu d’entités du modèle conceptuel est spécifié par la valeur de l’attribut **Name** de l’élément **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="95933-385">La table ou la vue mappée à est spécifiée par l’attribut **StoreEntitySet** dans chaque élément MappingFragment enfant ou dans l’élément **EntitySetMapping** lui-même.</span><span class="sxs-lookup"><span data-stu-id="95933-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="95933-386">L’élément **EntitySetMapping** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-387">EntityTypeMapping (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="95933-388">QueryView (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="95933-389">MappingFragment (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-390">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-390">Applicable Attributes</span></span>

<span data-ttu-id="95933-391">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="95933-392">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-392">Attribute Name</span></span>           | <span data-ttu-id="95933-393">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-393">Is Required</span></span> | <span data-ttu-id="95933-394">Value</span><span class="sxs-lookup"><span data-stu-id="95933-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-395">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-395">**Name**</span></span>                 | <span data-ttu-id="95933-396">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-396">Yes</span></span>         | <span data-ttu-id="95933-397">Nom du jeu d'entités de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="95933-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="95933-398">**TypeName** **1**</span></span>       | <span data-ttu-id="95933-399">Non</span><span class="sxs-lookup"><span data-stu-id="95933-399">No</span></span>          | <span data-ttu-id="95933-400">Nom du type d'entité de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="95933-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="95933-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="95933-402">Non</span><span class="sxs-lookup"><span data-stu-id="95933-402">No</span></span>          | <span data-ttu-id="95933-403">Nom du jeu d'entités de modèle de stockage de destination du mappage.</span><span class="sxs-lookup"><span data-stu-id="95933-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="95933-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="95933-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="95933-405">Non</span><span class="sxs-lookup"><span data-stu-id="95933-405">No</span></span>          | <span data-ttu-id="95933-406">**True** ou **false** selon que seules des lignes distinctes sont retournées.</span><span class="sxs-lookup"><span data-stu-id="95933-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="95933-407">Si cet attribut est défini sur **true**, l’attribut **GenerateUpdateViews** de l’élément EntityContainerMapping doit avoir la valeur **false**.</span><span class="sxs-lookup"><span data-stu-id="95933-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="95933-408">**1** les attributs **TypeName** et **StoreEntitySet** peuvent être utilisés à la place des éléments enfants EntityTypeMapping et MappingFragment pour mapper un type d’entité unique à une table unique.</span><span class="sxs-lookup"><span data-stu-id="95933-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="95933-409">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-409">Example</span></span>

<span data-ttu-id="95933-410">L’exemple suivant montre un élément **EntitySetMapping** qui mappe trois types (un type de base et deux types dérivés) dans le jeu d’entités **courses** du modèle conceptuel à trois tables différentes dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="95933-411">Les tables sont spécifiées par l’attribut **StoreEntitySet** dans chaque élément **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="95933-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="95933-412">EntityTypeMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="95933-413">L’élément **EntityTypeMapping** en Mapping Specification Language (MSL) définit le mappage entre un type d’entité dans le modèle conceptuel et les tables ou vues dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="95933-414">Pour plus d’informations sur les types d’entités de modèle conceptuel et les tables ou vues de base de données sous-jacentes, consultez élément EntityType (CSDL) et élément EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="95933-415">Le type d’entité de modèle conceptuel qui est mappé est spécifié par l’attribut **TypeName** de l’élément **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="95933-416">La table ou la vue mappée est spécifiée par l’attribut **StoreEntitySet** de l’élément MappingFragment enfant.</span><span class="sxs-lookup"><span data-stu-id="95933-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="95933-417">L’élément enfant ModificationFunctionMapping peut être utilisé pour mapper les fonctions d’insertion, de mise à jour ou de suppression des types d’entité aux procédures stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="95933-418">L’élément **EntityTypeMapping** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-419">MappingFragment (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="95933-420">ModificationFunctionMapping (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="95933-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="95933-421">ScalarProperty</span></span>
-   <span data-ttu-id="95933-422">Condition</span><span class="sxs-lookup"><span data-stu-id="95933-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="95933-423">Les éléments **MappingFragment** et **ModificationFunctionMapping** ne peuvent pas être des éléments enfants de l’élément **EntityTypeMapping** en même temps.</span><span class="sxs-lookup"><span data-stu-id="95933-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="95933-424">Les éléments **ScalarProperty** et **condition** ne peuvent être que des éléments enfants de l’élément **EntityTypeMapping** lorsqu’il est utilisé dans un élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-425">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-425">Applicable Attributes</span></span>

<span data-ttu-id="95933-426">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="95933-427">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-427">Attribute Name</span></span> | <span data-ttu-id="95933-428">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-428">Is Required</span></span> | <span data-ttu-id="95933-429">Value</span><span class="sxs-lookup"><span data-stu-id="95933-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="95933-430">**TypeName**</span></span>   | <span data-ttu-id="95933-431">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-431">Yes</span></span>         | <span data-ttu-id="95933-432">Nom qualifié par un espace de noms du type d'entité de modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="95933-433">Si le type correspond à un type abstrait ou dérivé, la valeur doit être `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="95933-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-434">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-434">Example</span></span>

<span data-ttu-id="95933-435">L’exemple suivant montre un élément EntitySetMapping avec deux éléments **EntityTypeMapping** enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="95933-436">Dans le premier élément **EntityTypeMapping** , le type d’entité **SchoolModel. Person** est mappé à la table **Person** .</span><span class="sxs-lookup"><span data-stu-id="95933-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="95933-437">Dans le deuxième élément **EntityTypeMapping** , la fonctionnalité de mise à jour du type **SchoolModel. Person** est mappée à une procédure stockée, **UpdatePerson**, dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-438">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-438">Example</span></span>

<span data-ttu-id="95933-439">L'exemple suivant illustre le mappage d'une hiérarchie de types dont le type racine est abstrait.</span><span class="sxs-lookup"><span data-stu-id="95933-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="95933-440">Notez l’utilisation de la syntaxe `IsOfType` pour les attributs **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="95933-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="95933-441">FunctionImportMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="95933-442">L’élément **FunctionImportMapping** en Mapping Specification Language (MSL) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée ou une fonction dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="95933-443">Les importations de fonction doivent être déclarées dans le modèle conceptuel et les procédures stockées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="95933-444">Pour plus d’informations, consultez FunctionImport, élément (CSDL) et Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="95933-445">Par défaut, si une importation de fonction retourne un type d'entité ou un type complexe de modèle conceptuel, les noms des colonnes retournés par la procédure stockée sous-jacente doivent correspondre exactement aux noms des propriétés sur le type de modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="95933-446">Si les noms de colonne ne correspondent pas exactement aux noms de propriété, le mappage doit être défini dans un élément ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="95933-447">L’élément **FunctionImportMapping** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-448">ResultMapping (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-449">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-449">Applicable Attributes</span></span>

<span data-ttu-id="95933-450">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="95933-451">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-451">Attribute Name</span></span>         | <span data-ttu-id="95933-452">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-452">Is Required</span></span> | <span data-ttu-id="95933-453">Value</span><span class="sxs-lookup"><span data-stu-id="95933-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="95933-454">**FunctionImportName**</span></span> | <span data-ttu-id="95933-455">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-455">Yes</span></span>         | <span data-ttu-id="95933-456">Nom de l'importation de fonction dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="95933-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-457">**FunctionName**</span></span>       | <span data-ttu-id="95933-458">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-458">Yes</span></span>         | <span data-ttu-id="95933-459">Nom qualifié par un espace de noms de la fonction dans le modèle de stockage mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-460">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-460">Example</span></span>

<span data-ttu-id="95933-461">L’exemple suivant est basé sur le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-461">The following example is based on the School model.</span></span> <span data-ttu-id="95933-462">Considérez la fonction suivante dans le modèle de stockage :</span><span class="sxs-lookup"><span data-stu-id="95933-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="95933-463">Considérez également cette importation de fonction dans le modèle conceptuel :</span><span class="sxs-lookup"><span data-stu-id="95933-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="95933-464">L’exemple suivant montre un élément **FunctionImportMapping** utilisé pour mapper la fonction et l’importation de fonction ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="95933-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="95933-465">InsertFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="95933-466">L’élément **InsertFunction** en Mapping Specification Language (MSL) mappe la fonction d’insertion d’un type d’entité ou d’une association dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="95933-467">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="95933-468">Pour plus d’informations, consultez Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="95933-469">Si vous ne mappez pas les trois opérations d’insertion, de mise à jour ou de suppression d’un type d’entité à des procédures stockées, les opérations non mappées échouent si elles sont exécutées au moment de l’exécution et qu’une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="95933-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="95933-470">L’élément **InsertFunction** peut être un enfant de l’élément ModificationFunctionMapping et être appliqué à l’élément EntityTypeMapping ou à l’élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="95933-471">Application d'InsertFunction à EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="95933-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="95933-472">Lorsqu’il est appliqué à l’élément EntityTypeMapping, l’élément **InsertFunction** mappe la fonction d’insertion d’un type d’entité dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="95933-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="95933-473">L’élément **InsertFunction** peut avoir les éléments enfants suivants lorsqu’il est appliqué à un élément **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="95933-474">AssociationEnd (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="95933-475">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="95933-476">ResultBinding (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="95933-477">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="95933-478">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-478">Applicable Attributes</span></span>

<span data-ttu-id="95933-479">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **InsertFunction** lorsqu’ils sont appliqués à un élément **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="95933-480">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-480">Attribute Name</span></span>            | <span data-ttu-id="95933-481">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-481">Is Required</span></span> | <span data-ttu-id="95933-482">Value</span><span class="sxs-lookup"><span data-stu-id="95933-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-483">**FunctionName**</span></span>          | <span data-ttu-id="95933-484">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-484">Yes</span></span>         | <span data-ttu-id="95933-485">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="95933-486">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="95933-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="95933-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="95933-488">Non</span><span class="sxs-lookup"><span data-stu-id="95933-488">No</span></span>          | <span data-ttu-id="95933-489">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95933-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="95933-490">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-490">Example</span></span>

<span data-ttu-id="95933-491">L’exemple suivant est basé sur le modèle School et montre l’élément **InsertFunction** utilisé pour mapper la fonction d’insertion du type d’entité Person à la procédure stockée **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="95933-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="95933-492">La procédure stockée **InsertPerson** est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="95933-493">Application d'InsertFunction à AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="95933-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="95933-494">Lorsqu’il est appliqué à l’élément AssociationSetMapping, l’élément **InsertFunction** mappe la fonction d’insertion d’une association dans le modèle conceptuel à une procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="95933-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="95933-495">L’élément **InsertFunction** peut avoir les éléments enfants suivants lorsqu’il est appliqué à l’élément **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="95933-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="95933-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="95933-497">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-497">Applicable Attributes</span></span>

<span data-ttu-id="95933-498">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **InsertFunction** lorsqu’il est appliqué à l’élément **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="95933-499">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-499">Attribute Name</span></span>            | <span data-ttu-id="95933-500">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-500">Is Required</span></span> | <span data-ttu-id="95933-501">Value</span><span class="sxs-lookup"><span data-stu-id="95933-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-502">**FunctionName**</span></span>          | <span data-ttu-id="95933-503">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-503">Yes</span></span>         | <span data-ttu-id="95933-504">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction d'insertion est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="95933-505">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="95933-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="95933-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="95933-507">Non</span><span class="sxs-lookup"><span data-stu-id="95933-507">No</span></span>          | <span data-ttu-id="95933-508">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95933-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="95933-509">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-509">Example</span></span>

<span data-ttu-id="95933-510">L’exemple suivant est basé sur le modèle School et montre l’élément **InsertFunction** utilisé pour mapper la fonction d’insertion de l’Association **CourseInstructor** à la procédure stockée **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="95933-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="95933-511">La procédure stockée **InsertCourseInstructor** est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="95933-512">Élément de mappage (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="95933-513">L’élément **Mapping** en Mapping Specification Language (MSL) contient des informations pour le mappage d’objets définis dans un modèle conceptuel à une base de données (comme décrit dans un modèle de stockage).</span><span class="sxs-lookup"><span data-stu-id="95933-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="95933-514">Pour plus d’informations, consultez Spécification CSDL et spécification SSDL.</span><span class="sxs-lookup"><span data-stu-id="95933-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="95933-515">L’élément **Mapping** est l’élément racine d’une spécification de mappage.</span><span class="sxs-lookup"><span data-stu-id="95933-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="95933-516">L’espace de noms XML pour les spécifications de mappage est https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="95933-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="95933-517">L'élément de mappage peut avoir les éléments enfants suivants (dans l'ordre répertorié) :</span><span class="sxs-lookup"><span data-stu-id="95933-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="95933-518">Alias (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="95933-519">EntityContainerMapping (exactement un)</span><span class="sxs-lookup"><span data-stu-id="95933-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="95933-520">Les noms de types de modèle conceptuel et de stockage référencés en MSL doivent être qualifiés par le nom de leur espace de noms respectif.</span><span class="sxs-lookup"><span data-stu-id="95933-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="95933-521">Pour plus d’informations sur le nom de l’espace de noms du modèle conceptuel, consultez Schema, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="95933-522">Pour plus d’informations sur le nom de l’espace de noms du modèle de stockage, consultez Schema, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="95933-523">Les alias pour les espaces de noms utilisés dans MSL peuvent être définis avec l’élément alias.</span><span class="sxs-lookup"><span data-stu-id="95933-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-524">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-524">Applicable Attributes</span></span>

<span data-ttu-id="95933-525">Le tableau ci-dessous décrit les attributs qui peuvent être appliqués à l’élément **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="95933-526">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-526">Attribute Name</span></span> | <span data-ttu-id="95933-527">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-527">Is Required</span></span> | <span data-ttu-id="95933-528">Value</span><span class="sxs-lookup"><span data-stu-id="95933-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="95933-529">**Espace**</span><span class="sxs-lookup"><span data-stu-id="95933-529">**Space**</span></span>      | <span data-ttu-id="95933-530">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-530">Yes</span></span>         | <span data-ttu-id="95933-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="95933-531">**C-S**.</span></span> <span data-ttu-id="95933-532">Il s'agit d'une valeur fixe qui ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="95933-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-533">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-533">Example</span></span>

<span data-ttu-id="95933-534">L’exemple suivant illustre un élément de **mappage** basé sur une partie du modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="95933-535">Pour plus d’informations sur le modèle School, consultez démarrage rapide (Entity Framework) :</span><span class="sxs-lookup"><span data-stu-id="95933-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="95933-536">MappingFragment, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="95933-537">L’élément **MappingFragment** en Mapping Specification Language (MSL) définit le mappage entre les propriétés d’un type d’entité de modèle conceptuel et une table ou une vue dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="95933-538">Pour plus d’informations sur les types d’entités de modèle conceptuel et les tables ou vues de base de données sous-jacentes, consultez élément EntityType (CSDL) et élément EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="95933-539">L’élément **MappingFragment** peut être un élément enfant de l’élément EntityTypeMapping ou de l’élément EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="95933-540">L’élément **MappingFragment** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-541">ComplexType (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="95933-542">ScalarProperty (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="95933-543">Condition (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-544">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-544">Applicable Attributes</span></span>

<span data-ttu-id="95933-545">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="95933-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="95933-546">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-546">Attribute Name</span></span>          | <span data-ttu-id="95933-547">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-547">Is Required</span></span> | <span data-ttu-id="95933-548">Value</span><span class="sxs-lookup"><span data-stu-id="95933-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="95933-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="95933-550">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-550">Yes</span></span>         | <span data-ttu-id="95933-551">Nom de la table ou de la vue mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="95933-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="95933-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="95933-553">Non</span><span class="sxs-lookup"><span data-stu-id="95933-553">No</span></span>          | <span data-ttu-id="95933-554">**True** ou **false** selon que seules des lignes distinctes sont retournées.</span><span class="sxs-lookup"><span data-stu-id="95933-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="95933-555">Si cet attribut est défini sur **true**, l’attribut **GenerateUpdateViews** de l’élément EntityContainerMapping doit avoir la valeur **false**.</span><span class="sxs-lookup"><span data-stu-id="95933-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-556">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-556">Example</span></span>

<span data-ttu-id="95933-557">L’exemple suivant montre un élément **MappingFragment** en tant qu’enfant d’un élément **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="95933-558">Dans cet exemple, les propriétés du type de **cours** dans le modèle conceptuel sont mappées aux colonnes de la table **course** dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-559">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-559">Example</span></span>

<span data-ttu-id="95933-560">L’exemple suivant montre un élément **MappingFragment** en tant qu’enfant d’un élément **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="95933-561">Comme dans l’exemple ci-dessus, les propriétés du type de **cours** dans le modèle conceptuel sont mappées aux colonnes de la table **course** dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="95933-562">ModificationFunctionMapping, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="95933-563">L’élément **ModificationFunctionMapping** en Mapping Specification Language (MSL) mappe les fonctions d’insertion, de mise à jour et de suppression d’un type d’entité de modèle conceptuel aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="95933-564">L’élément **ModificationFunctionMapping** peut également mapper les fonctions d’insertion et de suppression pour les associations plusieurs-à-plusieurs dans le modèle conceptuel aux procédures stockées dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="95933-565">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="95933-566">Pour plus d’informations, consultez Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="95933-567">Si vous ne mappez pas les trois opérations d’insertion, de mise à jour ou de suppression d’un type d’entité à des procédures stockées, les opérations non mappées échouent si elles sont exécutées au moment de l’exécution et qu’une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="95933-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="95933-568">Si les fonctions de modification pour une entité dans une hiérarchie d'héritage sont mappées aux procédures stockées, les fonctions de modification de tous les types dans la hiérarchie doivent être mappées aux procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="95933-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="95933-569">L’élément **ModificationFunctionMapping** peut être un enfant de l’élément EntityTypeMapping ou de l’élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="95933-570">L’élément **ModificationFunctionMapping** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-571">DeleteFunction (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="95933-572">InsertFunction (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="95933-573">UpdateFunction (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="95933-574">Aucun attribut n’est applicable à l’élément **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="95933-575">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-575">Example</span></span>

<span data-ttu-id="95933-576">L’exemple suivant montre le mappage de jeu d’entités pour le jeu d’entités **People** dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="95933-577">En plus du mappage de colonnes pour le type d’entité **Person** , le mappage des fonctions d’insertion, de mise à jour et de suppression du type **Person** est affiché.</span><span class="sxs-lookup"><span data-stu-id="95933-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="95933-578">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-579">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-579">Example</span></span>

<span data-ttu-id="95933-580">L’exemple suivant montre le mappage de l’ensemble d’associations pour l’ensemble d’associations **CourseInstructor** dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="95933-581">En plus du mappage de colonnes pour l’Association **CourseInstructor** , le mappage des fonctions d’insertion et de suppression de l’Association **CourseInstructor** est affiché.</span><span class="sxs-lookup"><span data-stu-id="95933-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="95933-582">Les fonctions mappées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="95933-583">QueryView, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="95933-584">L’élément **QueryView** en Mapping Specification Language (MSL) définit un mappage en lecture seule entre un type d’entité ou une association dans le modèle conceptuel et une table dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="95933-585">Le mappage est défini avec une requête Entity SQL qui est évaluée par rapport au modèle de stockage, et vous exprimez le jeu de résultats en termes d’entité ou d’association dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="95933-586">Les affichages des requêtes étant en lecture seule, les types qu'ils définissent ne peuvent pas être mis à jour au moyen des commandes de mise à jour standard.</span><span class="sxs-lookup"><span data-stu-id="95933-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="95933-587">Les mises à jour de ces types peuvent être effectuées au moyen de fonctions de modification.</span><span class="sxs-lookup"><span data-stu-id="95933-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="95933-588">Pour plus d'informations, consultez Guide pratique pour Mapper des fonctions de modification à des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="95933-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="95933-589">Dans l’élément **QueryView** , Entity SQL expressions qui contiennent des propriétés **GroupBy**, Group ou de navigation ne sont pas prises en charge.</span><span class="sxs-lookup"><span data-stu-id="95933-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="95933-590">L’élément **QueryView** peut être un enfant de l’élément EntitySetMapping ou de l’élément AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="95933-591">Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une entité dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="95933-592">Dans le cas précédent, l'affichage des requêtes définit un mappage en lecture seule pour une association dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="95933-593">Si l’élément **AssociationSetMapping** est destiné à une association avec une contrainte référentielle, l’élément **AssociationSetMapping** est ignoré.</span><span class="sxs-lookup"><span data-stu-id="95933-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="95933-594">Pour plus d’informations, consultez ReferentialConstraint, élément (CSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="95933-595">L’élément **QueryView** ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-596">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-596">Applicable Attributes</span></span>

<span data-ttu-id="95933-597">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="95933-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="95933-598">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-598">Attribute Name</span></span> | <span data-ttu-id="95933-599">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-599">Is Required</span></span> | <span data-ttu-id="95933-600">Value</span><span class="sxs-lookup"><span data-stu-id="95933-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="95933-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="95933-601">**TypeName**</span></span>   | <span data-ttu-id="95933-602">Non</span><span class="sxs-lookup"><span data-stu-id="95933-602">No</span></span>          | <span data-ttu-id="95933-603">Nom du type de modèle conceptuel mappé par l'affichage des requêtes.</span><span class="sxs-lookup"><span data-stu-id="95933-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-604">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-604">Example</span></span>

<span data-ttu-id="95933-605">L’exemple suivant montre l’élément **QueryView** en tant qu’enfant de l’élément **EntitySetMapping** et définit un mappage d’affichage des requêtes pour le type d’entité **Department** dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="95933-606">Étant donné que la requête retourne uniquement un sous-ensemble des membres du type de **service** dans le modèle de stockage, le type de **service** du modèle School a été modifié en fonction de ce mappage comme suit :</span><span class="sxs-lookup"><span data-stu-id="95933-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-607">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-607">Example</span></span>

<span data-ttu-id="95933-608">L’exemple suivant montre l’élément **QueryView** en tant qu’enfant d’un élément **AssociationSetMapping** et définit un mappage en lecture seule pour l’Association `FK_Course_Department` dans le modèle School.</span><span class="sxs-lookup"><span data-stu-id="95933-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="95933-609">Commentaires</span><span class="sxs-lookup"><span data-stu-id="95933-609">Comments</span></span>

<span data-ttu-id="95933-610">Vous pouvez définir des affichages des requêtes pour activer les scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="95933-611">Définir une entité du modèle conceptuel qui n'inclut pas toutes les propriétés de l'entité dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="95933-612">Cela comprend les propriétés qui n’ont pas de valeurs par défaut et qui ne prennent pas en charge les valeurs **null** .</span><span class="sxs-lookup"><span data-stu-id="95933-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="95933-613">Mapper des colonnes calculées du modèle de stockage aux propriétés de types d'entités du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="95933-614">Définir un mappage dans lequel les conditions utilisées pour partitionner des entités du modèle conceptuel ne sont pas basées sur l'égalité.</span><span class="sxs-lookup"><span data-stu-id="95933-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="95933-615">Lorsque vous spécifiez un mappage conditionnel à l’aide de l’élément **condition** , la condition fournie doit être égale à la valeur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="95933-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="95933-616">Pour plus d’informations, consultez élément condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="95933-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="95933-617">Mapper la même colonne du modèle de stockage à plusieurs types du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="95933-618">Mapper plusieurs types à la même table.</span><span class="sxs-lookup"><span data-stu-id="95933-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="95933-619">Définir des associations dans le modèle conceptuel qui ne sont pas basées sur des clés étrangères du schéma relationnel.</span><span class="sxs-lookup"><span data-stu-id="95933-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="95933-620">Utiliser une logique métier personnalisée pour définir la valeur de propriétés du modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="95933-621">Par exemple, vous pouvez mapper la valeur de chaîne « T » dans la source de données à la valeur **true**, une valeur booléenne, dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="95933-622">Définir des filtres conditionnels pour les résultats de la requête.</span><span class="sxs-lookup"><span data-stu-id="95933-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="95933-623">Appliquer moins de restrictions sur les données dans le modèle conceptuel que dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="95933-624">Par exemple, vous pouvez rendre une propriété dans le modèle conceptuel Nullable même si la colonne à laquelle elle est mappée ne prend pas en charge les valeurs **null**.</span><span class="sxs-lookup"><span data-stu-id="95933-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="95933-625">Vous devez tenir compte des points suivants lorsque vous définissez des affichages des requêtes pour les entités :</span><span class="sxs-lookup"><span data-stu-id="95933-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="95933-626">Les affichages des requêtes sont en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="95933-626">Query views are read-only.</span></span> <span data-ttu-id="95933-627">Les mises à jour des entités ne peuvent être effectuées qu'au moyen de fonctions de modification.</span><span class="sxs-lookup"><span data-stu-id="95933-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="95933-628">Lorsque vous définissez un type d'entité par un affichage des requêtes, vous devez également définir toutes les entités associées par les affichages des requêtes.</span><span class="sxs-lookup"><span data-stu-id="95933-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="95933-629">Lorsque vous mappez une association plusieurs-à-plusieurs à une entité dans le modèle de stockage qui représente une table de liens dans le schéma relationnel, vous devez définir un élément **QueryView** dans l’élément **AssociationSetMapping** pour cette table de liens.</span><span class="sxs-lookup"><span data-stu-id="95933-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="95933-630">Les affichages des requêtes doivent être définis pour tous les types d'une hiérarchie des types.</span><span class="sxs-lookup"><span data-stu-id="95933-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="95933-631">Pour ce faire, vous pouvez procéder de différentes façons :</span><span class="sxs-lookup"><span data-stu-id="95933-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="95933-632">Avec un seul élément **QueryView** qui spécifie une seule requête Entity SQL qui retourne une Union de tous les types d’entité dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="95933-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="95933-633">Avec un seul élément **QueryView** qui spécifie une requête Entity SQL unique qui utilise l’opérateur case pour retourner un type d’entité spécifique dans la hiérarchie en fonction d’une condition spécifique.</span><span class="sxs-lookup"><span data-stu-id="95933-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="95933-634">Avec un élément **QueryView** supplémentaire pour un type spécifique dans la hiérarchie.</span><span class="sxs-lookup"><span data-stu-id="95933-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="95933-635">Dans ce cas, utilisez l’attribut **TypeName** de l’élément **QueryView** pour spécifier le type d’entité pour chaque vue.</span><span class="sxs-lookup"><span data-stu-id="95933-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="95933-636">Quand une vue de requête est définie, vous ne pouvez pas spécifier l’attribut **StorageSetName** sur l’élément **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="95933-637">Quand une vue de requête est définie, l’élément **EntitySetMapping**ne peut pas contenir également des mappages de **Propriétés** .</span><span class="sxs-lookup"><span data-stu-id="95933-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="95933-638">Élément ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="95933-639">L’élément **ResultBinding** en Mapping Specification Language (MSL) mappe les valeurs de colonne retournées par les procédures stockées aux propriétés d’entité dans le modèle conceptuel lorsque les fonctions de modification de type d’entité sont mappées aux procédures stockées dans le base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="95933-640">Par exemple, lorsque la valeur d’une colonne d’identité est retournée par une procédure stockée Insert, l’élément **ResultBinding** mappe la valeur retournée à une propriété de type d’entité dans le modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="95933-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="95933-641">L’élément **ResultBinding** peut être un enfant de l’élément InsertFunction ou de l’élément UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="95933-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="95933-642">L’élément **ResultBinding** ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-643">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-643">Applicable Attributes</span></span>

<span data-ttu-id="95933-644">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **ResultBinding** :</span><span class="sxs-lookup"><span data-stu-id="95933-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="95933-645">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-645">Attribute Name</span></span> | <span data-ttu-id="95933-646">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-646">Is Required</span></span> | <span data-ttu-id="95933-647">Value</span><span class="sxs-lookup"><span data-stu-id="95933-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="95933-648">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-648">**Name**</span></span>       | <span data-ttu-id="95933-649">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-649">Yes</span></span>         | <span data-ttu-id="95933-650">Nom de la propriété d'entité dans le modèle conceptuel mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="95933-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="95933-651">**ColumnName**</span></span> | <span data-ttu-id="95933-652">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-652">Yes</span></span>         | <span data-ttu-id="95933-653">Nom de la colonne mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="95933-654">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-654">Example</span></span>

<span data-ttu-id="95933-655">L’exemple suivant est basé sur le modèle School et montre un élément **InsertFunction** utilisé pour mapper la fonction insert du type d’entité **Person** à la procédure stockée **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="95933-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="95933-656">(La procédure stockée **InsertPerson** est indiquée ci-dessous et est déclarée dans le modèle de stockage.) Un élément **ResultBinding** est utilisé pour mapper une valeur de colonne retournée par la procédure stockée (**NewPersonID**) à une propriété de type d’entité (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="95933-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="95933-657">Le code Transact-SQL suivant décrit la procédure stockée **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="95933-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="95933-658">Élément ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="95933-659">L’élément **ResultMapping** en Mapping Specification Language (MSL) définit le mappage entre une importation de fonction dans le modèle conceptuel et une procédure stockée dans la base de données sous-jacente lorsque les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="95933-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="95933-660">L'importation de fonction retourne un type d'entité de modèle conceptuel ou le type complexe.</span><span class="sxs-lookup"><span data-stu-id="95933-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="95933-661">Les noms des colonnes retournés par la procédure stockée ne correspondent pas exactement aux noms des propriétés sur le type d'entité ou le type complexe.</span><span class="sxs-lookup"><span data-stu-id="95933-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="95933-662">Par défaut, le mappage entre les colonnes retournées par une procédure stockée et un type d'entité ou un type complexe est basé sur les noms de colonne et de propriété.</span><span class="sxs-lookup"><span data-stu-id="95933-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="95933-663">Si les noms de colonne ne correspondent pas exactement aux noms de propriété, vous devez utiliser l’élément **ResultMapping** pour définir le mappage.</span><span class="sxs-lookup"><span data-stu-id="95933-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="95933-664">Pour obtenir un exemple du mappage par défaut, consultez élément FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="95933-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="95933-665">L’élément **ResultMapping** est un élément enfant de l’élément FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="95933-666">L’élément **ResultMapping** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-667">EntityTypeMapping (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="95933-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="95933-668">ComplexTypeMapping</span></span>

<span data-ttu-id="95933-669">Aucun attribut n’est applicable à l’élément **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="95933-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="95933-670">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-670">Example</span></span>

<span data-ttu-id="95933-671">Considérons la procédure stockée suivante :</span><span class="sxs-lookup"><span data-stu-id="95933-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="95933-672">De même, considérons le type d'entité de modèle conceptuel suivant :</span><span class="sxs-lookup"><span data-stu-id="95933-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="95933-673">Afin de créer une importation de fonction qui retourne des instances du type d’entité précédent, le mappage entre les colonnes retournées par la procédure stockée et le type d’entité doit être défini dans un élément **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="95933-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="95933-674">ScalarProperty, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="95933-675">L’élément **ScalarProperty** en Mapping Specification Language (MSL) mappe une propriété sur un type d’entité de modèle conceptuel, un type complexe ou une association à une colonne de table ou un paramètre de procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="95933-676">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="95933-677">Pour plus d’informations, consultez Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="95933-678">L’élément **ScalarProperty** peut être un enfant des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="95933-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="95933-679">MappingFragment</span></span>
-   <span data-ttu-id="95933-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="95933-680">InsertFunction</span></span>
-   <span data-ttu-id="95933-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="95933-681">UpdateFunction</span></span>
-   <span data-ttu-id="95933-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="95933-682">DeleteFunction</span></span>
-   <span data-ttu-id="95933-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="95933-683">EndProperty</span></span>
-   <span data-ttu-id="95933-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="95933-684">ComplexProperty</span></span>
-   <span data-ttu-id="95933-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="95933-685">ResultMapping</span></span>

<span data-ttu-id="95933-686">En tant qu’enfant de l’élément **MappingFragment**, **ComplexProperty**ou **EndProperty** , l’élément **ScalarProperty** mappe une propriété du modèle conceptuel à une colonne de la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="95933-687">En tant qu’enfant de l’élément **InsertFunction**, **UpdateFunction**ou **DeleteFunction** , l’élément **ScalarProperty** mappe une propriété du modèle conceptuel à un paramètre de procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="95933-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="95933-688">L’élément **ScalarProperty** ne peut pas avoir d’éléments enfants.</span><span class="sxs-lookup"><span data-stu-id="95933-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-689">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-689">Applicable Attributes</span></span>

<span data-ttu-id="95933-690">Les attributs qui s’appliquent à l’élément **ScalarProperty** varient en fonction du rôle de l’élément.</span><span class="sxs-lookup"><span data-stu-id="95933-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="95933-691">Le tableau suivant décrit les attributs qui s’appliquent lorsque l’élément **ScalarProperty** est utilisé pour mapper une propriété de modèle conceptuel à une colonne de la base de données :</span><span class="sxs-lookup"><span data-stu-id="95933-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="95933-692">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-692">Attribute Name</span></span> | <span data-ttu-id="95933-693">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-693">Is Required</span></span> | <span data-ttu-id="95933-694">Value</span><span class="sxs-lookup"><span data-stu-id="95933-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="95933-695">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-695">**Name**</span></span>       | <span data-ttu-id="95933-696">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-696">Yes</span></span>         | <span data-ttu-id="95933-697">Nom de la propriété de modèle conceptuel mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="95933-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="95933-698">**ColumnName**</span></span> | <span data-ttu-id="95933-699">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-699">Yes</span></span>         | <span data-ttu-id="95933-700">Nom de la colonne de table mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="95933-701">Le tableau suivant décrit les attributs qui s’appliquent à l’élément **ScalarProperty** lorsqu’il est utilisé pour mapper une propriété de modèle conceptuel à un paramètre de procédure stockée :</span><span class="sxs-lookup"><span data-stu-id="95933-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="95933-702">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-702">Attribute Name</span></span>    | <span data-ttu-id="95933-703">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-703">Is Required</span></span> | <span data-ttu-id="95933-704">Value</span><span class="sxs-lookup"><span data-stu-id="95933-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-705">**Nom**</span><span class="sxs-lookup"><span data-stu-id="95933-705">**Name**</span></span>          | <span data-ttu-id="95933-706">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-706">Yes</span></span>         | <span data-ttu-id="95933-707">Nom de la propriété de modèle conceptuel mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="95933-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="95933-708">**ParameterName**</span></span> | <span data-ttu-id="95933-709">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-709">Yes</span></span>         | <span data-ttu-id="95933-710">Nom du paramètre mappé.</span><span class="sxs-lookup"><span data-stu-id="95933-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="95933-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="95933-711">**Version**</span></span>       | <span data-ttu-id="95933-712">Non</span><span class="sxs-lookup"><span data-stu-id="95933-712">No</span></span>          | <span data-ttu-id="95933-713">**Actuel** ou **original** selon que la valeur actuelle ou la valeur d’origine de la propriété doit être utilisée pour les contrôles d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="95933-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="95933-714">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-714">Example</span></span>

<span data-ttu-id="95933-715">L’exemple suivant montre l’élément **ScalarProperty** utilisé de deux manières :</span><span class="sxs-lookup"><span data-stu-id="95933-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="95933-716">Pour mapper les propriétés du type d’entité **Person** aux colonnes de la table **Person**.</span><span class="sxs-lookup"><span data-stu-id="95933-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="95933-717">Pour mapper les propriétés du type d’entité **Person** aux paramètres de la procédure stockée **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="95933-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="95933-718">Les procédures stockées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="95933-719">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-719">Example</span></span>

<span data-ttu-id="95933-720">L’exemple suivant montre l’élément **ScalarProperty** utilisé pour mapper les fonctions d’insertion et de suppression d’une association de modèle conceptuel à des procédures stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="95933-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="95933-721">Les procédures stockées sont déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="95933-722">UpdateFunction, élément (MSL)</span><span class="sxs-lookup"><span data-stu-id="95933-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="95933-723">L’élément **UpdateFunction** en Mapping Specification Language (MSL) mappe la fonction de mise à jour d’un type d’entité dans le modèle conceptuel à une procédure stockée dans la base de données sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="95933-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="95933-724">Les procédures stockées auxquelles des fonctions de modification sont mappées doivent être déclarées dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="95933-725">Pour plus d’informations, consultez Function, élément (SSDL).</span><span class="sxs-lookup"><span data-stu-id="95933-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="95933-726">Si vous ne mappez pas les trois opérations d’insertion, de mise à jour ou de suppression d’un type d’entité à des procédures stockées, les opérations non mappées échouent si elles sont exécutées au moment de l’exécution et qu’une UpdateException est levée.</span><span class="sxs-lookup"><span data-stu-id="95933-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="95933-727">L’élément **UpdateFunction** peut être un enfant de l’élément ModificationFunctionMapping et être appliqué à l’élément EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="95933-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="95933-728">L’élément **UpdateFunction** peut avoir les éléments enfants suivants :</span><span class="sxs-lookup"><span data-stu-id="95933-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="95933-729">AssociationEnd (zéro, un ou plusieurs)</span><span class="sxs-lookup"><span data-stu-id="95933-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="95933-730">ComplexProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="95933-731">ResultBinding (zéro ou un)</span><span class="sxs-lookup"><span data-stu-id="95933-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="95933-732">ScalarProperty (zéro ou plus)</span><span class="sxs-lookup"><span data-stu-id="95933-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="95933-733">Attributs applicables</span><span class="sxs-lookup"><span data-stu-id="95933-733">Applicable Attributes</span></span>

<span data-ttu-id="95933-734">Le tableau suivant décrit les attributs qui peuvent être appliqués à l’élément **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="95933-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="95933-735">Nom d'attribut</span><span class="sxs-lookup"><span data-stu-id="95933-735">Attribute Name</span></span>            | <span data-ttu-id="95933-736">Requis</span><span class="sxs-lookup"><span data-stu-id="95933-736">Is Required</span></span> | <span data-ttu-id="95933-737">Value</span><span class="sxs-lookup"><span data-stu-id="95933-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95933-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="95933-738">**FunctionName**</span></span>          | <span data-ttu-id="95933-739">Oui</span><span class="sxs-lookup"><span data-stu-id="95933-739">Yes</span></span>         | <span data-ttu-id="95933-740">Nom qualifié par un espace de noms de la procédure stockée à laquelle la fonction de mise à jour est mappée.</span><span class="sxs-lookup"><span data-stu-id="95933-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="95933-741">La procédure stockée doit être déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="95933-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="95933-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="95933-743">Non</span><span class="sxs-lookup"><span data-stu-id="95933-743">No</span></span>          | <span data-ttu-id="95933-744">Nom du paramètre de sortie qui retourne le nombre de lignes affectées.</span><span class="sxs-lookup"><span data-stu-id="95933-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="95933-745">Exemple</span><span class="sxs-lookup"><span data-stu-id="95933-745">Example</span></span>

<span data-ttu-id="95933-746">L’exemple suivant est basé sur le modèle School et montre l’élément **UpdateFunction** utilisé pour mapper la fonction de mise à jour du type d’entité **Person** à la procédure stockée **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="95933-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="95933-747">La procédure stockée **UpdatePerson** est déclarée dans le modèle de stockage.</span><span class="sxs-lookup"><span data-stu-id="95933-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
