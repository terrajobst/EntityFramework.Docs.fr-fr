---
title: Configuration basée sur le code-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417868"
---
# <a name="code-based-configuration"></a><span data-ttu-id="b9f3c-102">Configuration basée sur le code</span><span class="sxs-lookup"><span data-stu-id="b9f3c-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="b9f3c-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b9f3c-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b9f3c-105">La configuration d’une application Entity Framework peut être spécifiée dans un fichier de configuration (App. config/Web. config) ou via du code.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="b9f3c-106">Cette dernière est connue sous le nom de configuration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="b9f3c-107">La configuration dans un fichier de configuration est décrite dans un [article distinct](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="b9f3c-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="b9f3c-108">Le fichier de configuration est prioritaire par rapport à la configuration basée sur le code.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="b9f3c-109">En d’autres termes, si une option de configuration est définie à la fois dans le code et dans le fichier de configuration, alors le paramètre dans le fichier de configuration est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="b9f3c-110">Utilisation de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b9f3c-110">Using DbConfiguration</span></span>  

<span data-ttu-id="b9f3c-111">La configuration basée sur le code dans EF6 et versions ultérieures est obtenue en créant une sous-classe de System. Data. Entity. config. DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="b9f3c-112">Les instructions suivantes doivent être suivies lors de la sous-classe DbConfiguration :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="b9f3c-113">Créez une seule classe DbConfiguration pour votre application.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="b9f3c-114">Cette classe spécifie les paramètres de l’ensemble du domaine d’application.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="b9f3c-115">Placez votre classe DbConfiguration dans le même assembly que votre classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b9f3c-116">(Consultez la section *déplacement de DbConfiguration* si vous souhaitez modifier cela.)</span><span class="sxs-lookup"><span data-stu-id="b9f3c-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="b9f3c-117">Donnez à votre classe DbConfiguration un constructeur sans paramètre public.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="b9f3c-118">Définissez les options de configuration en appelant les méthodes DbConfiguration protégées à partir de ce constructeur.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="b9f3c-119">Conformément à ces instructions, EF peut détecter et utiliser votre configuration automatiquement par les outils qui ont besoin d’accéder à votre modèle et lorsque votre application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="b9f3c-120">Exemple</span><span class="sxs-lookup"><span data-stu-id="b9f3c-120">Example</span></span>  

<span data-ttu-id="b9f3c-121">Une classe dérivée de DbConfiguration peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-121">A class derived from DbConfiguration might look like this:</span></span>  

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

<span data-ttu-id="b9f3c-122">Cette classe configure EF pour utiliser la stratégie d’exécution de SQL Azure-pour réessayer automatiquement les opérations de base de données ayant échoué, et pour utiliser la base de données locale pour les bases de données créées par Convention à partir de Code First.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="b9f3c-123">Déplacement de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b9f3c-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="b9f3c-124">Dans certains cas, il n’est pas possible de placer votre classe DbConfiguration dans le même assembly que votre classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b9f3c-125">Par exemple, vous pouvez avoir deux classes DbContext chacune dans des assemblys différents.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="b9f3c-126">Il existe deux options pour gérer cela.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-126">There are two options for handling this.</span></span>  

<span data-ttu-id="b9f3c-127">La première option consiste à utiliser le fichier de configuration pour spécifier l’instance DbConfiguration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="b9f3c-128">Pour ce faire, définissez l’attribut codeConfigurationType de la section entityFramework.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="b9f3c-129">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="b9f3c-130">La valeur de codeConfigurationType doit être le nom complet de l’assembly et de l’espace de noms de votre classe DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="b9f3c-131">La deuxième option consiste à placer DbConfigurationTypeAttribute sur votre classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="b9f3c-132">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="b9f3c-133">La valeur passée à l’attribut peut être votre type DbConfiguration, comme indiqué ci-dessus, ou la chaîne de nom de type qualifié d’assembly et d’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="b9f3c-134">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="b9f3c-135">Définition explicite de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b9f3c-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="b9f3c-136">Dans certaines situations, la configuration peut être nécessaire avant l’utilisation d’un type DbContext.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="b9f3c-137">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-137">Examples of this include:</span></span>  

