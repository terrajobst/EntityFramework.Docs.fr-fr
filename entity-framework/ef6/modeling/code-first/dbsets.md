---
title: Définition de DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993855"
---
# <a name="defining-dbsets"></a><span data-ttu-id="3ce13-102">Définition de DbSets</span><span class="sxs-lookup"><span data-stu-id="3ce13-102">Defining DbSets</span></span>
<span data-ttu-id="3ce13-103">Lors du développement avec le workflow de Code First que vous définissez un DbContext dérivé qui représente votre session avec la base de données et expose un DbSet pour chaque type dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="3ce13-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="3ce13-104">Cette rubrique décrit les différentes méthodes que vous pouvez définir les propriétés DbSet.</span><span class="sxs-lookup"><span data-stu-id="3ce13-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="3ce13-105">DbContext avec des propriétés DbSet</span><span class="sxs-lookup"><span data-stu-id="3ce13-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="3ce13-106">Le cas courant indiqué dans les exemples de Code First consiste à avoir un DbContext avec les propriétés publiques DbSet automatique pour les types d’entités de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="3ce13-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="3ce13-107">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3ce13-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="3ce13-108">Lorsqu’il est utilisé en mode Code First, cette opération configure des Blogs et des publications en tant que types d’entité, comme la configuration est accessible à partir de ces autres types de.</span><span class="sxs-lookup"><span data-stu-id="3ce13-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="3ce13-109">En outre, DbContext appelle automatiquement la méthode setter pour chacune de ces propriétés pour définir une instance de la DbSet appropriée.</span><span class="sxs-lookup"><span data-stu-id="3ce13-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="3ce13-110">DbContext avec des propriétés IDbSet</span><span class="sxs-lookup"><span data-stu-id="3ce13-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="3ce13-111">Il existe des situations, par exemple créer des objets fictifs ou fakes, où il est plus utile déclarer des propriétés de votre jeu à l’aide d’une interface.</span><span class="sxs-lookup"><span data-stu-id="3ce13-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="3ce13-112">Dans ce cas le IDbSet interface peut être utilisée à la place de DbSet.</span><span class="sxs-lookup"><span data-stu-id="3ce13-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="3ce13-113">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3ce13-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="3ce13-114">Ce contexte fonctionne dans exactement la même façon que le contexte qui utilise la classe DbSet pour ses propriétés de jeu.</span><span class="sxs-lookup"><span data-stu-id="3ce13-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="3ce13-115">DbContext les propriétés définies en lecture seule</span><span class="sxs-lookup"><span data-stu-id="3ce13-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="3ce13-116">Si vous ne souhaitez pas exposer des accesseurs Set publics pour vos propriétés DbSet ou IDbSet, vous pouvez créer des propriétés en lecture seule et créer les instances vous-même.</span><span class="sxs-lookup"><span data-stu-id="3ce13-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="3ce13-117">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3ce13-117">For example:</span></span>  

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

<span data-ttu-id="3ce13-118">Notez que DbContext met en cache l’instance de DbSet retourné à partir de la méthode Set afin que chacune de ces propriétés retournera la même instance chaque fois qu’elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="3ce13-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="3ce13-119">Découverte de types d’entités pour Code First fonctionne de la même façon ici car il fait pour les propriétés avec accesseurs Get publique et Set.</span><span class="sxs-lookup"><span data-stu-id="3ce13-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
