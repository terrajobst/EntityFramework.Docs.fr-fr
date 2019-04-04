---
title: Fournisseurs de base de données - EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 3748496db89c110d55a0876727e33e1f3ec987d9
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914089"
---
# <a name="database-providers"></a>Fournisseurs de bases de données

Entity Framework Core peut accéder à différentes bases de données par le biais de bibliothèques plug-in appelées fournisseurs de base de données.

## <a name="current-providers"></a>Fournisseurs actuels
> [!IMPORTANT]  
> Les fournisseurs EF Core sont créés à partir de différentes sources. Les fournisseurs ne sont pas tous gérés dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Quand vous envisagez un fournisseur, veillez à évaluer la qualité, la gestion des licences, la prise en charge, etc., pour vérifier qu’il répond à vos besoins. Veillez également à passer en revue la documentation de chaque fournisseur pour obtenir des informations détaillées sur la compatibilité des versions.

| Package NuGet                                                                                                        | Moteurs de base de données pris en charge | Chargé de maintenance / fournisseur                                                           | Remarques / exigences | Liens utiles                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 et ultérieur    | [Projet EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 et ultérieur         | [Projet EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Fournisseur en mémoire EF Core | [Projet EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | À des fins de test uniquement     | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | API SQL Azure Cosmos DB    | [Projet EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Préversion uniquement         | [blog](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Équipe de développement Npgsql](https://github.com/npgsql)                          |                      | [docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projet Pomelo Foundation](https://github.com/PomeloFoundation)              |                      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Serveur MyCAT               | [Projet Pomelo Foundation](https://github.com/PomeloFoundation)              | Version préliminaire uniquement      | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 et 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 et 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projet MySQL](http://dev.mysql.com) (Oracle)                                |                      | [docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 et versions ultérieures     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Version préliminaire           | [site web](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Version Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Version Linux        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Version macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Fichiers Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 et versions ultérieures  | [DevArt](https://www.devart.com/)                                             | Payé                 | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 et ultérieur     | [DevArt](https://www.devart.com/)                                             | Payé                 | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 et ultérieur           | [DevArt](https://www.devart.com/)                                             | Payé                 | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 et ultérieur            | [DevArt](https://www.devart.com/)                                             | Payé                 | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="future-providers"></a>Futurs fournisseurs

### <a name="cosmos-db"></a>Cosmos DB

Nous développons actuellement un fournisseur EF Core pour l’API SQL dans Cosmos DB.
Il s’agit du premier fournisseur de base de données complet orienté documents que nous produisons. Les retours de cet exercice nous aideront à déterminer les améliorations conceptuelles à apporter aux prochaines versions d’EF Core et, éventuellement, à d’autres fournisseurs non relationnels.
Une préversion est disponible dans la [galerie NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos).

### <a name="oracle-first-party-provider"></a>Fournisseur interne Oracle
L’équipe Oracle .NET a publié la version bêta du [fournisseur Oracle pour EF Core](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/).
Veuillez adresser vos questions concernant ce fournisseur, y compris la chronologie de la publication, au [site de la communauté Oracle](https://community.oracle.com/).

## <a name="adding-a-database-provider-to-your-application"></a>Ajout d’un fournisseur de base de données à votre application

La plupart des fournisseurs de base de données pour EF Core sont distribués sous la forme de packages NuGet. Cela signifie que vous pouvez les installer à l’aide de l’outil `dotnet` dans la ligne de commande :

``` console
dotnet add package provider_package_name
```

Ou, dans Visual Studio, à l’aide de la console du Gestionnaire de package de NuGet :

``` powershell
install-package provider_package_name
```

Une fois les packages installés, configurez le fournisseur dans votre `DbContext` : soit dans la méthode `OnConfiguring`, soit dans la méthode `AddDbContext` si vous utilisez un conteneur d’injection de dépendance.
Par exemple, la ligne suivante configure le fournisseur SQL Server avec la chaîne de connexion passée :

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Les fournisseurs de base de données peuvent étendre Core EF pour activer des fonctionnalités propres à des bases de données spécifiques.
Certains concepts, communs à la plupart des bases de données, sont inclus dans les principaux composants d’EF Core.
Ces concepts comprennent l’expression des requêtes dans LINQ, les transactions et le suivi des changements apportés aux objets une fois qu’ils sont chargés à partir de la base de données.
Certains concepts sont propres à un fournisseur.
Par exemple, le fournisseur SQL Server vous permet de [configurer des tables à mémoire optimisée](xref:core/providers/sql-server/memory-optimized-tables) (fonctionnalité spécifique à SQL Server).
D’autres concepts sont propres à une classe de fournisseurs.
Par exemple, les fournisseurs EF Core pour les bases de données relationnelles reposent sur la bibliothèque `Microsoft.EntityFrameworkCore.Relational` commune, qui fournit des API pour configurer les mappages de tables et de colonnes, les contraintes de clé étrangère, etc. Les fournisseurs sont généralement distribués sous la forme de packages NuGet.

> [!IMPORTANT]  
> Quand une nouvelle version corrective d’EF Core est publiée, des mises à jour au package `Microsoft.EntityFrameworkCore.Relational` sont souvent incluses.
> Quand vous ajoutez un fournisseur de base de données relationnelle, ce package devient une dépendance transitive de votre application.
> Mais de nombreux fournisseurs sont publiés indépendamment d’EF et peuvent ne pas être mis à jour pour dépendre de la nouvelle version corrective de ce package.
> Pour que vous receviez tous les correctifs, nous vous recommandons d’ajouter la version corrective de `Microsoft.EntityFrameworkCore.Relational` comme dépendance directe de votre application.