- <span data-ttu-id="b9f3c-138">Utilisation de DbModelBuilder pour générer un modèle sans contexte</span><span class="sxs-lookup"><span data-stu-id="b9f3c-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="b9f3c-139">Utilisation d’un autre code d’infrastructure/utilitaire qui utilise un DbContext où ce contexte est utilisé avant l’utilisation de votre contexte d’application</span><span class="sxs-lookup"><span data-stu-id="b9f3c-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="b9f3c-140">Dans ce cas, EF ne parvient pas à découvrir la configuration automatiquement, mais vous devez effectuer l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="b9f3c-141">Définissez le type DbConfiguration dans le fichier de configuration, comme décrit dans la section *déplacement de DbConfiguration* ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="b9f3c-142">Appeler la méthode statique DbConfiguration. SetConfiguration pendant le démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="b9f3c-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="b9f3c-143">Remplacement de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="b9f3c-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="b9f3c-144">Dans certains cas, vous devez remplacer le jeu de configuration dans le DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="b9f3c-145">Cela n’est généralement pas effectué par les développeurs d’applications, mais plutôt par des fournisseurs tiers et des plug-ins qui ne peuvent pas utiliser une classe DbConfiguration dérivée.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="b9f3c-146">Pour ce faire, EntityFramework autorise l’inscription d’un gestionnaire d’événements qui peut modifier la configuration existante juste avant qu’elle ne soit verrouillée.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="b9f3c-147">Il fournit également une méthode de sucre pour remplacer tout service retourné par le localisateur de service EF.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="b9f3c-148">Voici comment il est destiné à être utilisé :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="b9f3c-149">Au démarrage de l’application (avant l’utilisation d’EF), le plug-in ou le fournisseur doit inscrire la méthode du gestionnaire d’événements pour cet événement.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="b9f3c-150">(Notez que cela doit se produire avant que l’application utilise EF.)</span><span class="sxs-lookup"><span data-stu-id="b9f3c-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="b9f3c-151">Le gestionnaire d’événements effectue un appel à ReplaceService pour chaque service qui doit être remplacé.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="b9f3c-152">Par exemple, pour remplacer IDbConnectionFactory et DbProviderService, vous devez enregistrer un gestionnaire de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="b9f3c-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="b9f3c-153">Dans le code ci-dessus, MyProviderServices et MyConnectionFactory représentent vos implémentations du service.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="b9f3c-154">Vous pouvez également ajouter des gestionnaires de dépendances supplémentaires pour obtenir le même effet.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="b9f3c-155">Notez que vous pouvez également encapsuler DbProviderFactory de cette manière, mais cela n’affecte que EF et non les utilisations de DbProviderFactory en dehors d’EF.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="b9f3c-156">Pour cette raison, vous souhaiterez sans doute continuer à encapsuler DbProviderFactory comme vous l’avez déjà fait.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="b9f3c-157">Vous devez également garder à l’esprit les services que vous exécutez en externe dans votre application, par exemple, lors de l’exécution de migrations à partir de la console du gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="b9f3c-158">Lorsque vous exécutez la migration à partir de la console, elle tente de trouver votre DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="b9f3c-159">Toutefois, le fait qu’il récupère ou non le service encapsulé dépend de l’emplacement du gestionnaire d’événements qu’il a inscrit.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="b9f3c-160">S’il est enregistré dans le cadre de la construction de votre DbConfiguration, le code doit s’exécuter et le service doit être encapsulé.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="b9f3c-161">Ce n’est généralement pas le cas et cela signifie que les outils n’obtiendront pas le service encapsulé.</span><span class="sxs-lookup"><span data-stu-id="b9f3c-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
