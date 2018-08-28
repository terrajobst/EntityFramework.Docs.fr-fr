---
title: En sélectionnant la Version du Runtime Entity Framework pour les modèles de Concepteur EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998240"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>En sélectionnant la Version du Runtime Entity Framework pour les modèles de Concepteur EF
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

L’écran suivant depuis EF6 a été ajoutée au Concepteur d’Entity Framework vous permettent de sélectionner la version du runtime à cibler lors de la création d’un modèle. L’écran s’affiche lorsque la dernière version d’Entity Framework n’est pas déjà installée dans le projet. Si la version la plus récente est déjà installée. il sera uniquement utilisé par défaut.

![Écran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Ciblage EF6.x

Vous pouvez choisir d’EF6 à partir de l’écran « Choisir votre Version » pour ajouter le runtime EF6 à votre projet. Une fois que vous avez ajouté EF6, vous allez cessent de se voir cet écran dans le projet actuel.

EF6 va être désactivé si vous avez déjà une version antérieure d’EF installé (étant donné que vous ne pouvez pas cibler plusieurs versions du runtime à partir du même projet). Si l’option d’EF6 n’est pas activée ici, suivez ces étapes pour mettre à niveau votre projet EF6 :

1.  Avec le bouton droit sur votre projet dans l’Explorateur de solutions et sélectionnez **gérer les Packages NuGet...**
2.  Sélectionnez **mises à jour**
3.  Sélectionnez **EntityFramework** (Assurez-vous qu’elle va mettre à jour vers la version souhaitée)
4.  Cliquez sur **mise à jour**

 

## <a name="targeting-ef5x"></a>Ciblage EF5.x

Vous pouvez choisir d’EF5 à partir de l’écran « Choisir votre Version » pour ajouter le runtime EF5 à votre projet. Une fois que vous avez ajouté EF5, vous continuerez à voir l’écran avec l’option EF6 désactivée.

Si vous avez une version EF4.x déjà installée du runtime vous verrez que la version d’EF répertorié dans l’écran, plutôt que dans EF5. Dans ce cas, vous pouvez mettre à niveau vers EF5 en procédant comme suit :

1.  Sélectionnez **Tools -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**
2.  Exécutez **Install-Package EntityFramework-version 5.0.0**

 

## <a name="targeting-ef4x"></a>Ciblage EF4.x

Vous pouvez installer le runtime EF4.x à votre projet en procédant comme suit :

1.  Sélectionnez **Tools -&gt; Library Package Manager -&gt; Console du Gestionnaire de Package**
2.  Exécutez **Install-Package EntityFramework-version 4.3.0**
