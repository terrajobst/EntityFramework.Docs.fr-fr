---
title: Personnalisation de la table d’historique de migrations - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: e3faefc4b812ec4bc440ed2bb48747053d8cb1b3
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283691"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="882c9-102">Personnalisation de la table d’historique de migrations</span><span class="sxs-lookup"><span data-stu-id="882c9-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="882c9-103">**EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="882c9-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="882c9-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.</span><span class="sxs-lookup"><span data-stu-id="882c9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="882c9-105">Cet article suppose que vous savez comment utiliser Code First Migrations dans des scénarios de base.</span><span class="sxs-lookup"><span data-stu-id="882c9-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="882c9-106">Si vous n’avez pas, vous devez lire [Migrations Code First](~/ef6/modeling/code-first/migrations/index.md) avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="882c9-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="882c9-107">Quelle est la Table d’historique de Migrations ?</span><span class="sxs-lookup"><span data-stu-id="882c9-107">What is Migrations History Table?</span></span>

<span data-ttu-id="882c9-108">Table d’historique de migrations est une table utilisée par les Migrations Code First pour stocker les détails sur les migrations appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="882c9-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="882c9-109">Par défaut est le nom de la table dans la base de données \_ \_MigrationHistory et il est créé lors de l’application de la première ne de migration la base de données.</span><span class="sxs-lookup"><span data-stu-id="882c9-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="882c9-110">Dans Entity Framework 5 cette table était une table système si l’application utilise la base de données Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="882c9-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="882c9-111">Cela a changé dans Entity Framework 6 toutefois et la table d’historique de migrations n’est plus marquée une table système.</span><span class="sxs-lookup"><span data-stu-id="882c9-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="882c9-112">Pourquoi personnaliser les Migrations de la Table d’historique ?</span><span class="sxs-lookup"><span data-stu-id="882c9-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="882c9-113">Table d’historique de migrations est censé être utilisé uniquement par les Migrations Code First et changer manuellement peut interrompre les migrations.</span><span class="sxs-lookup"><span data-stu-id="882c9-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="882c9-114">Cependant parfois la configuration par défaut ne convient pas, et la table doit être personnalisé, par exemple :</span><span class="sxs-lookup"><span data-stu-id="882c9-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="882c9-115">Vous devez modifier les noms et/ou des facettes des colonnes pour activer un 3<sup>Bureau à distance</sup> fournisseur de Migrations de tiers</span><span class="sxs-lookup"><span data-stu-id="882c9-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="882c9-116">Vous souhaitez modifier le nom de la table</span><span class="sxs-lookup"><span data-stu-id="882c9-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="882c9-117">Vous devez utiliser un schéma par défaut pour le \_ \_MigrationHistory table</span><span class="sxs-lookup"><span data-stu-id="882c9-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="882c9-118">Vous devez stocker des données supplémentaires pour une version donnée du contexte et par conséquent, vous devez ajouter une colonne supplémentaire à la table</span><span class="sxs-lookup"><span data-stu-id="882c9-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="882c9-119">Mots de précaution</span><span class="sxs-lookup"><span data-stu-id="882c9-119">Words of precaution</span></span>

<span data-ttu-id="882c9-120">Modification de la table d’historique de migration est puissante, mais vous devez faire attention à ne pas abuser de cette possibilité.</span><span class="sxs-lookup"><span data-stu-id="882c9-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="882c9-121">Runtime EF actuellement ne pas vérifie si la table d’historique des migrations personnalisé est compatible avec le runtime.</span><span class="sxs-lookup"><span data-stu-id="882c9-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="882c9-122">Si elle n’est pas votre application peut rompre lors de l’exécution ou se comportent de manière imprévisible.</span><span class="sxs-lookup"><span data-stu-id="882c9-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="882c9-123">Ceci est encore plus important si vous utilisez plusieurs contextes par base de données auquel cas plusieurs contextes peuvent utiliser la même table d’historique de migration pour stocker des informations sur les migrations.</span><span class="sxs-lookup"><span data-stu-id="882c9-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="882c9-124">Comment personnaliser les Migrations de la Table d’historique ?</span><span class="sxs-lookup"><span data-stu-id="882c9-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="882c9-125">Avant de commencer, vous devez savoir que vous pouvez personnaliser la table d’historique de migrations uniquement avant d’appliquer la première migration.</span><span class="sxs-lookup"><span data-stu-id="882c9-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="882c9-126">Maintenant, pour le code.</span><span class="sxs-lookup"><span data-stu-id="882c9-126">Now, to the code.</span></span>

<span data-ttu-id="882c9-127">Tout d’abord, vous devez créer une classe dérivée de la classe de System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="882c9-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="882c9-128">La classe HistoryContext est dérivée de la classe DbContext afin de la configuration de la table d’historique des migrations est très similaire à la configuration des modèles EF avec l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="882c9-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="882c9-129">Vous devez remplacer la méthode OnModelCreating et utiliser des API fluent pour configurer la table.</span><span class="sxs-lookup"><span data-stu-id="882c9-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="882c9-130">En général, lorsque vous configurez des modèles EF ne pas devoir appeler la base. OnModelCreating() à partir de la méthode OnModelCreating substituée dans la mesure où le DbContext.OnModelCreating() a un corps vide.</span><span class="sxs-lookup"><span data-stu-id="882c9-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="882c9-131">Cela n’est pas le cas lors de la configuration de la table d’historique de migrations.</span><span class="sxs-lookup"><span data-stu-id="882c9-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="882c9-132">Dans ce cas, la première chose à faire dans votre remplacement OnModelCreating() est en fait appeler la base. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="882c9-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="882c9-133">Cela configurera la table d’historique de migrations de la façon par défaut qui vous ajustez ensuite dans la méthode de substitution.</span><span class="sxs-lookup"><span data-stu-id="882c9-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="882c9-134">Supposons que vous souhaitez renommer la table d’historique de migrations et placez-le à un schéma personnalisé appelé « administrateur ».</span><span class="sxs-lookup"><span data-stu-id="882c9-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="882c9-135">En outre votre DBA souhaitons que vous renommez la colonne MigrationId Migration\_ID.</span><span class="sxs-lookup"><span data-stu-id="882c9-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="882c9-136">Vous pourriez obtenir cela en créant la classe suivante dérivée HistoryContext :</span><span class="sxs-lookup"><span data-stu-id="882c9-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="882c9-137">Une fois que votre HistoryContext personnalisée est prête vous devez faire EF prenant en charge de celui-ci en vous inscrivant via [configuration basée sur le code](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="882c9-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="882c9-138">C’est à peu près tout.</span><span class="sxs-lookup"><span data-stu-id="882c9-138">That’s pretty much it.</span></span> <span data-ttu-id="882c9-139">Maintenant que vous pouvez accéder à la Console du Gestionnaire de Package, Enable-Migrations, Add-Migration et enfin de mise à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="882c9-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="882c9-140">Cela doit entraîner l’ajout à la base de données une table d’historique de migrations configurée en fonction des détails que vous avez spécifié dans votre classe dérivée de HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="882c9-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Base de données](~/ef6/media/database.png)
