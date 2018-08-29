---
title: Amélioration des performances de démarrage avec NGen - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 324769ca6ee95e1576cf03d20e569f8b24778119
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998333"
---
# <a name="improving-startup-performance-with-ngen"></a>Amélioration des performances de démarrage avec NGen
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Le .NET Framework prend en charge la génération d’images natives pour les applications gérées et bibliothèques comme un moyen pour aider les applications à démarrer plus rapidement, ainsi que dans certains cas utilisent moins de mémoire. Les images natives sont créés en traduisant les assemblys de code managé dans des fichiers contenant des instructions machine natives avant l’exécution de l’application, ce qui décharge le compilateur .NET JIT (juste-à-temps) d’avoir à générer les instructions natives au exécution de l’application.  

Avant la version 6, les bibliothèques du runtime EF core faisaient partie du .NET Framework et les images natives ont été générés automatiquement pour les. Depuis la version 6 le runtime EF entière a été combiné dans le package EntityFramework NuGet. Les images natives ont à présent être généré à l’aide de l’outil de ligne de commande NGen.exe pour obtenir des résultats similaires.  

Observations empiriques montrent que les images natives des assemblys de runtime EF peuvent couper entre 1 et 3 secondes de temps de démarrage d’application.  

## <a name="how-to-use-ngenexe"></a>L’utilisation de NGen.exe  

La fonction la plus simple de l’outil NGen.exe consiste à « installer » (autrement dit, pour créer et rendre persistantes sur disque) des images natives pour un assembly et toutes ses dépendances directes. Voici comment vous pouvez effectuer cette opération :  

1. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur  
2. Modifier le répertoire de travail actuel à l’emplacement des assemblys que vous souhaitez générer des images natives pour :  

  ``` console
    cd <*Assemblies location*>  
  ```
3. Selon votre système d’exploitation et configuration de l’application, vous devrez peut-être à générer des images natives pour architecture 32 bits, architecture 64 bits ou les deux.  

    Pour 32 bits à exécuter :  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Pour les 64 bits à exécuter :
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Génération d’images natives pour la bonne architecture est très fréquente. Dans le doute, vous pouvez simplement générer des images natives pour toutes les architectures qui s’appliquent au système d’exploitation installé sur l’ordinateur.  

NGen.exe prend également en charge les autres fonctions telles que la désinstallation et afficher les images natives installées, queuing la génération de plusieurs images, etc. Pour plus d’informations sur l’utilisation, consultez le [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Quand utiliser NGen.exe  

Lorsqu’il s’agit de décider quels assemblys pour générer des images natives pour dans une application basée sur Entity Framework 6 ou version ultérieure, vous devez envisager les options suivantes :  

- **Le principal assembly du runtime EF, EntityFramework.dll**: une application Entity Framework en fonction de type exécute une quantité importante de code à partir de cet assembly sur le démarrage ou sur son premier accès à la base de données. Par conséquent, la création d’images natives de cet assembly produira les plus grands gains de performances de démarrage.  
- **N’importe quel assembly de fournisseur Entity Framework utilisé par votre application**: temps de démarrage peuvent également bénéficier légèrement de génération d’images natives d'entre eux. Par exemple, si l’application utilise le fournisseur Entity Framework pour SQL Server vous devez générer une image native pour EntityFramework.SqlServer.dll.  
- **Assemblys de votre application et autres dépendances**: le [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) couvre les critères généraux permettant de choisir quels assemblys pour générer des images natives pour et l’impact des images natives sur la sécurité, options avancées telles que « liaison matérielle », des scénarios tels que l’utilisation d’images natives lors du débogage et profilage des scénarios, etc.  

> [!TIP]
> Assurez-vous que vous avec soin de mesurer l’impact de l’utilisation des images natives sur les performances de démarrage et les performances globales de votre application et de les comparez par rapport à la configuration requise réelle. Tandis que les images natives sont généralement aident à améliorer le démarrage, les performances et dans certains cas, réduire l’utilisation de mémoire, pas tous les scénarios bénéficieront également. Par exemple, lors de l’exécution d’un état stable (autrement dit, une fois que toutes les méthodes utilisées par l’application ont été appelées au moins une fois) code généré par le compilateur JIT peut en fait produire des performances légèrement meilleures que les images natives.  

## <a name="using-ngenexe-in-a-development-machine"></a>À l’aide de NGen.exe dans un ordinateur de développement  

Pendant le développement, le JIT .NET compilateur offrira le meilleur compromis global pour le code qui change fréquemment. Génération d’images natives pour les dépendances compilés tels que les assemblys de runtime EF peut aider à accélérer le développement et le test en découpant quelques secondes au début de chaque exécution.  

Un bon point de rechercher les assemblys de runtime EF est l’emplacement du package NuGet pour la solution. Par exemple, pour une application à l’aide d’EF 6.0.2 avec SQL Server et de ciblage .NET 4.5 ou version ultérieure vous pouvez taper ce qui suit dans une fenêtre d’invite de commandes (n’oubliez pas pour l’ouvrir en tant qu’administrateur) :  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Il tire parti du fait que l’installation des images natives pour le fournisseur Entity Framework pour SQL Server par défaut installe également les images natives pour l’assembly de runtime EF principal. Cela fonctionne car NGen.exe peut détecter que EntityFramework.dll est une dépendance directe de l’assembly EntityFramework.SqlServer.dll situé dans le même répertoire.  

## <a name="creating-native-images-during-setup"></a>Création d’images natives lors de l’installation  

Le WiX Toolkit prend en charge queuing la génération d’images natives pour les assemblys managés pendant l’installation, comme expliqué dans cet [guide de procédure](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Une autre solution consiste à créer une tâche d’installation personnalisée qui exécute la commande NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Vérification que les images natives sont utilisées pour Entity Framework  

Vous pouvez vérifier qu’une application spécifique est à l’aide d’un assembly natif en recherchant des assemblys chargés qui ont l’extension «. ni.dll « ou ». ni.exe ». Par exemple, une image native pour l’assembly de runtime principal de l’Entity Framework s’appellera EntityFramework.ni.dll. Un moyen simple pour inspecter les assemblys .NET chargés d’un processus consiste à utiliser [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Autres choses à connaître  

**Création d’une image native d’un assembly pas confondre avec l’inscription de l’assembly dans le [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe permet la création d’images d’assemblys qui ne figurent pas dans le GAC, et en fait, plusieurs applications qui utilisent une version spécifique d’EF peuvent partager la même image de native. Bien que Windows 8 peut créer automatiquement des images natives pour les assemblys placés dans le GAC, le runtime Entity Framework est optimisé pour être déployé en même temps que votre application et nous ne recommandons pas l’inscription dans le GAC comme cela a un impact négatif sur la résolution d’assembly et vos applications parmi les autres aspects de la maintenance.  
