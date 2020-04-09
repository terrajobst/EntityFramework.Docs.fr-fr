---
title: Portage de EF6 à EF Core - Porting a Code-Based Model - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419636"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="e1582-102">Portage d’un modèle basé sur le code EF6 à EF Core</span><span class="sxs-lookup"><span data-stu-id="e1582-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="e1582-103">Si vous avez lu toutes les mises en garde et que vous êtes prêt à bâbord, alors voici quelques lignes directrices pour vous aider à démarrer.</span><span class="sxs-lookup"><span data-stu-id="e1582-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="e1582-104">Installer des paquets EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="e1582-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="e1582-105">Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de bases de données que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="e1582-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="e1582-106">Par exemple, lorsque vous ciblez SQL Server, vous installeriez `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="e1582-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="e1582-107">Consultez [les fournisseurs de bases de données](../../core/providers/index.md) pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="e1582-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="e1582-108">Si vous prévoyez d’utiliser les migrations, `Microsoft.EntityFrameworkCore.Tools` alors vous devez également installer le paquet.</span><span class="sxs-lookup"><span data-stu-id="e1582-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="e1582-109">Il est bon de laisser le paquet EF6 NuGet (EntityFramework) installé, comme EF Core et EF6 peuvent être utilisés côte à côte dans la même application.</span><span class="sxs-lookup"><span data-stu-id="e1582-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="e1582-110">Toutefois, si vous n’avez pas l’intention d’utiliser EF6 dans tous les domaines de votre application, puis le désinstallation du paquet aidera à compiler des erreurs sur les morceaux de code qui nécessitent une attention particulière.</span><span class="sxs-lookup"><span data-stu-id="e1582-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="e1582-111">Échanger des espaces de nom</span><span class="sxs-lookup"><span data-stu-id="e1582-111">Swap namespaces</span></span>

<span data-ttu-id="e1582-112">La plupart des API que vous `System.Data.Entity` utilisez dans EF6 sont dans l’espace nom (et les sous-noms connexes).</span><span class="sxs-lookup"><span data-stu-id="e1582-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="e1582-113">La première modification de code `Microsoft.EntityFrameworkCore` est d’échanger vers l’espace nom.</span><span class="sxs-lookup"><span data-stu-id="e1582-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="e1582-114">Vous commencez généralement avec votre fichier de code contextuelle dérivé, puis travailler à partir de là, en abordant les erreurs de compilation au fur et à mesure qu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="e1582-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="e1582-115">Configuration contextuelle (connexion, etc.)</span><span class="sxs-lookup"><span data-stu-id="e1582-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="e1582-116">Comme décrit dans [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core a moins de magie autour de la détection de la base de données pour se connecter à.</span><span class="sxs-lookup"><span data-stu-id="e1582-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="e1582-117">Vous devrez passer outre `OnConfiguring` à la méthode sur votre contexte dérivé, et utiliser l’API spécifique au fournisseur de base de données pour configurer la connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e1582-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="e1582-118">La plupart des applications EF6 `App/Web.config` stockent la chaîne de connexion dans le fichier des applications.</span><span class="sxs-lookup"><span data-stu-id="e1582-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="e1582-119">Dans EF Core, vous lisez `ConfigurationManager` cette chaîne de connexion à l’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="e1582-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="e1582-120">Vous devrez peut-être ajouter `System.Configuration` une référence à l’assemblage-cadre pour pouvoir utiliser cette API.</span><span class="sxs-lookup"><span data-stu-id="e1582-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="e1582-121">Mettre à jour votre code</span><span class="sxs-lookup"><span data-stu-id="e1582-121">Update your code</span></span>

<span data-ttu-id="e1582-122">À ce stade, il s’agit de traiter les erreurs de compilation et d’examiner le code pour voir si les changements de comportement auront un impact sur vous.</span><span class="sxs-lookup"><span data-stu-id="e1582-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="e1582-123">Migrations existantes</span><span class="sxs-lookup"><span data-stu-id="e1582-123">Existing migrations</span></span>

<span data-ttu-id="e1582-124">Il n’y a pas vraiment de moyen faisable de transférer les migrations EF6 existantes vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="e1582-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="e1582-125">Si possible, il est préférable de supposer que toutes les migrations précédentes de EF6 ont été appliquées à la base de données, puis commencer à migrer le schéma à partir de ce point en utilisant EF Core.</span><span class="sxs-lookup"><span data-stu-id="e1582-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="e1582-126">Pour ce faire, vous `Add-Migration` utiliseriez la commande pour ajouter une migration une fois que le modèle est porté à EF Core.</span><span class="sxs-lookup"><span data-stu-id="e1582-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="e1582-127">Vous supprimeriez alors tout `Up` `Down` code de la migration et des méthodes de la migration échafaudée.</span><span class="sxs-lookup"><span data-stu-id="e1582-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="e1582-128">Les migrations subséquentes se compareront au modèle lorsque cette migration initiale a été échafaudée.</span><span class="sxs-lookup"><span data-stu-id="e1582-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="e1582-129">Tester le port</span><span class="sxs-lookup"><span data-stu-id="e1582-129">Test the port</span></span>

<span data-ttu-id="e1582-130">Ce n’est pas parce que votre application est compilée qu’elle est portée avec succès à EF Core.</span><span class="sxs-lookup"><span data-stu-id="e1582-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="e1582-131">Vous devrez tester tous les domaines de votre application pour vous assurer qu’aucun des changements de comportement n’a eu d’impact négatif sur votre application.</span><span class="sxs-lookup"><span data-stu-id="e1582-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
