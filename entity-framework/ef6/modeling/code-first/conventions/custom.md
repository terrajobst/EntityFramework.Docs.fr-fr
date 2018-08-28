---
title: Conventions personnalisées Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995826"
---
# <a name="custom-code-first-conventions"></a>Conventions personnalisées Code First
> [!NOTE]
> **EF6 et versions ultérieures uniquement** : Les fonctionnalités, les API, etc. décrites dans cette page ont été introduites dans Entity Framework 6. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas.

Lorsque vous utilisez Code First, votre modèle est calculé à partir de vos classes à l’aide d’un ensemble de conventions. La valeur par défaut [Conventions Code First](~/ef6/modeling/code-first/conventions/built-in.md) déterminer des choses comme qui propriété devient la clé primaire d’une entité, le nom de la table d’une entité est mappé à, et que la précision et l’échelle une colonne decimal a par défaut.

Parfois, ces conventions par défaut ne sont pas adaptées à votre modèle, et vous devez y remédier en configurant des nombreuses entités individuelles à l’aide des Annotations de données ou l’API Fluent. Conventions Code First personnalisées vous permettent de définir vos propres conventions qui fournissent des valeurs par défaut de configuration pour votre modèle. Dans cette procédure pas à pas, nous allons examiner les différents types de conventions personnalisées et comment créer chacun d’eux.


## <a name="model-based-conventions"></a>Conventions reposant sur un modèle

