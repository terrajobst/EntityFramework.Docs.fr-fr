---
title: Prise en charge de fournisseur pour les Types spatiaux - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022273"
---
# <a name="provider-support-for-spatial-types"></a>Prise en charge de fournisseur pour les Types spatiaux
Entity Framework prend en charge l’utilisation des données spatiales via les classes de DbGeography ou DbGeometry. Ces classes s’appuient sur les fonctionnalités spécifiques à la base de données offertes par le fournisseur Entity Framework. Pas tous les fournisseurs prennent en charge les données spatiales et les autres peuvent avoir des conditions préalables supplémentaires telles que l’installation des assemblys de type spatial. Vous trouverez ci-dessous plus d’informations sur la prise en charge de fournisseur pour les types spatiaux.  

Vous trouverez des informations supplémentaires sur l’utilisation des types spatiaux dans une application dans deux procédures, une pour Code First, l’autre pour Database First ou Model First :  

- [Types de données spatiales dans le Code tout d’abord](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Types de données spatiales dans Entity Framework Designer](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versions d’Entity Framework qui prennent en charge les types de données spatiales  

Prise en charge pour les types spatiaux a été introduite dans EF5. Toutefois, dans EF5 types spatiaux sont uniquement pris en charge quand l’application cible et s’exécute sur .NET 4.5.  

À compter de EF6 types spatiaux sont pris en charge pour les applications ciblant le .NET 4 et .NET 4.5.  

## <a name="ef-providers-that-support-spatial-types"></a>Fournisseurs EF qui prennent en charge les types de données spatiales  

### <a name="ef5"></a>EF5  

Les fournisseurs d’Entity Framework pour EF5 qui nous prennent en charge que les types spatiaux prise en charge sont :  

- Fournisseur Microsoft SQL Server  
    - Ce fournisseur est partie intégrante d’EF5.  
    - Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires devant être installé, voir ci-dessous pour plus d’informations.  
- [Devart dotconnect relative pour Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Il s’agit d’un fournisseur tiers à partir de Devart.  

Si vous connaissez d’un fournisseur d’EF5 prenant en charge les types spatiaux puis Veuillez contacter et nous serons heureux de vous ajouter à cette liste.  

### <a name="ef6"></a>EF6  

Les fournisseurs d’Entity Framework pour EF6 qui nous prennent en charge que les types spatiaux prise en charge sont :  

- Fournisseur Microsoft SQL Server  
    - Ce fournisseur est partie intégrante d’EF6.  
    - Ce fournisseur dépend de certaines bibliothèques de bas niveau supplémentaires devant être installé, voir ci-dessous pour plus d’informations.  
- [Devart dotconnect relative pour Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Il s’agit d’un fournisseur tiers à partir de Devart.  

Si vous connaissez d’un fournisseur d’EF6 prenant en charge les types spatiaux puis Veuillez contacter et nous serons heureux de vous ajouter à cette liste.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Conditions préalables pour les types spatiaux avec Microsoft SQL Server  

Prise en charge spatiale SQL Server dépend des types spécifiques de SQL Server bas niveau, SqlGeography et SqlGeometry. Ces types se trouvent dans l’assembly de Microsoft.SqlServer.Types.dll, et cet assembly n’est pas fourni dans le cadre d’EF ou en tant que partie du .NET Framework.  

Lorsque Visual Studio est installé il installe souvent également une version de SQL Server, et cela inclut l’installation du fichier Microsoft.SqlServer.Types.dll.  

Si SQL Server n’est pas installé sur l’ordinateur où vous souhaitez utiliser des types de données spatiales, ou si les types spatiaux ont été exclus de l’installation de SQL Server, vous devrez les installer manuellement. Les types peuvent être installés à l’aide de `SQLSysClrTypes.msi`, qui fait partie du Feature Pack Microsoft SQL Server. Types de données spatiales sont SQL Server spécifique à la version, nous vous recommandons de [recherche pour « Feature Pack SQL Server »](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) dans le Microsoft Download Center, puis sélectionnez et téléchargez l’option qui correspond à la version de SQL Server, vous allez utiliser.
