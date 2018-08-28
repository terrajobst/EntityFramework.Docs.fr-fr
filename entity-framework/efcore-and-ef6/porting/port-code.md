---
title: Portage depuis EF6 vers EF Core - portage d’un modèle basé sur le Code
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997047"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="b6c26-102">Portage d’un modèle basé sur le Code EF6 vers EF Core</span><span class="sxs-lookup"><span data-stu-id="b6c26-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="b6c26-103">Si vous avez lu les mises en garde et vous êtes prêt à un port, voici quelques conseils pour vous aider à commencer.</span><span class="sxs-lookup"><span data-stu-id="b6c26-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="b6c26-104">Installer les packages NuGet de EF Core</span><span class="sxs-lookup"><span data-stu-id="b6c26-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="b6c26-105">Pour utiliser EF Core, vous installez le package NuGet pour le fournisseur de base de données que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="b6c26-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="b6c26-106">Par exemple, lors du ciblage de SQL Server, que vous installeriez `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="b6c26-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="b6c26-107">Consultez [les fournisseurs de base de données](../../core/providers/index.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="b6c26-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="b6c26-108">Si vous envisagez d’utiliser des migrations, vous devez également installer le `Microsoft.EntityFrameworkCore.Tools` package.</span><span class="sxs-lookup"><span data-stu-id="b6c26-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="b6c26-109">Il est possible de laisser le package NuGet d’EF6 (Entity Framework) est installé, comme EF Core et EF6 peuvent être utilisé côté à côte dans la même application.</span><span class="sxs-lookup"><span data-stu-id="b6c26-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="b6c26-110">Toutefois, si vous n’êtes pas l’intention d’utiliser EF6 dans les zones de votre application, puis la désinstallation du package vous permettent d’exercer des erreurs de compilation sur les éléments de code nécessitant votre attention.</span><span class="sxs-lookup"><span data-stu-id="b6c26-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="b6c26-111">Remplacez les espaces de noms</span><span class="sxs-lookup"><span data-stu-id="b6c26-111">Swap namespaces</span></span>

<span data-ttu-id="b6c26-112">La plupart des API que vous utilisez dans EF6 se trouvent dans le `System.Data.Entity` espace de noms (et liés sous-espaces de noms).</span><span class="sxs-lookup"><span data-stu-id="b6c26-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="b6c26-113">La première modification de code est disponible pour le remplacement à la `Microsoft.EntityFrameworkCore` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="b6c26-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="b6c26-114">Vous commence généralement par votre fichier de code de contexte dérivé et passez ensuite à partir de là, ciblant des erreurs de compilation lorsqu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="b6c26-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="b6c26-115">Configuration de contexte (connexion etc.).</span><span class="sxs-lookup"><span data-stu-id="b6c26-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="b6c26-116">Comme décrit dans [vous assurer de EF Core sera travail pour votre Application](ensure-requirements.md), EF Core offre moins magic autour de détection pour se connecter à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b6c26-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="b6c26-117">Vous devrez remplacer le `OnConfiguring` méthode sur votre contexte dérivé et utiliser l’API spécifique de fournisseur de base de données pour configurer la connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b6c26-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="b6c26-118">La plupart des applications EF6 stocker la chaîne de connexion dans les applications `App/Web.config` fichier.</span><span class="sxs-lookup"><span data-stu-id="b6c26-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="b6c26-119">Dans EF Core, vous lisez cette chaîne de connexion à l’aide de la `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="b6c26-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="b6c26-120">Vous devrez peut-être ajouter une référence à la `System.Configuration` assembly .NET framework à être en mesure d’utiliser cette API.</span><span class="sxs-lookup"><span data-stu-id="b6c26-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="b6c26-121">Mettre à jour votre code</span><span class="sxs-lookup"><span data-stu-id="b6c26-121">Update your code</span></span>

<span data-ttu-id="b6c26-122">À ce stade, il est une question de l’adressage des erreurs de compilation et de révision de code pour voir si les modifications de comportement aura un impact sur vous.</span><span class="sxs-lookup"><span data-stu-id="b6c26-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="b6c26-123">Migrations existantes</span><span class="sxs-lookup"><span data-stu-id="b6c26-123">Existing migrations</span></span>

<span data-ttu-id="b6c26-124">Il n’est pas vraiment possible permettent de déplacer des migrations EF6 existantes vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6c26-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="b6c26-125">Si possible, il est préférable de supposer que toutes les migrations précédentes d’EF6 ont été appliquées à la base de données et migration du schéma de celle de début pointez à l’aide d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6c26-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="b6c26-126">Pour ce faire, vous utiliseriez le `Add-Migration` commande pour ajouter une migration une fois que le modèle est déplacée vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6c26-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="b6c26-127">Puis supprimer tout le code à partir de la `Up` et `Down` méthodes de la migration du modèle généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b6c26-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="b6c26-128">Des migrations compare au modèle lors de la migration initiale a été généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b6c26-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="b6c26-129">Le port de test</span><span class="sxs-lookup"><span data-stu-id="b6c26-129">Test the port</span></span>

<span data-ttu-id="b6c26-130">Parce que votre application est compilée, ne signifie pas qu’il est correctement déplacée vers EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6c26-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="b6c26-131">Vous devez tester toutes les zones de votre application pour vous assurer qu’aucun des changements de comportement ont défavorablement affectées de votre application.</span><span class="sxs-lookup"><span data-stu-id="b6c26-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
