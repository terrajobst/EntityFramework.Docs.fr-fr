---
title: Utilisation de proxys - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120816"
---
# <a name="working-with-proxies"></a><span data-ttu-id="4c627-102">Utilisation de proxys</span><span class="sxs-lookup"><span data-stu-id="4c627-102">Working with proxies</span></span>
<span data-ttu-id="4c627-103">Lorsque vous créez des instances de types d’entités POCO, Entity Framework crée souvent des instances d’un type dérivé généré dynamiquement qui agit comme un proxy pour l’entité.</span><span class="sxs-lookup"><span data-stu-id="4c627-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="4c627-104">Ce proxy substitue certaines propriétés virtuelles de l’entité à insérer des raccordements pour effectuer des actions automatiquement lorsque la propriété est accessible.</span><span class="sxs-lookup"><span data-stu-id="4c627-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="4c627-105">Par exemple, ce mécanisme est utilisé pour prendre en charge le chargement différé des relations.</span><span class="sxs-lookup"><span data-stu-id="4c627-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="4c627-106">Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.</span><span class="sxs-lookup"><span data-stu-id="4c627-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="4c627-107">La désactivation de la création de proxy</span><span class="sxs-lookup"><span data-stu-id="4c627-107">Disabling proxy creation</span></span>  

<span data-ttu-id="4c627-108">Il est parfois utile empêcher la création d’instances de proxy d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4c627-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="4c627-109">Par exemple, la sérialisation des instances de proxy non est considérablement plus facile que la sérialisation des instances de proxy.</span><span class="sxs-lookup"><span data-stu-id="4c627-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="4c627-110">La création de proxy peut être désactivée en désactivant l’indicateur ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="4c627-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="4c627-111">Un seul endroit, vous pouvez procéder ainsi est dans le constructeur de votre contexte.</span><span class="sxs-lookup"><span data-stu-id="4c627-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="4c627-112">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c627-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="4c627-113">Notez que l’Entity Framework ne crée pas de proxys pour les types où il n’existe rien pour le proxy à faire.</span><span class="sxs-lookup"><span data-stu-id="4c627-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="4c627-114">Cela signifie que vous pouvez également éviter les proxys grâce à des types qui sont scellés et/ou disposent pas de propriétés virtuels.</span><span class="sxs-lookup"><span data-stu-id="4c627-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="4c627-115">Création explicite d’une instance d’un proxy</span><span class="sxs-lookup"><span data-stu-id="4c627-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="4c627-116">Une instance de proxy ne sera pas créée si vous créez une instance d’une entité à l’aide de l’opérateur new.</span><span class="sxs-lookup"><span data-stu-id="4c627-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="4c627-117">Il peut s’agir d’un problème, mais si vous avez besoin créer une instance de proxy (par exemple, pour que différé le chargement ou le proxy le suivi des modifications fonctionnent) puis vous pouvez le faire à l’aide de la méthode Create de DbSet.</span><span class="sxs-lookup"><span data-stu-id="4c627-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="4c627-118">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c627-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="4c627-119">La version générique de création peut être utilisée si vous souhaitez créer une instance d’un type d’entité dérivé.</span><span class="sxs-lookup"><span data-stu-id="4c627-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="4c627-120">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c627-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="4c627-121">Notez que la méthode Create ne pas ajouter ou joindre l’entité créée dans le contexte.</span><span class="sxs-lookup"><span data-stu-id="4c627-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="4c627-122">Notez que la méthode Create ne crée simplement une instance du type d’entité si la création d’un type de proxy pour l’entité n’aurait aucune valeur, car il ne ferait rien.</span><span class="sxs-lookup"><span data-stu-id="4c627-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="4c627-123">Par exemple, si le type d’entité est scellé et/ou ne possède aucune propriété virtuelle créer créer simplement une instance du type d’entité.</span><span class="sxs-lookup"><span data-stu-id="4c627-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="4c627-124">Obtenir le type d’entité réelle à partir d’un type de proxy</span><span class="sxs-lookup"><span data-stu-id="4c627-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="4c627-125">Types de proxy ont des noms qui ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="4c627-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="4c627-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="4c627-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="4c627-127">Vous pouvez trouver le type d’entité pour ce type de proxy à l’aide de la méthode GetObjectType a d’ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="4c627-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="4c627-128">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4c627-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="4c627-129">Notez que si le type passé à GetObjectType a est une instance d’un type d’entité qui n’est pas un type de proxy, le type d’entité est toujours retournée.</span><span class="sxs-lookup"><span data-stu-id="4c627-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="4c627-130">Cela signifie que vous pouvez toujours utiliser cette méthode pour obtenir le type d’entité réel sans aucune vérification pour voir si le type est un type de proxy ou non.</span><span class="sxs-lookup"><span data-stu-id="4c627-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
