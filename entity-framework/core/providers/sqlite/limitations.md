---
title: "Base de données SQLite fournisseur - Limitations - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limites du fournisseur de base de données SQLite EF Core

Le fournisseur de SQLite a un nombre de limitations des migrations. La plupart de ces limitations est le résultat des limitations dans le moteur de base de données SQLite sous-jacent et n’est pas spécifique à EF.

## <a name="modeling-limitations"></a>Limitations de modélisation

La bibliothèque de relationnelle commune (partagée par les fournisseurs de base de données relationnelle d’Entity Framework) définit des API pour la modélisation des concepts qui sont communes à la plupart des moteurs de base de données relationnelle. Plusieurs de ces concepts ne sont pas pris en charge par le fournisseur de SQLite.

* Schémas
* Séquences

## <a name="migrations-limitations"></a>Limitations de migration

Le moteur de base de données SQLite ne prend pas en charge un nombre d’opérations de schéma qui sont pris en charge par la plupart des autres bases de données relationnelles. Si vous essayez d’appliquer l’une des opérations non prises en charge pour une base de données SQLite un `NotSupportedException` sera levée.

| Opération            | Prise en charge ? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| Create table          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DROP index            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Solution de contournement migrations limitations

Vous pouvez contourner certaines de ces limitations en écrivant du code dans vos migrations pour effectuer une table manuellement régénérer. Une régénération de la table implique la modification du nom de la table existante, créez une table, copie de données vers la nouvelle table et la suppression de l’ancienne table. Vous devez utiliser le `Sql(string)` méthode pour effectuer certaines de ces étapes.

Consultez [rendre autres types de Table de modifications de schéma](http://sqlite.org/lang_altertable.html#otheralter) dans la documentation de SQLite pour plus de détails.

À l’avenir, EF peut prendre en charge certaines de ces opérations à l’aide de l’approche de reconstruction de table en arrière-plan. Vous pouvez [suivre cette fonctionnalité sur notre projet GitHub](https://github.com/aspnet/EntityFramework/issues/329).