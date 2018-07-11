---
title: La mise à niveau à partir d’EF Core 1.0 RC2 vers RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949055"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>La mise à niveau à partir d’EF Core 1.0 RC2 vers RTM

Cet article fournit des conseils pour déplacer une application générée avec les packages de RC2 à 1.0.0 RTM.

## <a name="package-versions"></a>Versions de package

Les noms des packages de niveau supérieur que vous installeriez en général dans une application n’a pas changé entre RC2 et RTM.

**Vous devez mettre à niveau les packages installés pour les versions RTM :**

* Packages de Runtime (par exemple, `Microsoft.EntityFrameworkCore.SqlServer`) a été remplacée par `1.0.0-rc2-final` à `1.0.0`.

* Le `Microsoft.EntityFrameworkCore.Tools` package a été remplacée par `1.0.0-preview1-final` à `1.0.0-preview2-final`. Notez que les outils sont toujours en version préliminaire.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migrations existantes peut-être maxLength ajouté

Dans RC2, la définition de colonne dans une migration ressemblait à `table.Column<string>(nullable: true)` et la longueur de la colonne a été recherchée dans des métadonnées que nous stockons dans le code-behind de la migration. Dans la version RTM, la longueur est désormais incluse dans le code structuré `table.Column<string>(maxLength: 450, nullable: true)`.

Les migrations existantes qui ont été structurées avant d’utiliser la version RTM n’aura pas la `maxLength` argument spécifié. Cela signifie que la longueur maximale prise en charge par la base de données sera utilisée (`nvarchar(max)` sur SQL Server). Cela peut être adaptée pour certaines colonnes, mais les colonnes qui font partie d’une clé, clé étrangère, ou les index doivent être mis à jour pour inclure une longueur maximale. Par convention, 450 correspond à la longueur maximale utilisés pour les clés, clés étrangères et les colonnes indexées. Si vous avez explicitement configuré une longueur dans le modèle, vous devez utiliser cette longueur à la place.

**ASP.NET Identity**

Cette modification a une incidence sur les projets qui utilisent ASP.NET Identity et ont été créés à partir d’un pré-RTM de modèle de projet. Le modèle de projet inclut une migration permet de créer la base de données. Cette migration doit être modifiée pour spécifier une longueur maximale de `256` sur les colonnes suivantes.

*  **AspNetRoles**

    * Name

    * NormalizedName

*  **AspNetUsers**

   * Messagerie

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Échec pour apporter cette modification entraîne l’exception suivante lors de la migration initiale est appliquée à une base de données.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core : Supprimer « imports » dans project.json

Si vous cibliez .NET Core avec RC2, vous deviez ajouter `imports` à project.json en tant que solution de contournement temporaire pour certaines des dépendances d’EF Core prenant ne pas en charge .NET Standard. Vous pouvez maintenant en supprimer.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> Depuis la version 1.0 RTM, le [du SDK .NET Core](https://www.microsoft.com/net/download/core) prend en charge n’est plus `project.json` ou que vous développez des applications .NET Core à l’aide de Visual Studio 2015. Nous vous recommandons de [migrer de project.json vers csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Si vous utilisez Visual Studio, nous vous recommandons de passer à [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP : Ajouter des redirections de liaison

Tentative d’exécution des commandes EF sur les projets de plateforme universelle Windows (UWP) entraîne l’erreur suivante :

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Vous devez ajouter manuellement des redirections de liaison pour le projet UWP. Créez un fichier nommé `App.config` dans le projet de dossier racine et ajouter des redirections vers les versions correcte de l’assembly.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
