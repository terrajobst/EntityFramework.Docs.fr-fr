---
title: Validation - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
caps.latest.revision: 3
ms.openlocfilehash: 758865255d7868337dc1d7801bd9ff77f0bb57a9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121478"
---
# <a name="data-validation"></a><span data-ttu-id="7a6fa-102">Validation de données</span><span class="sxs-lookup"><span data-stu-id="7a6fa-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="7a6fa-103">**EF4.1 et versions ultérieures uniquement** -les fonctionnalités, API, etc. abordés dans cette page ont été introduits dans Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="7a6fa-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’applique pas</span><span class="sxs-lookup"><span data-stu-id="7a6fa-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="7a6fa-105">Le contenu de cette page est une adaptation d’et que l’article ont été écrit par Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="7a6fa-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="7a6fa-106">Entity Framework fournit une grande variété de fonctionnalités de validation qui peuvent alimenter via à une interface utilisateur pour la validation côté client ou être utilisés pour la validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="7a6fa-107">Lorsque vous utilisez le code tout d’abord, vous pouvez spécifier des validations à l’aide des annotations ou des configurations d’API fluent.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="7a6fa-108">Validations supplémentaires et plus complexe, peuvent être spécifiés dans le code et fonctionneront si votre modèle est le fruit de code tout d’abord, modèle tout d’abord ou tout d’abord la base de données.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="7a6fa-109">Le modèle</span><span class="sxs-lookup"><span data-stu-id="7a6fa-109">The model</span></span>

<span data-ttu-id="7a6fa-110">Je vais vous montrer les validations avec une simple paire de classes : Blog et Post.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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
## <a name="data-annotations"></a><span data-ttu-id="7a6fa-111">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="7a6fa-111">Data Annotations</span></span>

<span data-ttu-id="7a6fa-112">Code utilise d’abord les annotations à partir de l’assembly System.ComponentModel.DataAnnotations comme un moyen de classes du premier code de configuration.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="7a6fa-113">Parmi ces annotations sont ceux qui fournissent les règles telles que le requis, MaxLength et MinLength.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="7a6fa-114">Un nombre d’applications clientes .NET reconnaisse également ces annotations, par exemple, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="7a6fa-115">Vous pouvez obtenir les deux validation client et côté serveur côté avec ces annotations.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="7a6fa-116">Par exemple, vous pouvez forcer la propriété de titre du Blog pour être une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="7a6fa-117">Sans code supplémentaire ni modifications de balisage dans l’application, une application MVC effectue la validation côté client, création même dynamique d’un message en utilisant les noms de propriété et d’annotation.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![figure01](~/ef6/media/figure01.png)

<span data-ttu-id="7a6fa-119">Dans le billet back (méthode) de cette vue de créer, Entity Framework est utilisé pour enregistrer le nouveau blog sur la base de données, mais la validation côté client de MVC est déclenchée avant que l’application n’atteigne ce code.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="7a6fa-120">Validation côté client n’est pas toutefois à toute épreuve.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="7a6fa-121">Les utilisateurs peuvent avoir un impact sur les fonctionnalités de leur navigateur ou pire encore, un pirate peut utiliser une astuce pour éviter les validations de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="7a6fa-122">Mais Entity Framework également reconnaît l’annotation requise et validez-le.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="7a6fa-123">Un moyen simple pour effectuer ce test consiste à désactiver la fonctionnalité de validation côté client de MVC.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="7a6fa-124">Pour cela, dans le fichier web.config de l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="7a6fa-125">La section appSettings possède une clé pour ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="7a6fa-126">Définition de cette clé sur false empêche l’interface utilisateur d’effectuer des validations.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="7a6fa-127">Même avec la validation côté client est désactivée, vous obtiendrez la même réponse dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="7a6fa-128">Le message d’erreur « le champ de titre est obligatoire » s’affichera en tant que.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="7a6fa-129">Excepté qu’à présent, il sera un résultat de validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="7a6fa-130">Entity Framework effectue la validation sur l’annotation requise (avant que vous souhaitez les supprimer même génération et de la commande d’insertion pour envoyer à la base de données) et renvoyer l’erreur MVC qui affiche le message.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7a6fa-131">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7a6fa-131">Fluent API</span></span>

