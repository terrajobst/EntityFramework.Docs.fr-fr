---
title: Chaînes de connexion et de modèles - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: dce414ea84f13235691abf0dcadef5c743d90f9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994957"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="96c10-102">Modèles et les chaînes de connexion</span><span class="sxs-lookup"><span data-stu-id="96c10-102">Connection strings and models</span></span>
<span data-ttu-id="96c10-103">Cette rubrique décrit comment Entity Framework détecte la connexion de base de données à utiliser, et comment vous pouvez le modifier.</span><span class="sxs-lookup"><span data-stu-id="96c10-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="96c10-104">Les modèles créés avec Code First et le Concepteur EF sont traités dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="96c10-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="96c10-105">En général, une application Entity Framework utilise une classe dérivée de DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="96c10-106">Cette classe dérivée appellera un des constructeurs de la classe DbContext de base au contrôle :</span><span class="sxs-lookup"><span data-stu-id="96c10-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="96c10-107">Comment le contexte doit se connecter à une base de données, autrement dit, comment une chaîne de connexion est trouvé/utilisé</span><span class="sxs-lookup"><span data-stu-id="96c10-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="96c10-108">Si le contexte utilisera calculer un modèle à l’aide de Code First ou charger un modèle créé avec le Concepteur EF</span><span class="sxs-lookup"><span data-stu-id="96c10-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="96c10-109">Options avancées supplémentaires</span><span class="sxs-lookup"><span data-stu-id="96c10-109">Additional advanced options</span></span>  

