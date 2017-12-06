---
title: "La mise à niveau à partir de EF Core 1.0 RC2 vers la version RTM - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>La mise à niveau à partir de EF Core 1.0 RC2 vers la version RTM

Cet article fournit des conseils pour déplacer une application générée avec les packages 1.0.0 RC2 RTM.

## <a name="package-versions"></a>Versions de package

Les noms des packages de niveau supérieur que vous devez généralement installer dans une application n’a pas changé entre RC2 et RTM.

**Vous devez mettre à niveau les packages installés pour les versions RTM :**

* Les packages d’exécution (par exemple, `Microsoft.EntityFrameworkCore.SqlServer`) a été remplacée par `1.0.0-rc2-final` à `1.0.0`.

* Le `Microsoft.EntityFrameworkCore.Tools` package a été remplacée par `1.0.0-preview1-final` à `1.0.0-preview2-final`. Notez que les outils sont toujours en version préliminaire.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migrations existantes doit peut-être maxLength ajouté

Dans RC2, la définition de colonne dans une migration présentait `table.Column<string>(nullable: true)` et la longueur de la colonne a été recherchée dans des métadonnées que nous stockons dans le code-behind de la migration. Dans la version finale, la longueur est désormais incluse dans le code de modèle généré automatiquement `table.Column<string>(maxLength: 450, nullable: true)`.

Les migrations existantes qui ont été structurées avant d’utiliser la version RTM n’aura pas la `maxLength` argument spécifié. Cela signifie que la longueur maximale prise en charge par la base de données sera utilisée (`nvarchar(max)` sur SQL Server). Cela peut convenir pour certaines colonnes, mais les colonnes qui font partie d’une clé, la clé étrangère, ou les index doivent être mis à jour pour inclure une longueur maximale. Par convention, 450 correspond à la longueur maximale utilisée pour les clés étrangères, clés et les colonnes indexées. Si vous avez explicitement configuré une longueur dans le modèle, vous devez utiliser cette longueur à la place.

**Identité ASP.NET**

Cette modification a un impact sur les projets qui utilisent ASP.NET Identity et ont été créés à partir d’un pré-RTM de modèle de projet. Le modèle de projet inclut une migration permet de créer la base de données. Cette migration doit être modifiée pour spécifier une longueur maximale de `256` sur les colonnes suivantes.

*  **AspNetRoles**

    * Nom

    * NormalizedName

*  **AspNetUsers**

   * Messagerie

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Pour effectuer ce changement entraînera l’exception suivante lors de la migration initiale est appliquée à une base de données.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core : Supprimez « importations » dans project.json

Si vous ciblait .NET Core avec RC2, vous deviez ajouter `imports` à project.json en tant que solution de contournement temporaire pour certaines des dépendances de cœur EF ne pas de prise en charge .NET Standard. Vous pouvez maintenant les supprimer.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a>UWP : Ajouter des redirections de liaison

Essayez d’exécuter des commandes d’EF sur les projets de plateforme Windows universelle (UWP) entraîne l’erreur suivante :

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Vous devez ajouter manuellement des redirections de liaison pour le projet UWP. Créez un fichier nommé `App.config` dans le projet racine du dossier et ajouter des redirections vers les versions correcte de l’assembly.

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
