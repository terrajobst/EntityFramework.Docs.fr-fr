---
title: Personnalisation de la table d’historique de migrations - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058745"
---
# <a name="customizing-the-migrations-history-table"></a>Personnalisation de la table d’historique de migrations
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

> [!NOTE]
> Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base. Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.

## <a name="what-is-migrations-history-table"></a>Quelle est la Table d’historique de Migrations ?

Table d’historique de migrations est une table utilisée par les Migrations Code First pour stocker les détails sur les migrations appliquées à la base de données. Par défaut est le nom de la table dans la base de données \_ \_MigrationHistory et il est créé lors de l’application de la première migration à la base de données. Dans Entity Framework 5 cette table était une table système si l’application utilise la base de données Microsoft Sql Server. Cela a changé dans Entity Framework 6 toutefois et la table d’historique de migrations n’est plus marquée une table système.

## <a name="why-customize-migrations-history-table"></a>Pourquoi personnaliser les Migrations de la Table d’historique ?

Table d’historique de migrations est censé être utilisé uniquement par les Migrations Code First et changer manuellement peut interrompre les migrations. Cependant parfois la configuration par défaut ne convient pas, et la table doit être personnalisé, par exemple :

-   Vous devez modifier les noms et/ou des facettes des colonnes pour activer un 3<sup>Bureau à distance</sup> fournisseur de Migrations de tiers
-   Vous souhaitez modifier le nom de la table
-   Vous devez utiliser un schéma par défaut pour le \_ \_MigrationHistory table
-   Vous devez stocker des données supplémentaires pour une version donnée du contexte et par conséquent, vous devez ajouter une colonne supplémentaire à la table

## <a name="words-of-precaution"></a>Mots de précaution

Modification de la table d’historique de migration est puissante, mais vous devez faire attention à ne pas abuser de cette possibilité. Runtime EF actuellement ne pas vérifie si la table d’historique des migrations personnalisé est compatible avec le runtime. Si elle n’est pas votre application peut rompre lors de l’exécution ou se comportent de manière imprévisible. Ceci est encore plus important si vous utilisez plusieurs contextes par base de données auquel cas plusieurs contextes peuvent utiliser la même table d’historique de migration pour stocker des informations sur les migrations.

## <a name="how-to-customize-migrations-history-table"></a>Comment personnaliser les Migrations de la Table d’historique ?

Avant de commencer, vous devez savoir que vous pouvez personnaliser la table d’historique de migrations uniquement avant d’appliquer la première migration. Maintenant, pour le code.

Tout d’abord, vous devez créer une classe dérivée de la classe de System.Data.Entity.Migrations.History.HistoryContext. La classe HistoryContext est dérivée de la classe DbContext afin de la configuration de la table d’historique des migrations est très similaire à la configuration des modèles EF avec l’API fluent. Vous devez remplacer la méthode OnModelCreating et utiliser des API fluent pour configurer la table.

>[!NOTE]
> En général, lorsque vous configurez des modèles EF ne pas devoir appeler la base. OnModelCreating() à partir de la méthode OnModelCreating substituée dans la mesure où le DbContext.OnModelCreating() a un corps vide. Cela n’est pas le cas lors de la configuration de la table d’historique de migrations. Dans ce cas, la première chose à faire dans votre remplacement OnModelCreating() est en fait appeler la base. OnModelCreating(). Cela configurera la table d’historique de migrations de la façon par défaut qui vous ajustez ensuite dans la méthode de substitution.

Supposons que vous souhaitez renommer la table d’historique de migrations et placez-le à un schéma personnalisé appelé « administrateur ». En outre votre DBA souhaitons que vous renommez la colonne MigrationId Migration\_ID.  Vous pourriez obtenir cela en créant la classe suivante dérivée HistoryContext :

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Une fois que votre HistoryContext personnalisée est prête vous devez faire EF prenant en charge de celui-ci en vous inscrivant via [configuration basée sur le code](https://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

C’est à peu près tout. Maintenant que vous pouvez accéder à la Console du Gestionnaire de Package, Enable-Migrations, Add-Migration et enfin de mise à jour la base de données. Cela doit entraîner l’ajout à la base de données une table d’historique de migrations configurée en fonction des détails que vous avez spécifié dans votre classe dérivée de HistoryContext.

![Base de données](~/ef6/media/database.png)
