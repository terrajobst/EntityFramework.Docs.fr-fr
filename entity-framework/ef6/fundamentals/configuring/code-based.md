---
title: Configuration par le code - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325351"
---
# <a name="code-based-configuration"></a><span data-ttu-id="84989-102">Configuration basée sur le code</span><span class="sxs-lookup"><span data-stu-id="84989-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="84989-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="84989-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="84989-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="84989-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="84989-105">Configuration d’une application Entity Framework peut être spécifiée dans un fichier de configuration (app.config/web.config) ou via le code.</span><span class="sxs-lookup"><span data-stu-id="84989-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="84989-106">Ce dernier est appelé configuration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="84989-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="84989-107">Configuration dans un fichier de configuration est décrite dans un [article distinct](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="84989-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="84989-108">Le fichier de configuration est prioritaire sur la configuration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="84989-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="84989-109">En d’autres termes, si une option de configuration est définie dans le code et dans le fichier de configuration, le paramètre dans le fichier de configuration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="84989-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="84989-110">À l’aide de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="84989-110">Using DbConfiguration</span></span>  

<span data-ttu-id="84989-111">Configuration par le code dans EF6 et versions ultérieures est obtenue en créant une sous-classe de System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="84989-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="84989-112">Les instructions suivantes doivent être suivies lors du sous-classement DbConfiguration :</span><span class="sxs-lookup"><span data-stu-id="84989-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="84989-113">Créer qu’une seule classe DbConfiguration pour votre application.</span><span class="sxs-lookup"><span data-stu-id="84989-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="84989-114">Cette classe spécifie les paramètres à l’échelle du domaine d’application.</span><span class="sxs-lookup"><span data-stu-id="84989-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="84989-115">Placez votre classe DbConfiguration dans le même assembly que votre classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="84989-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="84989-116">(Consultez le *DbConfiguration déplacement* section si vous souhaitez modifier cela.)</span><span class="sxs-lookup"><span data-stu-id="84989-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="84989-117">Donnez à votre classe DbConfiguration un constructeur sans paramètre public.</span><span class="sxs-lookup"><span data-stu-id="84989-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="84989-118">Définir les options de configuration en appelant des méthodes DbConfiguration protégées à partir de ce constructeur.</span><span class="sxs-lookup"><span data-stu-id="84989-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="84989-119">Les recommandations suivantes permet d’EF découvrir et d’utiliser votre configuration automatiquement par les deux outils qui a besoin d’accéder à votre modèle et quand votre application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="84989-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="84989-120">Exemple</span><span class="sxs-lookup"><span data-stu-id="84989-120">Example</span></span>  

<span data-ttu-id="84989-121">Une classe dérivée de DbConfiguration peut ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="84989-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="84989-122">Cette classe définit EF pour utiliser la stratégie d’exécution de SQL Azure - réessayer automatiquement les opérations de base de données ayant échoué - et à utiliser la base de données locale pour les bases de données créées par convention à partir de Code First.</span><span class="sxs-lookup"><span data-stu-id="84989-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="84989-123">Déplacement DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="84989-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="84989-124">Il existe des cas où il n’est pas possible de placer votre classe DbConfiguration dans le même assembly que votre classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="84989-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="84989-125">Par exemple, peut avoir deux classes DbContext dans des assemblys différents.</span><span class="sxs-lookup"><span data-stu-id="84989-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="84989-126">Il existe deux options pour gérer cela.</span><span class="sxs-lookup"><span data-stu-id="84989-126">There are two options for handling this.</span></span>  

<span data-ttu-id="84989-127">La première option consiste à utiliser le fichier de configuration pour spécifier l’instance DbConfiguration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="84989-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="84989-128">Pour ce faire, définissez l’attribut codeConfigurationType de la section entityFramework.</span><span class="sxs-lookup"><span data-stu-id="84989-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="84989-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="84989-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="84989-130">La valeur de codeConfigurationType doit être l’assembly et le nom qualifié d’espace de noms de votre classe DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="84989-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="84989-131">La deuxième option consiste à placer des DbConfigurationTypeAttribute sur votre classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="84989-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="84989-132">Exemple :</span><span class="sxs-lookup"><span data-stu-id="84989-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="84989-133">La valeur passée à l’attribut peut être votre type DbConfiguration - comme indiqué ci-dessus - ou l’assembly et l’espace de noms de chaîne de nom de type qualifié.</span><span class="sxs-lookup"><span data-stu-id="84989-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="84989-134">Exemple :</span><span class="sxs-lookup"><span data-stu-id="84989-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="84989-135">Définition explicite de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="84989-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="84989-136">Il existe certaines situations où la configuration peut être nécessaire avant que n’importe quel type DbContext a été utilisé.</span><span class="sxs-lookup"><span data-stu-id="84989-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="84989-137">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="84989-137">Examples of this include:</span></span>  

