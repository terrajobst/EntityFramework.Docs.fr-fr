---
title: Définition de DbSets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419093"
---
# <a name="defining-dbsets"></a><span data-ttu-id="9f9ab-102">Définition de DbSets</span><span class="sxs-lookup"><span data-stu-id="9f9ab-102">Defining DbSets</span></span>
<span data-ttu-id="9f9ab-103">Lors du développement avec le flux de travail Code First, vous définissez un DbContext dérivé qui représente votre session avec la base de données et expose un DbSet pour chaque type de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="9f9ab-104">Cette rubrique décrit les différentes façons dont vous pouvez définir les propriétés DbSet.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="9f9ab-105">DbContext avec les propriétés DbSet</span><span class="sxs-lookup"><span data-stu-id="9f9ab-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="9f9ab-106">Le cas courant présenté dans Code First exemples consiste à avoir un DbContext avec des propriétés DbSet automatiques publiques pour les types d’entité de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="9f9ab-107">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9f9ab-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="9f9ab-108">En cas d’utilisation en mode Code First, cette opération configure les blogs et les publications en tant que types d’entité, ainsi que la configuration d’autres types accessibles à partir de ceux-ci.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="9f9ab-109">De plus, DbContext appellera automatiquement la méthode setter pour chacune de ces propriétés afin de définir une instance du DbSet approprié.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="9f9ab-110">DbContext avec les propriétés IDbSet</span><span class="sxs-lookup"><span data-stu-id="9f9ab-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="9f9ab-111">Dans certaines situations, par exemple lors de la création de simulacres ou de substituts, il est plus utile de déclarer vos propriétés définies à l’aide d’une interface.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="9f9ab-112">Dans ce cas, l’interface IDbSet peut être utilisée à la place de DbSet.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="9f9ab-113">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9f9ab-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="9f9ab-114">Ce contexte fonctionne exactement de la même façon que le contexte qui utilise la classe DbSet pour ses propriétés set.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="9f9ab-115">DbContext avec propriétés de jeu en lecture seule</span><span class="sxs-lookup"><span data-stu-id="9f9ab-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="9f9ab-116">Si vous ne souhaitez pas exposer les accesseurs set publics pour vos propriétés DbSet ou IDbSet, vous pouvez créer des propriétés en lecture seule et créer vous-même les instances Set.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="9f9ab-117">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9f9ab-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="9f9ab-118">Notez que DbContext met en cache l’instance de DbSet retournée par la méthode Set afin que chacune de ces propriétés retourne la même instance chaque fois qu’elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="9f9ab-119">La découverte des types d’entités pour Code First fonctionne de la même façon que pour les propriétés avec les accesseurs get et les Setters publics.</span><span class="sxs-lookup"><span data-stu-id="9f9ab-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