<span data-ttu-id="7a6fa-132">Vous pouvez utiliser le code des API fluent du premier au lieu d’annotations pour obtenir le même client côté & serveur côté validation.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="7a6fa-133">Au lieu de l’utilisation obligatoires, je vais vous montrer à l’aide d’une validation MaxLength.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="7a6fa-134">Configurations d’API Fluent sont appliquées comme code crée tout d’abord le modèle à partir des classes.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="7a6fa-135">Vous pouvez injecter les configurations en substituant la méthode OnModelCreating de la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="7a6fa-136">Voici une configuration qui spécifie que la propriété BloggerName peut être pas plue de 10 caractères.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="7a6fa-137">Erreurs de validation levées selon les configurations de l’API Fluent ne serez pas reach de l’interface utilisateur, mais vous peut elle les capture automatiquement dans le code et puis de réagir à celle-ci en conséquence.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="7a6fa-138">Certaines exceptions Voici code d’erreur dans la classe d’application BlogController qui capture cette erreur de validation lors de l’Entity Framework tente d’enregistrer un blog avec un BloggerName qui dépasse le maximum de 10 caractères.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="7a6fa-139">La validation n’automatiquement transmise dans la vue qui est la raison pour laquelle le code supplémentaire qui utilise ModelState.AddModelError est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="7a6fa-140">Cela garantit que les détails de l’erreur rendent à la vue qui utilise le ValidationMessageFor Htmlhelper pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="7a6fa-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="7a6fa-141">IValidatableObject</span></span>

<span data-ttu-id="7a6fa-142">IValidatableObject est une interface qui réside dans System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="7a6fa-143">S’il n’est pas partie de l’API Entity Framework, vous pouvez toujours tirer parti pour la validation côté serveur dans vos classes Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="7a6fa-144">IValidatableObject fournit une méthode de validation Entity Framework appellera pendant SaveChanges, ou vous pouvez appeler vous-même n’importe quel moment que vous souhaitez valider les classes.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="7a6fa-145">Configurations requises comme et MaxLength effectuent de validation sur un seul champ.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="7a6fa-146">Dans la méthode Validate, vous pouvez avoir une logique encore plus complexe, par exemple, comparaison de deux champs.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="7a6fa-147">Dans l’exemple suivant, la classe Blog a été étendue pour implémenter IValidatableObject et fournir ensuite une règle qui le titre et le BloggerName ne peut pas correspondre.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="7a6fa-148">Le constructeur ValidationResult prend une chaîne qui représente le message d’erreur et un tableau de chaînes qui représentent les noms des membres qui sont associés à la validation.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="7a6fa-149">Étant donné que cette validation vérifie le titre et le BloggerName, les deux noms de propriété sont retournées.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="7a6fa-150">Contrairement à la validation fournie par l’API Fluent, ce résultat de validation est reconnu par la vue et le Gestionnaire d’exceptions que j’ai utilisée précédemment pour ajouter l’erreur dans ModelState n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="7a6fa-151">Étant donné que j’ai configuré les deux noms de propriété dans le ValidationResult, le HtmlHelpers MVC affiche le message d’erreur pour ces deux propriétés.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="7a6fa-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="7a6fa-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="7a6fa-154">DbContext a une méthode substituable appelée ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="7a6fa-155">Lorsque vous appelez SaveChanges, Entity Framework appelle cette méthode pour chaque entité dans son cache dont l’état n’est pas sans modification.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="7a6fa-156">Vous pouvez placer la logique de validation directement dans ici ou même utiliser cette méthode pour appeler, par exemple, la méthode Blog.Validate ajoutée dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="7a6fa-157">Voici un exemple d’un remplacement ValidateEntity qui valide des nouveaux billets pour vous assurer que le titre de la publication n’a pas déjà été utilisé.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="7a6fa-158">Il vérifie tout d’abord si l’entité est un billet et que son état est ajouté.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="7a6fa-159">Si tel est le cas, il recherche ensuite dans la base de données pour voir s’il existe déjà une publication avec le même titre.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="7a6fa-160">S’il existe déjà un billet existant, un nouveau DbEntityValidationResult est créé.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="7a6fa-161">DbEntityValidationResult héberge un DbEntityEntry et une ICollection de DbValidationErrors pour une seule entité.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="7a6fa-162">Au début de cette méthode, un DbEntityValidationResult est instancié et puis toutes les erreurs qui sont détectées sont ajoutées dans sa collection ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="7a6fa-163">Déclencher explicitement la validation</span><span class="sxs-lookup"><span data-stu-id="7a6fa-163">Explicitly triggering validation</span></span>