- <span data-ttu-id="84989-138">À l’aide de DbModelBuilder pour générer un modèle sans un contexte</span><span class="sxs-lookup"><span data-stu-id="84989-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="84989-139">À l’aide d’un autre code d’utilitaire/framework qui utilise un DbContext où ce contexte est utilisé avant votre contexte de l’application est utilisée.</span><span class="sxs-lookup"><span data-stu-id="84989-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="84989-140">Dans de telles situations EF ne peut pas découvrir automatiquement de la configuration et à la place effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="84989-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="84989-141">Définissez le type de DbConfiguration dans le fichier de configuration, comme décrit dans la *DbConfiguration déplacement* section ci-dessus</span><span class="sxs-lookup"><span data-stu-id="84989-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="84989-142">Appelle la méthode DbConfiguration.SetConfiguration statique au cours du démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="84989-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="84989-143">Substitution de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="84989-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="84989-144">Il existe certaines situations où vous devez remplacer la jeu de configuration dans la DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="84989-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="84989-145">Cela n’est pas généralement effectuée par les développeurs d’applications, mais plutôt par les fournisseurs tiers et les plug-ins qui ne peut pas utiliser une classe dérivée de DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="84989-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="84989-146">Pour ce faire, Entity Framework permet à inscrire un gestionnaire d’événements qui permettre modifier la configuration existante juste avant qu’il est verrouillé.</span><span class="sxs-lookup"><span data-stu-id="84989-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="84989-147">Il fournit également une méthode sugar spécifiquement pour remplacer n’importe quel service retourné par le localisateur de service EF.</span><span class="sxs-lookup"><span data-stu-id="84989-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="84989-148">Voici comment elle est destinée à être utilisée :</span><span class="sxs-lookup"><span data-stu-id="84989-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="84989-149">Au démarrage de l’application (avant EF est utilisé) le plug-in ou le fournisseur doit s’inscrire à la méthode de gestionnaire d’événements pour cet événement.</span><span class="sxs-lookup"><span data-stu-id="84989-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="84989-150">(Notez que cela doit se produire avant que l’application utilise Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="84989-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="84989-151">Le Gestionnaire d’événements effectue un appel à ReplaceService pour chaque service qui doit être remplacé.</span><span class="sxs-lookup"><span data-stu-id="84989-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="84989-152">Par exemple, repalce IDbConnectionFactory et DbProviderService s’inscrire un gestionnaire de quelque chose comme suit :</span><span class="sxs-lookup"><span data-stu-id="84989-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="84989-153">Dans le code ci-dessus MyProviderServices et MyConnectionFactory représentent vos implémentations du service.</span><span class="sxs-lookup"><span data-stu-id="84989-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="84989-154">Vous pouvez également ajouter des gestionnaires de dépendance supplémentaire pour obtenir le même effet.</span><span class="sxs-lookup"><span data-stu-id="84989-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="84989-155">Notez que vous pouvez également encapsuler DbProviderFactory de cette façon, mais cela sera uniquement d’affecter les EF et pas les utilisations de DbProviderFactory en dehors d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84989-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="84989-156">C’est pourquoi vous vous souhaitez probablement continuer à encapsuler DbProviderFactory que vous disposez avant.</span><span class="sxs-lookup"><span data-stu-id="84989-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="84989-157">Vous devez également garder à l’esprit les services que vous exécutez en externe pour votre application, par exemple, lors de l’exécution des migrations à partir de la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="84989-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="84989-158">Lorsque vous exécutez migrer à partir de la console, qu'il tente de trouver votre DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="84989-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="84989-159">Toutefois, s’il faut ou non il obtiendra le service encapsulé dépend où il inscrit le Gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="84989-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="84989-160">Si elle est inscrite dans le cadre de la construction de votre DbConfiguration ensuite le code doit s’exécuter et le service doit obtenir encapsulé.</span><span class="sxs-lookup"><span data-stu-id="84989-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="84989-161">Généralement, ce ne sera pas le cas et cela signifie que les outils ne sont pas obtenir le service encapsulé.</span><span class="sxs-lookup"><span data-stu-id="84989-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
