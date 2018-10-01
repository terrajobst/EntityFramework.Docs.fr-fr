---
title: À l’aide de migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: cf6c3a0a256730b24addf1012d6ff53b17035cd4
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459537"
---
# <a name="using-migrateexe"></a>À l’aide de migrate.exe
Migrations Code First peut être utilisées pour mettre à jour une base de données à l’intérieur de visual studio, mais peuvent également être exécutées via la migrate.exe outil ligne de commande. Cette page pour obtenir une vue d’ensemble rapide sur comment utiliser migrate.exe pour exécuter des migrations par rapport à une base de données.

> [!NOTE]
> Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base. Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.

## <a name="copy-migrateexe"></a>Copie migrate.exe

Lorsque vous installez Entity Framework à l’aide de NuGet migrate.exe se trouve dans le dossier des outils du package téléchargé. Dans &lt;dossier du projet&gt;\\packages\\EntityFramework.&lt; version&gt;\\outils

Une fois que vous avez migrate.exe vous devez copier dans l’emplacement de l’assembly qui contient vos migrations.

Si votre application cible le .NET 4 et 4.5 pas, puis vous devrez copier le **Redirect.config** dans l’emplacement ainsi et renommez-le **migrate.exe.config**. Il s’agit donc que migrate.exe obtient les redirections de liaison correct pour être en mesure de localiser l’assembly d’Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Fichiers de .NET 4.5](~/ef6/media/net45files.png) | ![Fichiers de .NET 4.0](~/ef6/media/net40files.png) |

> [!NOTE]
> Migrate.exe ne prend pas en charge x64 assemblys.

Une fois que vous avez déplacé migrate.exe dans le dossier approprié, vous devez être en mesure d’utiliser pour exécuter des migrations par rapport à la base de données. L’utilitaire est conçu pour faire il suffit exécuter des migrations. Il ne peut pas générer des migrations ou créer un script SQL.

## <a name="see-options"></a>Consultez les options

``` console
Migrate.exe /?
```

La méthode ci-dessus affiche la page d’aide associée à cet utilitaire, notez que vous devez avoir l’EntityFramework.dll dans le même emplacement que vous exécutez migrate.exe pour que cela fonctionne.

## <a name="migrate-to-the-latest-migration"></a>Migrer vers la dernière migration

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Lorsque migrate.exe en cours d’exécution le paramètre obligatoire uniquement est l’assembly, qui est l’assembly qui contient les migrations que vous tentez d’exécuter, mais il utilise une convention tous les paramètres basés sur si vous ne spécifiez pas le fichier de configuration.

## <a name="migrate-to-a-specific-migration"></a>Migrer vers une migration spécifique

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Si vous souhaitez exécuter des migrations jusqu'à une migration spécifique, vous pouvez spécifier le nom de la migration. Cela exécutera toutes les migrations précédentes en fonction des besoins jusqu'à ce que la mise à la migration spécifiée.

## <a name="specify-working-directory"></a>Spécifiez le répertoire de travail

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Si vous assembly possède des dépendances ou lit les fichiers par rapport au répertoire de travail vous devrez définir startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Spécifier la configuration de la migration à utiliser

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Si vous avez plusieurs classes de configuration de migration, classes qui héritent de DbMigrationConfiguration, puis vous devez spécifier ce qui doit être utilisé pour cette exécution. Cela est spécifié en fournissant le deuxième paramètre facultatif sans commutateur comme ci-dessus.

## <a name="provide-connection-string"></a>Fournir la chaîne de connexion

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Si vous souhaitez spécifier une chaîne de connexion à la ligne de commande, vous devez également fournir le nom du fournisseur. Ne pas spécifier le nom du fournisseur provoquera une exception.

## <a name="common-problems"></a>Problèmes courants

| Message d'erreur                                                                                                                                                                                                                                                                                                                      | Solution                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exception non gérée : System.IO.FileLoadException : Impossible de charger le fichier ou l’assembly ' Entity Framework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' ou une de ses dépendances. Définition du manifeste de l’assembly trouvée ne correspond pas à la référence d’assembly. (Exception de HRESULT : 0x80131040)         | Cela signifie généralement que vous exécutez une application .NET 4 sans le fichier Redirect.config. Vous devez copier le Redirect.config au même emplacement que migrate.exe et renommez-le migrate.exe.config.                                                                                       |
| Exception non gérée : System.IO.FileLoadException : Impossible de charger le fichier ou l’assembly ' Entity Framework, Version = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' ou une de ses dépendances. Définition du manifeste de l’assembly trouvée ne correspond pas à la référence d’assembly. (Exception de HRESULT : 0x80131040)          | Cette exception signifie que vous exécutez une application avec le Redirect.config copiée à l’emplacement migrate.exe de .NET 4.5. Si votre application est .NET 4.5 il est inutile d’avoir le fichier de configuration avec la redirection à l’intérieur. Supprimez le fichier migrate.exe.config.                                    |
| Erreur : Impossible de mettre à jour de la base de données pour faire correspondre le modèle actuel, car les modifications en attente et la migration automatique est désactivée. Écrire les modifications de modèle en attente dans une migration basée sur le code ou activez la migration automatique. Définissez DbMigrationsConfiguration.AutomaticMigrationsEnabled à True pour activer la migration automatique. | Cette erreur se produit si effectuez la migration en cours d’exécution lorsque vous n’avez pas créé une migration pour prendre en charge les modifications apportées au modèle et la base de données ne correspond pas au modèle. Ajout d’une propriété à une classe de modèle puis en exécutant migrate.exe sans provoquer la création d’une migration pour mettre à niveau la base de données est un exemple. |
| Erreur : Le Type n’est pas résolu pour le membre ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ».                                                                                                                                       | Cette erreur peut résulter en spécifiant un répertoire de démarrage incorrects. Cela doit être l’emplacement de migrate.exe                                                                                                                                                                                      |
| Exception non gérée : System.NullReferenceException : référence non définie sur une instance d’un objet de l’objet. <br/>   à System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Cela peut être dû à ne pas spécifier un paramètre obligatoire pour un scénario que vous utilisez. Par exemple, en spécifiant une chaîne de connexion sans spécifier le nom du fournisseur.                                                                                                                        |
| Erreur : plusieurs types de configuration de migrations a été trouvée dans l’assembly « ClassLibrary1 ». Spécifiez le nom de l’objet à utiliser.                                                                                                                                                                                                  | Comme l’erreur indique, il existe plusieurs configuration classe dans l’assembly donné. Vous devez utiliser le commutateur /configurationType pour spécifier lequel utiliser.                                                                                                                                           |
| Erreur : Impossible de charger un fichier ou assembly '&lt;assemblyName&gt;' ou une de ses dépendances. L’assembly donné codebase ou de nom n’est pas valide. (Exception de HRESULT : 0x80131047)                                                                                                                                                    | Cela peut être dû en spécifiant un nom d’assembly incorrecte ou n’ayant ne pas                                                                                                                                                                                                                          |
| Erreur : Impossible de charger un fichier ou assembly '&lt;assemblyName&gt;' ou une de ses dépendances. Tentative de chargement d’un programme au format incorrect.                                                                                                                                                                          | Cela se produit si vous essayez d’exécuter migrate.exe contre un x64 application. Entity Framework 5.0 et ci-dessous ne fonctionnent que pour x86.                                                                                                                                                                                |
