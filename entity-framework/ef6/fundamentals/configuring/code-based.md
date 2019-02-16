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
# <a name="code-based-configuration"></a>Configuration basée sur le code
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Configuration d’une application Entity Framework peut être spécifiée dans un fichier de configuration (app.config/web.config) ou via le code. Ce dernier est appelé configuration basée sur le code.  

Configuration dans un fichier de configuration est décrite dans un [article distinct](config-file.md). Le fichier de configuration est prioritaire sur la configuration basée sur le code. En d’autres termes, si une option de configuration est définie dans le code et dans le fichier de configuration, le paramètre dans le fichier de configuration est utilisé.  

## <a name="using-dbconfiguration"></a>À l’aide de DbConfiguration  

Configuration par le code dans EF6 et versions ultérieures est obtenue en créant une sous-classe de System.Data.Entity.Config.DbConfiguration. Les instructions suivantes doivent être suivies lors du sous-classement DbConfiguration :  

- Créer qu’une seule classe DbConfiguration pour votre application. Cette classe spécifie les paramètres à l’échelle du domaine d’application.  
- Placez votre classe DbConfiguration dans le même assembly que votre classe DbContext. (Consultez le *DbConfiguration déplacement* section si vous souhaitez modifier cela.)  
- Donnez à votre classe DbConfiguration un constructeur sans paramètre public.  
- Définir les options de configuration en appelant des méthodes DbConfiguration protégées à partir de ce constructeur.  

Les recommandations suivantes permet d’EF découvrir et d’utiliser votre configuration automatiquement par les deux outils qui a besoin d’accéder à votre modèle et quand votre application est exécutée.  

## <a name="example"></a>Exemple  

Une classe dérivée de DbConfiguration peut ressembler à ceci :  

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

Cette classe définit EF pour utiliser la stratégie d’exécution de SQL Azure - réessayer automatiquement les opérations de base de données ayant échoué - et à utiliser la base de données locale pour les bases de données créées par convention à partir de Code First.  

## <a name="moving-dbconfiguration"></a>Déplacement DbConfiguration  

Il existe des cas où il n’est pas possible de placer votre classe DbConfiguration dans le même assembly que votre classe DbContext. Par exemple, peut avoir deux classes DbContext dans des assemblys différents. Il existe deux options pour gérer cela.  

La première option consiste à utiliser le fichier de configuration pour spécifier l’instance DbConfiguration à utiliser. Pour ce faire, définissez l’attribut codeConfigurationType de la section entityFramework. Exemple :  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

La valeur de codeConfigurationType doit être l’assembly et le nom qualifié d’espace de noms de votre classe DbConfiguration.  

La deuxième option consiste à placer des DbConfigurationTypeAttribute sur votre classe de contexte. Exemple :  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

La valeur passée à l’attribut peut être votre type DbConfiguration - comme indiqué ci-dessus - ou l’assembly et l’espace de noms de chaîne de nom de type qualifié. Exemple :  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Définition explicite de DbConfiguration  

Il existe certaines situations où la configuration peut être nécessaire avant que n’importe quel type DbContext a été utilisé. Voici quelques exemples :  

- À l’aide de DbModelBuilder pour générer un modèle sans un contexte  
- À l’aide d’un autre code d’utilitaire/framework qui utilise un DbContext où ce contexte est utilisé avant votre contexte de l’application est utilisée.  

Dans de telles situations EF ne peut pas découvrir automatiquement de la configuration et à la place effectuez l’une des opérations suivantes :  

- Définissez le type de DbConfiguration dans le fichier de configuration, comme décrit dans la *DbConfiguration déplacement* section ci-dessus
- Appelle la méthode DbConfiguration.SetConfiguration statique au cours du démarrage de l’application  

## <a name="overriding-dbconfiguration"></a>Substitution de DbConfiguration  

Il existe certaines situations où vous devez remplacer la jeu de configuration dans la DbConfiguration. Cela n’est pas généralement effectuée par les développeurs d’applications, mais plutôt par les fournisseurs tiers et les plug-ins qui ne peut pas utiliser une classe dérivée de DbConfiguration.  

Pour ce faire, Entity Framework permet à inscrire un gestionnaire d’événements qui permettre modifier la configuration existante juste avant qu’il est verrouillé.  Il fournit également une méthode sugar spécifiquement pour remplacer n’importe quel service retourné par le localisateur de service EF. Voici comment elle est destinée à être utilisée :  

- Au démarrage de l’application (avant EF est utilisé) le plug-in ou le fournisseur doit s’inscrire à la méthode de gestionnaire d’événements pour cet événement. (Notez que cela doit se produire avant que l’application utilise Entity Framework).  
- Le Gestionnaire d’événements effectue un appel à ReplaceService pour chaque service qui doit être remplacé.  

Par exemple, repalce IDbConnectionFactory et DbProviderService s’inscrire un gestionnaire de quelque chose comme suit :  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

Dans le code ci-dessus MyProviderServices et MyConnectionFactory représentent vos implémentations du service.  

Vous pouvez également ajouter des gestionnaires de dépendance supplémentaire pour obtenir le même effet.  

Notez que vous pouvez également encapsuler DbProviderFactory de cette façon, mais cela sera uniquement d’affecter les EF et pas les utilisations de DbProviderFactory en dehors d’Entity Framework. C’est pourquoi vous vous souhaitez probablement continuer à encapsuler DbProviderFactory que vous disposez avant.  

Vous devez également garder à l’esprit les services que vous exécutez en externe pour votre application, par exemple, lors de l’exécution des migrations à partir de la Console du Gestionnaire de Package. Lorsque vous exécutez migrer à partir de la console, qu'il tente de trouver votre DbConfiguration. Toutefois, s’il faut ou non il obtiendra le service encapsulé dépend où il inscrit le Gestionnaire d’événements. Si elle est inscrite dans le cadre de la construction de votre DbConfiguration ensuite le code doit s’exécuter et le service doit obtenir encapsulé. Généralement, ce ne sera pas le cas et cela signifie que les outils ne sont pas obtenir le service encapsulé.  
