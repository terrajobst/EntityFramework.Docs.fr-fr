---
title: Migrations - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996520"
---
<a name="migrations"></a>Migrations
==========
Les migrations permettent d’appliquer de façon incrémentielle des modifications de schéma à la base de données pour qu’elle demeure synchronisée avec votre modèle EF Core tout en conservant les données existantes dans la base de données.

<a name="creating-the-database"></a>Création de la base de données
---------------------
Une fois que vous avez [défini votre modèle initial][1], il est temps de créer la base de données. Pour ce faire, ajoutez une migration initiale.
Installez les [outils EF Core][2] et exécutez la commande appropriée.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Trois fichiers sont ajoutés à votre projet sous le répertoire **Migrations** :

* **00000000000000_InitialCreate.cs** : fichier principal des migrations. Contient les opérations nécessaires à l’application de la migration (dans `Up()`) et à sa restauration (dans `Down()`).
* **00000000000000_InitialCreate.Designer.cs** : fichier de métadonnées des migrations. Contient des informations utilisées par EF.
* **MyContextModelSnapshot.cs** : instantané de votre modèle actuel. Permet de déterminer ce qui a changé pendant l’ajout de la migration suivante.

L’horodatage dans le nom des fichiers permet de conserver ces derniers dans l’ordre chronologique et de voir ainsi la progression des modifications.

> [!TIP]
> Vous êtes libre de déplacer les fichiers Migrations et de changer leur espace de noms. Les migrations sont créées en tant que sœurs de la dernière migration.

Ensuite, appliquez la migration à la base de données pour créer le schéma.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Ajout d’une autre migration
------------------------
Une fois votre modèle EF Core modifié, le schéma de base de données n’est plus synchronisé. Pour le mettre à jour, ajoutez une autre migration. Le nom de la migration peut être utilisé comme un message de validation dans un système de gestion de versions. Par exemple, si j’ai apporté des modifications pour enregistrer des avis de clients sur des produits, je peux choisir quelque chose comme *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Une fois la migration structurée, vous devez vérifier sa précision et ajouter toutes les opérations supplémentaires nécessaires à son application. Par exemple, la migration peut contenir les opérations suivantes :

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Bien que ces opérations rendent le schéma de base de données compatible, elles ne conservent pas les noms de client existants. Pour l’améliorer, réécrivez la migration comme suit.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> L’ajout d’une nouvelle migration émet un avertissement si la structuration d’une opération risque d’entraîner une perte de données (telle que la suppression d’une colonne). Veillez à vérifier tout particulièrement la précision de ces migrations.

Appliquez la migration à la base de données à l’aide de la commande appropriée.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Suppression d’une migration
--------------------
Parfois, vous ajoutez une migration et réalisez que vous devez apporter des modifications supplémentaires à votre modèle EF Core avant de l’appliquer.
Pour supprimer la dernière migration, utilisez cette commande.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Après l’avoir supprimée, vous pouvez apporter les modifications supplémentaires au modèle et la rajouter.

<a name="reverting-a-migration"></a>Restauration d’une migration
---------------------
Si vous avez déjà appliqué une migration (ou plusieurs migrations) à la base de données, mais que vous devez la restaurer, vous pouvez utiliser la même commande que celle servant à appliquer des migrations, mais en spécifiant le nom de la migration à restaurer.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Migrations vides
----------------
Il est parfois utile d’ajouter une migration sans apporter de modification au modèle. Dans ce cas, l’ajout d’une nouvelle migration crée une migration vide. Vous pouvez personnaliser cette migration pour effectuer des opérations qui ne sont pas directement liées au modèle EF Core.
Voici quelques exemples de ce que vous pouvez gérer de cette façon :

* Recherche en texte intégral
* Fonctions
* Procédures stockées
* Déclencheurs
* Affichages
* Etc.

<a name="generating-a-sql-script"></a>Génération d’un script SQL
-----------------------
Quand vous déboguez vos migrations ou que vous les déployez sur une base de données de production, il est utile de générer un script SQL. Vous pouvez ensuite revoir le script et l’affiner en fonction des besoins d’une base de données de production. Vous pouvez également utiliser le script conjointement avec une technologie de déploiement. La commande de base est la suivante.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Il existe plusieurs options pour cette commande.

La migration **from** doit être la dernière migration appliquée à la base de données avant l’exécution du script. Si aucune migration n’a été appliquée, spécifiez `0` (il s’agit de la valeur par défaut).

La migration **to** est la dernière migration à appliquer à la base de données après l’exécution du script. Par défaut, il s’agit de la dernière migration dans votre projet.

Un script **idempotent** peut également être généré. Ce script n’applique les migrations que si elles n’ont pas déjà été appliquées à la base de données. Cela est utile si vous ne savez pas exactement ce que la dernière migration a appliqué à la base de données ou si vous effectuez un déploiement sur plusieurs bases de données pouvant chacune être liée à une migration différente.

<a name="applying-migrations-at-runtime"></a>Application de migrations au moment de l’exécution
------------------------------
Certaines applications sont susceptibles d’appliquer des migrations au moment de l’exécution (au démarrage ou à la première exécution). Ces opérations nécessitent l’utilisation de la méthode `Migrate()`.

Attention, cette approche ne s’adresse pas à tout un chacun. Bien qu’elle soit idéale pour les applications avec une base de données locale, la plupart des applications nécessitent une stratégie de déploiement plus robuste, telle que la génération de scripts SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> N’appelez pas `EnsureCreated()` avant `Migrate()`. `EnsureCreated()` ignore Migrations pour créer le schéma, ce qui entraîne l’échec de `Migrate()`.

> [!NOTE]
> Cette méthode s’appuie sur le service `IMigrator`, qui peut être utilisé pour des scénarios plus avancés. Utilisez `DbContext.GetService<IMigrator>()` pour y accéder.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