<span data-ttu-id="96c10-110">Les fragments suivants illustrent que certaines des façons les constructeurs DbContext peuvent être utilisées.</span><span class="sxs-lookup"><span data-stu-id="96c10-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="96c10-111">Utiliser Code First par convention avec connexion</span><span class="sxs-lookup"><span data-stu-id="96c10-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="96c10-112">Si vous n’avez pas fait toute autre configuration dans votre application, puis en appelant le constructeur sans paramètre sur DbContext entraîne DbContext exécuter en mode Code First avec une connexion de base de données créée par convention.</span><span class="sxs-lookup"><span data-stu-id="96c10-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="96c10-113">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="96c10-114">Dans cet exemple DbContext utilise le nom qualifié d’espace de noms de votre class—Demo.EF.BloggingContext—as de contexte dérivé du nom de la base de données et crée une chaîne de connexion pour cette base de données à l’aide de SQL Express ou LocalDB.</span><span class="sxs-lookup"><span data-stu-id="96c10-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="96c10-115">Si les deux sont installés, SQL Express sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="96c10-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="96c10-116">Visual Studio 2010 inclut SQL Express par défaut et Visual Studio 2012 et versions ultérieures inclut LocalDB.</span><span class="sxs-lookup"><span data-stu-id="96c10-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="96c10-117">Pendant l’installation, le package EntityFramework NuGet vérifie le serveur de base de données est disponible.</span><span class="sxs-lookup"><span data-stu-id="96c10-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="96c10-118">Le package NuGet est puis mettez à jour le fichier de configuration en définissant le serveur de base de données par défaut qui utilise Code First lors de la création d’une connexion par convention.</span><span class="sxs-lookup"><span data-stu-id="96c10-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="96c10-119">Si SQL Express est en cours d’exécution, il sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="96c10-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="96c10-120">Si SQL Express n’est pas disponible, base de données locale sera être enregistré en tant que la valeur par défaut à la place.</span><span class="sxs-lookup"><span data-stu-id="96c10-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="96c10-121">Aucun modifications ne sont apportées au fichier de configuration si elle contient déjà un paramètre pour la fabrique de connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="96c10-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="96c10-122">Utiliser Code First avec connexion par convention et le nom de la base de données spécifiée</span><span class="sxs-lookup"><span data-stu-id="96c10-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="96c10-123">Si vous n’avez pas fait toute autre configuration dans votre application, puis en appelant le constructeur string sur DbContext avec le nom de base de données que vous souhaitez utiliser entraîne DbContext exécuter en mode Code First avec une connexion de base de données créée par convention à la base de données Ce nom.</span><span class="sxs-lookup"><span data-stu-id="96c10-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="96c10-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="96c10-125">Dans cet exemple DbContext utilise « BloggingDatabase » comme nom de base de données et crée une chaîne de connexion pour cette base de données à l’aide de SQL Express (installé avec Visual Studio 2010) ou base de données locale (installée avec Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="96c10-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="96c10-126">Si les deux sont installés, SQL Express sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="96c10-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="96c10-127">Utiliser Code First avec la chaîne de connexion dans le fichier de app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="96c10-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="96c10-128">Vous pouvez choisir de placer une chaîne de connexion dans votre fichier app.config ou web.config.</span><span class="sxs-lookup"><span data-stu-id="96c10-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="96c10-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="96c10-130">Il s’agit d’un moyen simple pour indiquer à DbContext d’utiliser un serveur de base de données autre que SQL Express ou de la base de données locale, l’exemple ci-dessus spécifie une base de données SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="96c10-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="96c10-131">Si le nom de la chaîne de connexion correspond au nom de votre contexte (avec ou sans qualification d’espace de noms) puis il se trouve par DbContext lorsque le constructeur sans paramètre est utilisé.</span><span class="sxs-lookup"><span data-stu-id="96c10-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="96c10-132">Si le nom de chaîne de connexion est différent du nom de votre contexte vous pouvez indiquer DbContext pour utiliser cette connexion en mode Code First en passant le nom de chaîne de connexion au constructeur DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="96c10-133">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="96c10-134">Vous pouvez également utiliser la forme « nom =\<nom de chaîne de connexion\>» pour la chaîne passée au constructeur DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="96c10-135">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="96c10-136">Ce formulaire rend explicite à laquelle la chaîne de connexion dans votre fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="96c10-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="96c10-137">Une exception sera levée si une chaîne de connexion portant le nom spécifié est introuvable.</span><span class="sxs-lookup"><span data-stu-id="96c10-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="96c10-138">Base de données/Model First avec la chaîne de connexion dans le fichier de app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="96c10-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="96c10-139">Les modèles créés avec le Concepteur EF diffèrent des Code First dans la mesure votre modèle existe déjà et n’est pas généré à partir du code lorsque l’application s’exécute.</span><span class="sxs-lookup"><span data-stu-id="96c10-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="96c10-140">Le modèle existe en général comme un fichier EDMX dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="96c10-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="96c10-141">Le concepteur ajouterez une chaîne de connexion EF à votre fichier app.config ou web.config.</span><span class="sxs-lookup"><span data-stu-id="96c10-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="96c10-142">Cette chaîne de connexion est spéciale car elle contient des informations sur la façon de trouver les informations dans votre fichier EDMX.</span><span class="sxs-lookup"><span data-stu-id="96c10-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="96c10-143">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="96c10-144">Le Concepteur EF génère également un code qui indique à DbContext pour utiliser cette connexion en passant le nom de chaîne de connexion au constructeur DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="96c10-145">Exemple :</span><span class="sxs-lookup"><span data-stu-id="96c10-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="96c10-146">DbContext sache qu’il peut pour charger le modèle existant (au lieu de Code First pour être calculé à partir du code), car la chaîne de connexion est une chaîne de connexion EF contenant les détails du modèle à utiliser.</span><span class="sxs-lookup"><span data-stu-id="96c10-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="96c10-147">Autres options de constructeur DbContext</span><span class="sxs-lookup"><span data-stu-id="96c10-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="96c10-148">La classe DbContext contient d’autres constructeurs et les modèles d’utilisation qui permettent des scénarios plus avancés.</span><span class="sxs-lookup"><span data-stu-id="96c10-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="96c10-149">Certaines d'entre elles sont :</span><span class="sxs-lookup"><span data-stu-id="96c10-149">Some of these are:</span></span>  

- <span data-ttu-id="96c10-150">Vous pouvez utiliser la classe DbModelBuilder pour générer un modèle Code First sans instancier une instance de DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="96c10-151">Le résultat de ce est un objet DbModel.</span><span class="sxs-lookup"><span data-stu-id="96c10-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="96c10-152">Vous pouvez ensuite passer cet objet DbModel à un des constructeurs DbContext lorsque vous êtes prêt à créer votre instance de DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="96c10-153">Vous pouvez passer une chaîne de connexion complète de DbContext au lieu de simplement le nom de chaîne de base de données ou de connexion.</span><span class="sxs-lookup"><span data-stu-id="96c10-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="96c10-154">Par défaut, cette chaîne de connexion est utilisée avec le fournisseur System.Data.SqlClient ; Cela peut être modifié en définissant une implémentation différente de IConnectionFactory sur le contexte. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="96c10-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="96c10-155">Vous pouvez utiliser un objet DbConnection existant en le passant à un constructeur DbContext.</span><span class="sxs-lookup"><span data-stu-id="96c10-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="96c10-156">Si l’objet de connexion est une instance de EntityConnection, puis le modèle spécifié dans la connexion sera utilisé plutôt que calculer un modèle avec Code First.</span><span class="sxs-lookup"><span data-stu-id="96c10-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="96c10-157">Si l’objet est une instance d’un autre type — par exemple, SqlConnection, le contexte de l’utilisera pour le mode Code First.</span><span class="sxs-lookup"><span data-stu-id="96c10-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="96c10-158">Vous pouvez passer un ObjectContext existant à un constructeur DbContext pour créer un DbContext encapsulant le contexte existant.</span><span class="sxs-lookup"><span data-stu-id="96c10-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="96c10-159">Cela peut être utilisé pour les applications existantes qui utilisent ObjectContext, mais qui souhaitent tirer parti de DbContext dans certaines parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="96c10-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
