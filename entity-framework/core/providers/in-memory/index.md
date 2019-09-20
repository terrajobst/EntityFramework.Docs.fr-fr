---
title: Fournisseur de base de données InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149228"
---
# <a name="ef-core-in-memory-database-provider"></a>Fournisseur de base de données InMemory EF Core

Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire. Ceci peut s’avérer utile pour les tests, bien que le fournisseur SQLite en mode en mémoire constitue peut-être un remplacement de test plus approprié pour les bases de données relationnelles. Il est géré dans le cadre du [projet Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installez

Installez le [package NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Bien démarrer

Les ressources suivantes vous permettent de commencer à utiliser ce fournisseur.
* [Test avec InMemory](../../miscellaneous/testing/in-memory.md)

* [Exemple de tests d’application UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Moteurs de base de données pris en charge

* Base de données en mémoire intégrée (conçue uniquement à des fins de test)
