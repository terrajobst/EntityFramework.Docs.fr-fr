---
title: Conventions de Model-Based - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283705"
---
# <a name="model-based-conventions"></a>Conventions reposant sur un modèle
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.  

Conventions de modèle basé sont une méthode avancée de la configuration de modèle basé sur une convention. La plupart des scénarios le [Custom API Code First Convention sur DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) doit être utilisé. Une présentation de l’API DbModelBuilder pour les conventions est recommandée avant d’utiliser les conventions de modèle en fonction.  

Conventions de modèle en fonction d’autoriser la création de conventions qui affectent des propriétés et les tables qui ne sont pas configurables via les conventions standard. Colonnes de discriminateur dans la table par les modèles de la hiérarchie et les colonnes de l’Association indépendante sont des exemples.  

## <a name="creating-a-convention"></a>Création d’une Convention   

La première étape de création d’une convention de modèle basé choisit quand dans le pipeline de la convention doit être appliquée au modèle. Il existe deux types de conventions de modèles, conceptuel (espace C) et Store (espace S). Une convention espace C est appliquée au modèle de l’application est générée, tandis qu’une convention espace S est appliquée à la version du modèle qui représente la base de données et des contrôles, notamment comment généré automatiquement les colonnes est nommés.  

Une convention de modèle est une classe qui s’étend de IConceptualModelConvention ou IStoreModelConvention.  Ces interfaces, de que les deux acceptent un type générique qui peut être type MetadataItem qui est utilisé pour filtrer le type de données qui s’applique à la convention.  

## <a name="adding-a-convention"></a>Ajout d’une Convention   

Conventions de modèle sont ajoutées dans la même façon que les classes de conventions régulière. Dans le **OnModelCreating** (méthode), ajoutez la convention à la liste de conventions pour un modèle.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

Une convention peut également être ajoutée par rapport à une autre convention à l’aide de la Conventions.AddBefore\< \> ou Conventions.AddAfter\< \> méthodes. Pour plus d’informations sur les conventions Entity Framework s’applique, consultez la section notes.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Exemple : Convention de modèle de discriminateur  

Renommer des colonnes générées par Entity Framework est un exemple de quelque chose que vous ne pouvez pas faire avec les autres conventions API.  Il s’agit d’une situation où à l’aide des conventions de modèle est votre seule option.  

Un exemple montrant comment utiliser une convention en fonction de modèle pour configurer les colonnes générées personnalise la manière dont les colonnes de discriminateur sont nommés.  Voici un exemple d’une convention en fonction de modèle simple qui renomme toutes les colonnes dans le modèle nommé « Discriminator » à « EntityType ».  Cela inclut les colonnes que le développeur doit simplement nommée « Discriminator ». Étant donné que la colonne « Discriminator » est une colonne générée, qu'il doit s’exécuter dans l’espace de S.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Exemple : Des IA général Convention de changement de nom   

Un autre exemple plus complexe des conventions de modèle basé en action consiste à configurer la façon que les Associations indépendantes (IAs) sont nommées.  Il s’agit d’une situation où les conventions de modèles sont applicables, car IAs sont générés par Entity Framework et ne sont pas présents dans le modèle qui peut accéder à l’API DbModelBuilder.  

Lors de l’Entity Framework génère un IA, il crée une colonne nommée EntityType_KeyName. Par exemple, pour une association nommée Customer avec une colonne de clé nommé CustomerId il générerait une colonne nommée Customer_CustomerId. Les bandes de convention suivant le «\_' caractère hors le nom de colonne qui est généré pour l’IA.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Extension des Conventions existantes   

Si vous avez besoin écrire une convention qui est similaire à un des conventions qui applique déjà les Entity Framework à votre modèle, que vous pouvez toujours étendre cette convention pour éviter d’avoir à réécrire de zéro.  Un exemple consiste à remplacer l’Id existant correspondant à la convention avec un personnalisé.   Un avantage supplémentaire à la substitution de la convention de clés est que la méthode substituée est appelée uniquement si aucune clé déjà détecté ou configurée de manière explicite. Une liste des conventions utilisées par Entity Framework est disponible ici : [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

Nous devons ensuite ajouter notre nouvelle convention avant la convention de clés existante. Une fois que nous ajoutons la CustomKeyDiscoveryConvention, nous pouvons supprimer la IdKeyDiscoveryConvention.  Si nous n’avez pas supprimé la IdKeyDiscoveryConvention existante cette convention est toujours prioritaire sur la convention de détection d’Id dans la mesure où il est exécuté tout d’abord, mais dans le cas où aucune propriété de « clé » est trouvée, la convention « id » s’exécute.  Nous voyons ce comportement, car chaque convention voit le modèle comme mise à jour par la convention précédente (plutôt que de travailler indépendamment sur celle-ci et tous être combinés) afin que si, par exemple, une convention précédente mise à jour un nom de colonne pour faire correspondre un élément de présentant un intérêt pour votre convention personnalisée (lorsque avant que le nom n’a pas d’intérêt), il s’applique à cette colonne.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Notes  

Une liste de conventions qui sont actuellement appliquées par Entity Framework est disponible dans la documentation MSDN ici : [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Cette liste est extraite directement à partir de notre code source.  Le code source pour Entity Framework 6 est disponible sur [GitHub](https://github.com/aspnet/entityframework6/) et la plupart des conventions utilisées par Entity Framework sont bon point de départ pour les modèles personnalisés en fonction des conventions.  