<span data-ttu-id="7a6fa-164">Un appel à SaveChanges déclenche toutes les validations abordées dans cet article.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="7a6fa-165">Mais vous n’avez pas besoin de s’appuyer sur SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="7a6fa-166">Vous pouvez valider ailleurs dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="7a6fa-167">DbContext.GetValidationErrors déclenche toutes les validations, ceux définis par les annotations ou l’API Fluent, la validation créé dans IValidatableObject (par exemple, Blog.Validate) et les validations effectuées dans le DbContext.ValidateEntity méthode.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="7a6fa-168">Le code suivant appelle GetValidationErrors sur l’instance actuelle d’un DbContext.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="7a6fa-169">ValidationErrors sont regroupés par type d’entité dans DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="7a6fa-170">Le code effectue une itération tout d’abord, via le DbValidationResults retourné par la méthode et puis chaque ValidationError à l’intérieur.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="7a6fa-171">Autres considérations sur l’utilisation de validation</span><span class="sxs-lookup"><span data-stu-id="7a6fa-171">Other considerations when using validation</span></span>

<span data-ttu-id="7a6fa-172">Voici quelques autres points à prendre en compte lors de l’utilisation de la validation de l’Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="7a6fa-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="7a6fa-173">Le chargement différé est désactivé lors de la validation.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="7a6fa-174">EF validera les annotations de données sur les propriétés non mappées (les propriétés qui ne sont pas mappées à une colonne dans la base de données).</span><span class="sxs-lookup"><span data-stu-id="7a6fa-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="7a6fa-175">La validation est effectuée une fois que les modifications sont détectées au cours de SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="7a6fa-176">Si vous apportez des modifications pendant la validation, qu'il s’agit de votre responsable de notifier le traceur de modifications.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="7a6fa-177">DbUnexpectedValidationException est levée si les erreurs se produisent pendant la validation.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="7a6fa-178">Facettes Entity Framework inclut dans le modèle (longueur maximale, requis, etc.) entraîne la validation, même s’il n’existe pas les annotations de données sur vos classes et/ou l’Entity Framework Designer vous permet de créer votre modèle.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="7a6fa-179">Règles de priorité :</span><span class="sxs-lookup"><span data-stu-id="7a6fa-179">Precedence rules:</span></span>
    -   <span data-ttu-id="7a6fa-180">Appels d’API Fluent remplacent les annotations de données correspondant</span><span class="sxs-lookup"><span data-stu-id="7a6fa-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="7a6fa-181">Ordre d’exécution :</span><span class="sxs-lookup"><span data-stu-id="7a6fa-181">Execution order:</span></span>
    -   <span data-ttu-id="7a6fa-182">Validation de propriété se produit avant la validation de type</span><span class="sxs-lookup"><span data-stu-id="7a6fa-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="7a6fa-183">Validation de type se produit uniquement si la validation de propriété réussit</span><span class="sxs-lookup"><span data-stu-id="7a6fa-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="7a6fa-184">Si une propriété est complexe inclut également sa validation :</span><span class="sxs-lookup"><span data-stu-id="7a6fa-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="7a6fa-185">Validation des propriétés sur les propriétés de type complexe</span><span class="sxs-lookup"><span data-stu-id="7a6fa-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="7a6fa-186">Type de validation au niveau du type complexe, y compris la validation IValidatableObject sur le type complexe</span><span class="sxs-lookup"><span data-stu-id="7a6fa-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="7a6fa-187">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7a6fa-187">Summary</span></span>

<span data-ttu-id="7a6fa-188">L’API de validation dans Entity Framework joue très bien avec la validation côté client dans MVC, mais vous n’êtes pas obligé de s’appuient sur la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="7a6fa-189">Entity Framework se chargera de la validation côté serveur pour DataAnnotations ou des configurations que vous avez appliqué avec l’API Fluent de code first.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="7a6fa-190">Vous avez également vu un nombre de points d’extensibilité pour personnaliser le comportement si vous utilisez l’interface IValidatableObject ou que vous appuyez sur la méthode DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="7a6fa-191">Et ces deux derniers moyens de validation sont disponibles via la classe DbContext, que vous utilisiez le flux de travail de Code First, Model First ou Database First pour décrire votre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="7a6fa-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
