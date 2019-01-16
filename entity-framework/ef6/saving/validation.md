---
title: Validation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 98d7bd08d841ee400afb62e1079f1a965f65e139
ms.sourcegitcommit: b4a5ed177b86bf7f81602106dab6b4acc18dfc18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54316645"
---
# <a name="data-validation"></a>Validation de données
> [!NOTE]
> **EF4.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 4.1. Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’applique pas

Le contenu de cette page est adapté à partir d’un article écrit à l’origine par Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework fournit une grande variété de fonctionnalités de validation qui peuvent alimenter via à une interface utilisateur pour la validation côté client ou être utilisés pour la validation côté serveur. Lorsque vous utilisez le code tout d’abord, vous pouvez spécifier des validations à l’aide des annotations ou des configurations d’API fluent. Validations supplémentaires et plus complexe, peuvent être spécifiés dans le code et fonctionneront si votre modèle est le fruit de code tout d’abord, modèle tout d’abord ou tout d’abord la base de données.

## <a name="the-model"></a>Le modèle

Je vais vous montrer les validations avec une simple paire de classes : Blog et Post.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```

## <a name="data-annotations"></a>Annotations de données

Code utilise d’abord les annotations à partir de l’assembly System.ComponentModel.DataAnnotations comme un moyen de classes du premier code de configuration. Parmi ces annotations sont ceux qui fournissent les règles telles que le requis, MaxLength et MinLength. Un nombre d’applications clientes .NET reconnaisse également ces annotations, par exemple, ASP.NET MVC. Vous pouvez obtenir les deux validation client et côté serveur côté avec ces annotations. Par exemple, vous pouvez forcer la propriété de titre du Blog pour être une propriété obligatoire.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sans code supplémentaire ni modifications de balisage dans l’application, une application MVC effectue la validation côté client, création même dynamique d’un message en utilisant les noms de propriété et d’annotation.

![Figure 1](~/ef6/media/figure01.png)

Dans le billet back (méthode) de cette vue de créer, Entity Framework est utilisé pour enregistrer le nouveau blog sur la base de données, mais la validation côté client de MVC est déclenchée avant que l’application n’atteigne ce code.

Validation côté client n’est pas toutefois à toute épreuve. Les utilisateurs peuvent avoir un impact sur les fonctionnalités de leur navigateur ou pire encore, un pirate peut utiliser une astuce pour éviter les validations de l’interface utilisateur. Mais Entity Framework également reconnaît l’annotation requise et validez-le.

Un moyen simple pour effectuer ce test consiste à désactiver la fonctionnalité de validation côté client de MVC. Pour cela, dans le fichier web.config de l’application MVC. La section appSettings possède une clé pour ClientValidationEnabled. Définition de cette clé sur false empêche l’interface utilisateur d’effectuer des validations.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Même avec la validation côté client est désactivée, vous obtiendrez la même réponse dans votre application. Le message d’erreur « le champ de titre est obligatoire » s’affichera en tant que. Excepté qu’à présent, il sera un résultat de validation côté serveur. Entity Framework effectue la validation sur l’annotation requise (avant que vous souhaitez les supprimer même génération et de la commande d’insertion pour envoyer à la base de données) et renvoyer l’erreur MVC qui affiche le message.

## <a name="fluent-api"></a>API Fluent

Vous pouvez utiliser le code des API fluent du premier au lieu d’annotations pour obtenir le même client côté & serveur côté validation. Au lieu de l’utilisation obligatoires, je vais vous montrer à l’aide d’une validation MaxLength.

Configurations d’API Fluent sont appliquées comme code crée tout d’abord le modèle à partir des classes. Vous pouvez injecter les configurations en substituant la méthode OnModelCreating de la classe DbContext. Voici une configuration qui spécifie que la propriété BloggerName peut être pas plue de 10 caractères.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Erreurs de validation levées selon les configurations de l’API Fluent ne serez pas reach de l’interface utilisateur, mais vous peut elle les capture automatiquement dans le code et puis de réagir à celle-ci en conséquence.

Certaines exceptions Voici code d’erreur dans la classe d’application BlogController qui capture cette erreur de validation lors de l’Entity Framework tente d’enregistrer un blog avec un BloggerName qui dépasse le maximum de 10 caractères.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

La validation n’automatiquement transmise dans la vue qui est la raison pour laquelle le code supplémentaire qui utilise ModelState.AddModelError est utilisé. Cela garantit que les détails de l’erreur rendent à la vue qui utilise le ValidationMessageFor Htmlhelper pour afficher l’erreur.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject est une interface qui réside dans System.ComponentModel.DataAnnotations. S’il n’est pas partie de l’API Entity Framework, vous pouvez toujours tirer parti pour la validation côté serveur dans vos classes Entity Framework. IValidatableObject fournit une méthode de validation Entity Framework appellera pendant SaveChanges, ou vous pouvez appeler vous-même n’importe quel moment que vous souhaitez valider les classes.

Configurations requises comme et MaxLength effectuent de validation sur un seul champ. Dans la méthode Validate, vous pouvez avoir une logique encore plus complexe, par exemple, comparaison de deux champs.

Dans l’exemple suivant, la classe Blog a été étendue pour implémenter IValidatableObject et fournir ensuite une règle qui le titre et le BloggerName ne peut pas correspondre.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

Le constructeur ValidationResult prend une chaîne qui représente le message d’erreur et un tableau de chaînes qui représentent les noms des membres qui sont associés à la validation. Étant donné que cette validation vérifie le titre et le BloggerName, les deux noms de propriété sont retournées.

Contrairement à la validation fournie par l’API Fluent, ce résultat de validation est reconnu par la vue et le Gestionnaire d’exceptions que j’ai utilisée précédemment pour ajouter l’erreur dans ModelState n’est pas nécessaire. Étant donné que j’ai configuré les deux noms de propriété dans le ValidationResult, le HtmlHelpers MVC affiche le message d’erreur pour ces deux propriétés.

![Figure 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext a une méthode substituable appelée ValidateEntity. Lorsque vous appelez SaveChanges, Entity Framework appelle cette méthode pour chaque entité dans son cache dont l’état n’est pas sans modification. Vous pouvez placer la logique de validation directement dans ici ou même utiliser cette méthode pour appeler, par exemple, la méthode Blog.Validate ajoutée dans la section précédente.

Voici un exemple d’un remplacement ValidateEntity qui valide des nouveaux billets pour vous assurer que le titre de la publication n’a pas déjà été utilisé. Il vérifie tout d’abord si l’entité est un billet et que son état est ajouté. Si tel est le cas, il recherche ensuite dans la base de données pour voir s’il existe déjà une publication avec le même titre. S’il existe déjà un billet existant, un nouveau DbEntityValidationResult est créé.

DbEntityValidationResult héberge un DbEntityEntry et une ICollection de DbValidationErrors pour une seule entité. Au début de cette méthode, un DbEntityValidationResult est instancié et puis toutes les erreurs qui sont détectées sont ajoutées dans sa collection ValidationErrors.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Déclencher explicitement la validation

Un appel à SaveChanges déclenche toutes les validations abordées dans cet article. Mais vous n’avez pas besoin de s’appuyer sur SaveChanges. Vous pouvez valider ailleurs dans votre application.

DbContext.GetValidationErrors déclenche toutes les validations, ceux définis par les annotations ou l’API Fluent, la validation créé dans IValidatableObject (par exemple, Blog.Validate) et les validations effectuées dans le DbContext.ValidateEntity méthode.

Le code suivant appelle GetValidationErrors sur l’instance actuelle d’un DbContext. ValidationErrors sont regroupés par type d’entité dans DbValidationResults. Le code effectue une itération tout d’abord, via le DbValidationResults retourné par la méthode et puis chaque ValidationError à l’intérieur.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Autres considérations sur l’utilisation de validation

Voici quelques autres points à prendre en compte lors de l’utilisation de la validation de l’Entity Framework :

-   Le chargement différé est désactivé lors de la validation.
-   EF validera les annotations de données sur les propriétés non mappées (les propriétés qui ne sont pas mappées à une colonne dans la base de données).
-   La validation est effectuée une fois que les modifications sont détectées au cours de SaveChanges. Si vous apportez des modifications pendant la validation, qu'il s’agit de votre responsable de notifier le traceur de modifications.
-   DbUnexpectedValidationException est levée si les erreurs se produisent pendant la validation.
-   Facettes Entity Framework inclut dans le modèle (longueur maximale, requis, etc.) entraîne la validation, même s’il n’existe pas les annotations de données sur vos classes et/ou l’Entity Framework Designer vous permet de créer votre modèle.
-   Règles de priorité :
    -   Appels d’API Fluent remplacent les annotations de données correspondant
-   Ordre d’exécution :
    -   Validation de propriété se produit avant la validation de type
    -   Validation de type se produit uniquement si la validation de propriété réussit
-   Si une propriété est complexe inclut également sa validation :
    -   Validation des propriétés sur les propriétés de type complexe
    -   Type de validation au niveau du type complexe, y compris la validation IValidatableObject sur le type complexe

## <a name="summary"></a>Récapitulatif

L’API de validation dans Entity Framework joue très bien avec la validation côté client dans MVC, mais vous n’êtes pas obligé de s’appuient sur la validation côté client. Entity Framework se chargera de la validation côté serveur pour DataAnnotations ou des configurations que vous avez appliqué avec l’API Fluent de code first.

Vous avez également vu un nombre de points d’extensibilité pour personnaliser le comportement si vous utilisez l’interface IValidatableObject ou que vous appuyez sur la méthode DbContet.ValidateEntity. Et ces deux derniers moyens de validation sont disponibles via la classe DbContext, que vous utilisiez le flux de travail de Code First, Model First ou Database First pour décrire votre modèle conceptuel.
