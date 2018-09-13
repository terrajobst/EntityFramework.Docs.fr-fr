---
title: Utilisation de proxys - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489815"
---
# <a name="working-with-proxies"></a>Utilisation de proxys
Lorsque vous créez des instances de types d’entités POCO, Entity Framework crée souvent des instances d’un type dérivé généré dynamiquement qui agit comme un proxy pour l’entité. Ce proxy substitue certaines propriétés virtuelles de l’entité à insérer des raccordements pour effectuer des actions automatiquement lorsque la propriété est accessible. Par exemple, ce mécanisme est utilisé pour prendre en charge le chargement différé des relations. Les techniques présentées dans cette rubrique s’appliquent également aux modèles créés avec Code First et EF Designer.  

## <a name="disabling-proxy-creation"></a>La désactivation de la création de proxy  

Il est parfois utile empêcher la création d’instances de proxy d’Entity Framework. Par exemple, la sérialisation des instances de proxy non est considérablement plus facile que la sérialisation des instances de proxy. La création de proxy peut être désactivée en désactivant l’indicateur ProxyCreationEnabled. Un seul endroit, vous pouvez procéder ainsi est dans le constructeur de votre contexte. Exemple :  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Notez que l’Entity Framework ne crée pas de proxys pour les types où il n’existe rien pour le proxy à faire. Cela signifie que vous pouvez également éviter les proxys grâce à des types qui sont scellés et/ou disposent pas de propriétés virtuels.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Création explicite d’une instance d’un proxy  

Une instance de proxy ne sera pas créée si vous créez une instance d’une entité à l’aide de l’opérateur new. Il peut s’agir d’un problème, mais si vous avez besoin créer une instance de proxy (par exemple, pour que différé le chargement ou le proxy le suivi des modifications fonctionnent) puis vous pouvez le faire à l’aide de la méthode Create de DbSet. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

La version générique de création peut être utilisée si vous souhaitez créer une instance d’un type d’entité dérivé. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Notez que la méthode Create ne pas ajouter ou joindre l’entité créée dans le contexte.  

Notez que la méthode Create ne crée simplement une instance du type d’entité si la création d’un type de proxy pour l’entité n’aurait aucune valeur, car il ne ferait rien. Par exemple, si le type d’entité est scellé et/ou ne possède aucune propriété virtuelle créer créer simplement une instance du type d’entité.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Obtenir le type d’entité réelle à partir d’un type de proxy  

Types de proxy ont des noms qui ressemblent à ceci :  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Vous pouvez trouver le type d’entité pour ce type de proxy à l’aide de la méthode GetObjectType a d’ObjectContext. Exemple :  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Notez que si le type passé à GetObjectType a est une instance d’un type d’entité qui n’est pas un type de proxy, le type d’entité est toujours retournée. Cela signifie que vous pouvez toujours utiliser cette méthode pour obtenir le type d’entité réel sans aucune vérification pour voir si le type est un type de proxy ou non.  
