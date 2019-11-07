---
title: Glossaire Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656157"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="dbd16-102">Glossaire Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dbd16-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="dbd16-103">Code First</span><span class="sxs-lookup"><span data-stu-id="dbd16-103">Code First</span></span>
<span data-ttu-id="dbd16-104">Création d’un modèle de Entity Framework à l’aide de code.</span><span class="sxs-lookup"><span data-stu-id="dbd16-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="dbd16-105">Le modèle peut cibler une base de données existante ou une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="dbd16-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="dbd16-106">Contexte</span><span class="sxs-lookup"><span data-stu-id="dbd16-106">Context</span></span>
<span data-ttu-id="dbd16-107">Classe qui représente une session avec la base de données, ce qui vous permet d’interroger et d’enregistrer des données.</span><span class="sxs-lookup"><span data-stu-id="dbd16-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="dbd16-108">Un contexte dérive de la classe DbContext ou ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="dbd16-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="dbd16-109">Convention (Code First)</span><span class="sxs-lookup"><span data-stu-id="dbd16-109">Convention (Code First)</span></span>
<span data-ttu-id="dbd16-110">Règle que Entity Framework utilise pour déduire la forme de votre modèle à partir de vos classes.</span><span class="sxs-lookup"><span data-stu-id="dbd16-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="dbd16-111">Database First</span><span class="sxs-lookup"><span data-stu-id="dbd16-111">Database First</span></span>
<span data-ttu-id="dbd16-112">Création d’un modèle de Entity Framework, à l’aide du concepteur EF, ciblant une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="dbd16-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="dbd16-113">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="dbd16-113">Eager loading</span></span>
<span data-ttu-id="dbd16-114">Modèle de chargement de données associées dans laquelle une requête pour un type d’entité charge également des entités associées dans le cadre de la requête.</span><span class="sxs-lookup"><span data-stu-id="dbd16-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="dbd16-115">EF Designer</span><span class="sxs-lookup"><span data-stu-id="dbd16-115">EF Designer</span></span>
<span data-ttu-id="dbd16-116">Concepteur visuel dans Visual Studio qui vous permet de créer un modèle de Entity Framework à l’aide de zones et de lignes.</span><span class="sxs-lookup"><span data-stu-id="dbd16-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="dbd16-117">Entité</span><span class="sxs-lookup"><span data-stu-id="dbd16-117">Entity</span></span>
<span data-ttu-id="dbd16-118">Classe ou objet qui représente des données d'application, telles que des clients, des produits et des commandes.</span><span class="sxs-lookup"><span data-stu-id="dbd16-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="dbd16-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="dbd16-119">Entity Data Model</span></span>
<span data-ttu-id="dbd16-120">Modèle qui décrit les entités et les relations entre eux.</span><span class="sxs-lookup"><span data-stu-id="dbd16-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="dbd16-121">EF utilise le modèle EDM pour décrire le modèle conceptuel sur lequel les programmes de développement sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="dbd16-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="dbd16-122">EDM s’appuie sur le modèle de relation d’entité introduit par Dr. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="dbd16-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="dbd16-123">L’EDM a été développé à l’origine avec l’objectif principal de devenir le modèle de données commun à travers une suite de technologies de développement et de serveur de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dbd16-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="dbd16-124">Le modèle EDM est également utilisé dans le cadre du protocole OData.</span><span class="sxs-lookup"><span data-stu-id="dbd16-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="dbd16-125">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="dbd16-125">Explicit loading</span></span>
<span data-ttu-id="dbd16-126">Modèle de chargement de données associées où les objets connexes sont chargés en appelant une API.</span><span class="sxs-lookup"><span data-stu-id="dbd16-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dbd16-127">API Fluent</span><span class="sxs-lookup"><span data-stu-id="dbd16-127">Fluent API</span></span>
<span data-ttu-id="dbd16-128">API qui peut être utilisée pour configurer un modèle de Code First.</span><span class="sxs-lookup"><span data-stu-id="dbd16-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="dbd16-129">Association de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="dbd16-129">Foreign key association</span></span>
<span data-ttu-id="dbd16-130">Association entre des entités où une propriété qui représente la clé étrangère est incluse dans la classe de l’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="dbd16-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="dbd16-131">Par exemple, Product contient une propriété CategoryId.</span><span class="sxs-lookup"><span data-stu-id="dbd16-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="dbd16-132">Relation d’identification</span><span class="sxs-lookup"><span data-stu-id="dbd16-132">Identifying relationship</span></span>
<span data-ttu-id="dbd16-133">Relation où la clé primaire de l'entité principale fait également partie de la clé primaire de l'entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="dbd16-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="dbd16-134">Dans ce type de relation, l'entité dépendante ne peut pas exister dans l'entité principale.</span><span class="sxs-lookup"><span data-stu-id="dbd16-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="dbd16-135">Association indépendante</span><span class="sxs-lookup"><span data-stu-id="dbd16-135">Independent association</span></span>
<span data-ttu-id="dbd16-136">Association entre des entités où il n’y a aucune propriété représentant la clé étrangère dans la classe de l’entité dépendante.</span><span class="sxs-lookup"><span data-stu-id="dbd16-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="dbd16-137">Par exemple, une classe Product contient une relation à Category, mais aucune propriété CategoryId.</span><span class="sxs-lookup"><span data-stu-id="dbd16-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="dbd16-138">Entity Framework effectue le suivi de l’état de l’Association indépendamment de l’état des entités aux terminaisons des deux associations.</span><span class="sxs-lookup"><span data-stu-id="dbd16-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="dbd16-139">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="dbd16-139">Lazy loading</span></span>
<span data-ttu-id="dbd16-140">Modèle de chargement de données associées où les objets connexes sont chargés automatiquement lors de l’accès à une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="dbd16-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="dbd16-141">Model First</span><span class="sxs-lookup"><span data-stu-id="dbd16-141">Model First</span></span>
<span data-ttu-id="dbd16-142">Création d’un modèle de Entity Framework à l’aide du concepteur EF, qui est ensuite utilisé pour créer une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="dbd16-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="dbd16-143">Propriété de navigation</span><span class="sxs-lookup"><span data-stu-id="dbd16-143">Navigation property</span></span>
<span data-ttu-id="dbd16-144">Propriété d’une entité qui référence une autre entité.</span><span class="sxs-lookup"><span data-stu-id="dbd16-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="dbd16-145">Par exemple, Product contient une propriété de navigation Category et Category contient une propriété de navigation Products.</span><span class="sxs-lookup"><span data-stu-id="dbd16-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="dbd16-146">POCO</span><span class="sxs-lookup"><span data-stu-id="dbd16-146">POCO</span></span>
<span data-ttu-id="dbd16-147">Acronyme de Plain-Old CLR Object.</span><span class="sxs-lookup"><span data-stu-id="dbd16-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="dbd16-148">Classe d’utilisateur simple qui n’a pas de dépendances avec n’importe quelle infrastructure.</span><span class="sxs-lookup"><span data-stu-id="dbd16-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="dbd16-149">Dans le contexte d’EF, une classe d’entité qui ne dérive pas de EntityObject, implémente toutes les interfaces ou transporte tous les attributs définis dans EF.</span><span class="sxs-lookup"><span data-stu-id="dbd16-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="dbd16-150">De telles classes d’entité qui sont découplées de l’infrastructure de persistance sont également dites « ignorant la persistance ».</span><span class="sxs-lookup"><span data-stu-id="dbd16-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="dbd16-151">Relation inverse</span><span class="sxs-lookup"><span data-stu-id="dbd16-151">Relationship inverse</span></span>
<span data-ttu-id="dbd16-152">L’extrémité opposée d’une relation, par exemple Product. Catégorie et catégorie. Production.</span><span class="sxs-lookup"><span data-stu-id="dbd16-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="dbd16-153">Entité de suivi automatique</span><span class="sxs-lookup"><span data-stu-id="dbd16-153">Self-tracking entity</span></span>
<span data-ttu-id="dbd16-154">Entité générée à partir d’un modèle de génération de code qui permet le développement multicouche.</span><span class="sxs-lookup"><span data-stu-id="dbd16-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="dbd16-155">Type de table par béton (TPC)</span><span class="sxs-lookup"><span data-stu-id="dbd16-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="dbd16-156">Méthode de mappage de l’héritage où chaque type non abstrait de la hiérarchie est mappé à une table distincte dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dbd16-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="dbd16-157">TPH (table par hiérarchie)</span><span class="sxs-lookup"><span data-stu-id="dbd16-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="dbd16-158">Méthode de mappage de l’héritage dans lequel tous les types de la hiérarchie sont mappés à la même table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dbd16-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="dbd16-159">Une ou plusieurs colonnes de discriminateur sont utilisées pour identifier le type auquel chaque ligne est associée.</span><span class="sxs-lookup"><span data-stu-id="dbd16-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="dbd16-160">Table par type (TPT)</span><span class="sxs-lookup"><span data-stu-id="dbd16-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="dbd16-161">Méthode de mappage de l’héritage où les propriétés communes de tous les types de la hiérarchie sont mappées à la même table dans la base de données, mais les propriétés propres à chaque type sont mappées à une table distincte.</span><span class="sxs-lookup"><span data-stu-id="dbd16-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="dbd16-162">Détection de type</span><span class="sxs-lookup"><span data-stu-id="dbd16-162">Type discovery</span></span>
<span data-ttu-id="dbd16-163">Processus d’identification des types qui doivent faire partie d’un modèle de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dbd16-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