Cette page traite de l’API DbModelBuilder pour conventions personnalisées. Cette API doit être suffisante pour la création de la plupart des conventions personnalisées. Toutefois, il est également la capacité à créer des conventions basée sur modèle - conventions qui manipulent le modèle final qui a été créé - pour gérer des scénarios avancés. Pour plus d’informations, consultez [Conventions reposant sur le modèle](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Notre modèle

Commençons par définir un modèle simple que nous pouvons utiliser avec nos conventions. Ajoutez les classes suivantes à votre projet.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Présentation de Conventions personnalisées

Nous allons écrire une convention qui configure de n’importe quelle propriété de clé qui sera la clé primaire pour son type d’entité nommée.

Conventions sont activées sur le Générateur de modèles, qui est accessible en substituant OnModelCreating dans le contexte. Mettre à jour la classe ProductContext comme suit :

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Maintenant, n’importe quelle propriété dans notre modèle nommé clé sera configuré en tant que la clé primaire de toute entité sa partie de.

Nous pourrions également souhaiter que nos conventions plus spécifique en filtrant sur le type de propriété que nous allons configurer :

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Cette opération configure toutes les propriétés appelées clé primaire de clé de l’entité, mais uniquement si elles sont un entier.

Une fonctionnalité intéressante de la méthode IsKey est qu’il est additif. Ce qui signifie que si vous appelez IsKey sur plusieurs propriétés et ils deviennent tous partie d’une clé composite. Pour cela l’inconvénient est que lorsque vous spécifiez plusieurs propriétés pour une clé vous devez également spécifier un ordre de ces propriétés. Pour cela, en appelant le HasColumnOrder méthode comme ci-dessous :

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Ce code configurera les types dans notre modèle pour avoir une clé composite constituée de la colonne de clé de type int et la colonne de nom de chaîne. Si nous permet d’afficher le modèle dans le concepteur, elle ressemblerait à ceci :

![compositeKey](~/ef6/media/compositekey.png)

Un autre exemple de conventions de propriété consiste à configurer toutes les propriétés de date/heure dans mon modèle pour mapper au type datetime2 dans SQL Server au lieu de date/heure. Vous pouvez y parvenir avec les éléments suivants :

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Classes de convention

Une autre façon de définir les conventions consiste à utiliser une Convention de classe pour encapsuler votre convention. Lorsque vous utilisez une classe de Convention puis vous créez un type qui hérite de la classe Convention dans l’espace de noms System.Data.Entity.ModelConfiguration.Conventions.

Nous pouvons créer une classe de Convention avec la convention datetime2 que nous vous avons montré précédemment en procédant comme suit :

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Pour demander à EF d’utiliser cette convention ajoutez-le à la collection de Conventions dans OnModelCreating, qui si vous avez suivi la procédure pas à pas se présentera comme suit :

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Comme vous pouvez le voir nous ajoutons une instance de notre convention à la collection de conventions. Héritage à partir de la Convention d’offre un moyen pratique de regroupement et de partager des conventions entre équipes ou quelques projets. Vous pouvez, par exemple, avoir une bibliothèque de classes avec un ensemble commun de conventions que toutes vos organisations projets utilisent.

 

## <a name="custom-attributes"></a>Attributs personnalisés

Une autre utilisation des conventions consiste à activer les nouveaux attributs à utiliser lors de la configuration d’un modèle. Pour illustrer cela, nous allons créer un attribut que nous pouvons utiliser pour marquer les propriétés de chaîne en tant que non-Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Maintenant, nous allons créer une convention pour appliquer cet attribut à notre modèle :

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Avec cette convention, nous pouvons ajouter l’attribut de type non-Unicode à un de nos propriétés de chaîne, ce qui signifie que la colonne dans la base de données est stockée en tant que varchar au lieu de nvarchar.

Une chose à noter concernant cette convention est que si vous placez l’attribut de type non-Unicode sur autre chose qu’une propriété de chaîne, puis elle lève une exception. Pour cela, car vous ne pouvez pas configurer IsUnicode sur n’importe quel type autre qu’une chaîne. Si cela se produit, alors vous pouvez rendre votre convention plus spécifiques, afin qu’il exclut tout ce qui n’est pas une chaîne.

Bien que la convention ci-dessus fonctionne pour la définition des attributs personnalisés, il existe une autre API peut être beaucoup plus facile à utiliser, en particulier lorsque vous souhaitez utiliser les propriétés de la classe d’attributs.

Pour cet exemple, nous allons mettre à jour notre attribut et remplacez-le par un attribut IsUnicode, afin qu’il ressemble à ceci :

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Une fois que nous avons ceci, nous pouvons définir une valeur booléenne sur notre attribut pour indiquer à la convention ou non une propriété doit être Unicode. Nous pourrions effectuer cette opération dans la convention que nous avons déjà en accédant à la ClrProperty de la classe de configuration comme suit :

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Cela est assez simple, mais il existe un moyen plus succinct de parvenir à l’aide de la méthode de l’API de conventions. L’avoir de méthode a un paramètre de type Func&lt;PropertyInfo, T&gt; qui accepte le PropertyInfo identique à l’emplacement où la méthode, mais est censée renvoyer un objet. Si l’objet retourné est null, alors la propriété ne sera pas configurée, ce qui signifie que vous pouvez filtrer les propriétés avec lui comme Where, mais il est différent, il sera également capturer l’objet retourné et passez-le à la méthode Configure. Cela fonctionne comme suit :

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Attributs personnalisés ne sont pas la seule raison d’utiliser la méthode, il est utile en tout lieu dont vous avez besoin de raisonner sur quelque chose que vous filtrez sur lors de la configuration de vos types ou les propriétés.

 

## <a name="configuring-types"></a>Configuration des Types

Jusqu'à présent, toutes nos conventions ont été pour les propriétés, mais il existe une autre zone de l’API de conventions pour la configuration des types dans votre modèle. L’expérience est semblable aux conventions que nous avons vus jusqu'à présent, mais les options de configuration à l’intérieur sera à l’entité au lieu de la propriété niveau.

Une des choses que les conventions de niveau de Type peuvent être très utiles pour change la convention de nommage table pour mapper à un schéma existant qui diffère de la valeur par défaut d’EF ou pour créer une nouvelle base de données avec une autre convention d’affectation de noms. Pour ce faire, nous devons tout d’abord une méthode qui peut accepter la TypeInfo pour un type dans notre modèle et retourner ce que le nom de table pour ce type doit être :

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Cette méthode prend un type et retourne une chaîne qui utilise des minuscules avec des traits de soulignement au lieu de la casse mixte. Dans notre modèle, cela signifie que la classe de ProductCategory sera être mappée à une table appelée produit\_catégorie au lieu de ProductCategories.

Une fois que nous avons cette méthode, nous pouvons l’appeler dans une convention comme suit :

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Cette convention configure chaque type dans notre modèle pour mapper au nom de table qui est retourné à partir de notre méthode GetTableName. Cette convention est l’équivalent à l’appel de la méthode ToTable pour chaque entité dans le modèle à l’aide de l’API Fluent.

Une chose à noter à ce sujet est que lorsque vous appelez EF ToTable prendra la chaîne que vous fournissez le nom de table exact, sans les pluralisation qui serait normalement le cas lors de la détermination des noms de table. C’est pourquoi le nom de table à partir de notre convention est produit\_catégorie au lieu de produit\_catégories. Nous pouvons résoudre que dans notre convention en effectuant un appel au service de pluralisation nous-mêmes.

Dans le code suivant, nous utiliserons le [résolution des dépendances](~/ef6/fundamentals/configuring/dependency-resolution.md) fonctionnalité ajoutée dans EF6 pour récupérer le service de pluralisation EF aurait utilisé et pluraliser notre nom de la table.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> La version générique de GetService est une méthode d’extension dans l’espace de noms System.Data.Entity.Infrastructure.DependencyResolution, vous devez ajouter une à l’aide de l’instruction à votre contexte pour pouvoir l’utiliser.

### <a name="totable-and-inheritance"></a>ToTable et héritage

Un autre aspect important de ToTable est que si vous mappez explicitement un type à une table donnée, vous pouvez modifier la stratégie de mappage qui seront utilisés par EF. Si vous appelez ToTable pour chaque type dans une hiérarchie d’héritage, en passant le nom de type en tant que le nom de la table, comme nous l’avons fait ci-dessus, puis vous allez modifier la stratégie de mappage de Table par hiérarchie (TPH) par défaut pour la Table par Type (TPT). La meilleure façon de décrire Ceci est un exemple concret de whith :

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Par défaut employé et manager sont mappés à la même table (employés) dans la base de données. La table contiendra les employés et aux responsables d’une colonne de discriminateur qui vous indiquera à quel type d’instance est stocké dans chaque ligne. Il s’agit de mappage TPH qu’il existe une seule table pour la hiérarchie. Toutefois, si vous appelez ToTable sur les deux classe puis chaque type sera plutôt être mappée à sa propre table, également appelé TPT, car chaque type possède sa propre table.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Le code ci-dessus mappera vers une structure de table qui ressemble à ceci :

![tptExample](~/ef6/media/tptexample.jpg)

Vous pouvez éviter ce problème et mettre à jour le mappage TPH par défaut, de différentes manières :

1.  Appelez ToTable portant le même nom de table pour chaque type dans la hiérarchie.
2.  Appelez ToTable uniquement sur la classe de base de la hiérarchie, dans notre exemple serait employé.

 

## <a name="execution-order"></a>Ordre d’exécution

Conventions de fonctionnent de manière dernière wins, le même que l’API Fluent. Cela signifie que si vous écrivez deux conventions qui configurent la même option de la même propriété, puis il pour exécuter wins. Par exemple, dans le code ci-dessous, la longueur maximale de toutes les chaînes est définie sur 500, mais nous ensuite configurer toutes les propriétés appelées nom dans le modèle pour avoir une longueur maximale de 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Étant donné que la convention pour définir la longueur maximale de 250 est après celle qui définit toutes les chaînes à 500, toutes les propriétés appelées nom dans notre modèle aura un MaxLength de 250 lors de toutes les autres chaînes, telles que des descriptions, serait de 500. À l’aide des conventions de cette façon signifie que vous pouvez fournir une convention générale pour les types ou les propriétés dans votre modèle, puis overide pour les sous-ensembles sont différents.

L’API Fluent et les Annotations de données peuvent également être utilisées pour remplacer une convention dans des cas spécifiques. Dans l’exemple ci-dessus si nous avions utilisé l’API Fluent pour définir la longueur maximale d’une propriété puis nous pouvons également avoir ajouter il avant ou après la convention, étant donné que l’API Fluent plus spécifiques emporte sur la Convention de Configuration plus générales.

 

## <a name="built-in-conventions"></a>Conventions intégrées

Étant donné que les conventions personnalisées pourrait être affectées par les conventions Code First par défaut, il peut être utile d’ajouter des conventions à exécuter avant ou après une autre convention. Pour ce faire, vous pouvez utiliser les méthodes AddBefore et AddAfter de la collection de Conventions sur votre DbContext dérivée. Le code suivant ajouteriez la classe convention que nous avons créé précédemment afin qu’elle sera exécutée avant la génération de la convention de découverte des clés.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Cela va être les plus utiles lors de l’ajout des conventions qui doivent s’exécuter avant ou après les conventions intégrées, une liste des conventions intégrées est disponible ici : [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Vous pouvez également supprimer les conventions que vous ne souhaitez pas appliquées à votre modèle. Pour supprimer une convention, utilisez la méthode Remove. Voici un exemple de la suppression de la PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
