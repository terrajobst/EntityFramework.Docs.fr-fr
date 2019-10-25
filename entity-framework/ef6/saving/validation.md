---
title: Validation-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812152"
---
# <a name="data-validation"></a><span data-ttu-id="7a596-102">Validation de données</span><span class="sxs-lookup"><span data-stu-id="7a596-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="7a596-103">**EF 4.1 uniquement** : les fonctionnalités, les API, etc. présentées dans cette page ont été introduites dans Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="7a596-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="7a596-104">Si vous utilisez une version antérieure, certaines ou toutes les informations ne s’appliquent pas</span><span class="sxs-lookup"><span data-stu-id="7a596-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="7a596-105">Le contenu de cette page est adapté à partir d’un article écrit à l’origine par Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="7a596-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="7a596-106">Entity Framework fournit une grande variété de fonctionnalités de validation qui peuvent être alimentées par une interface utilisateur pour la validation côté client ou être utilisées pour la validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7a596-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="7a596-107">Lorsque vous utilisez code First, vous pouvez spécifier des validations à l’aide d’une annotation ou de configurations d’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7a596-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="7a596-108">Des validations supplémentaires, et plus complexes, peuvent être spécifiées dans le code et fonctionnent si votre modèle se passe d’abord par code First, Model First ou Database.</span><span class="sxs-lookup"><span data-stu-id="7a596-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="7a596-109">Le modèle</span><span class="sxs-lookup"><span data-stu-id="7a596-109">The model</span></span>

<span data-ttu-id="7a596-110">Je vais démontrer les validations à l’aide d’une simple paire de classes : blog et billet.</span><span class="sxs-lookup"><span data-stu-id="7a596-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

## <a name="data-annotations"></a><span data-ttu-id="7a596-111">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="7a596-111">Data Annotations</span></span>

<span data-ttu-id="7a596-112">Code First utilise des annotations de l’assembly `System.ComponentModel.DataAnnotations` comme un moyen de configurer les classes code First.</span><span class="sxs-lookup"><span data-stu-id="7a596-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="7a596-113">Parmi ces annotations, citons celles qui fournissent des règles telles que les `Required`, les `MaxLength` et les `MinLength`.</span><span class="sxs-lookup"><span data-stu-id="7a596-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="7a596-114">Plusieurs applications clientes .NET reconnaissent également ces annotations, par exemple, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7a596-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="7a596-115">Vous pouvez réaliser la validation côté client et côté serveur avec ces annotations.</span><span class="sxs-lookup"><span data-stu-id="7a596-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="7a596-116">Par exemple, vous pouvez forcer la propriété titre du blog à être une propriété obligatoire.</span><span class="sxs-lookup"><span data-stu-id="7a596-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="7a596-117">Sans aucune modification de code ou de balisage supplémentaire dans l’application, une application MVC existante effectue la validation côté client, même en générant dynamiquement un message à l’aide des noms de propriété et d’annotation.</span><span class="sxs-lookup"><span data-stu-id="7a596-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Figure 1](~/ef6/media/figure01.png)

<span data-ttu-id="7a596-119">Dans la méthode de publication de cette vue Create View, Entity Framework est utilisé pour enregistrer le nouveau blog dans la base de données, mais la validation côté client de MVC est déclenchée avant que l’application n’atteigne ce code.</span><span class="sxs-lookup"><span data-stu-id="7a596-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="7a596-120">Toutefois, la validation côté client n’a pas de preuve de puce.</span><span class="sxs-lookup"><span data-stu-id="7a596-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="7a596-121">Les utilisateurs peuvent avoir un impact sur les fonctionnalités de leur navigateur ou pire encore, un pirate peut utiliser des astuces pour éviter les validations de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7a596-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="7a596-122">Mais Entity Framework reconnaîtront également l’annotation `Required` et la valider.</span><span class="sxs-lookup"><span data-stu-id="7a596-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="7a596-123">Un moyen simple de tester cela consiste à désactiver la fonctionnalité de validation côté client de MVC.</span><span class="sxs-lookup"><span data-stu-id="7a596-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="7a596-124">Vous pouvez le faire dans le fichier Web. config de l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="7a596-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="7a596-125">La section appSettings a une clé pour ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="7a596-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="7a596-126">L’affectation de la valeur false à cette clé empêchera l’interface utilisateur d’effectuer des validations.</span><span class="sxs-lookup"><span data-stu-id="7a596-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="7a596-127">Même si la validation côté client est désactivée, vous obtiendrez la même réponse dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7a596-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="7a596-128">Le message d’erreur « le champ titre est obligatoire » s’affiche comme avant.</span><span class="sxs-lookup"><span data-stu-id="7a596-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="7a596-129">À ce stade, il s’agit d’un résultat de la validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7a596-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="7a596-130">Entity Framework effectue la validation sur l’annotation de `Required` (avant même que les deux à la fois pour générer une commande `INSERT` à envoyer à la base de données) et retourner l’erreur à MVC qui affichera le message.</span><span class="sxs-lookup"><span data-stu-id="7a596-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7a596-131">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7a596-131">Fluent API</span></span>

<span data-ttu-id="7a596-132">Vous pouvez utiliser l’API Fluent code First au lieu des annotations pour accéder au même côté client & validation côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7a596-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="7a596-133">Au lieu d’utiliser `Required`, je vous montrerai cela à l’aide d’une validation MaxLength.</span><span class="sxs-lookup"><span data-stu-id="7a596-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="7a596-134">Les configurations de l’API Fluent sont appliquées comme code First pour générer le modèle à partir des classes.</span><span class="sxs-lookup"><span data-stu-id="7a596-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="7a596-135">Vous pouvez injecter les configurations en remplaçant la méthode OnModelCreating de la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="7a596-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="7a596-136">Voici une configuration spécifiant que la propriété BloggerName ne peut pas comporter plus de 10 caractères.</span><span class="sxs-lookup"><span data-stu-id="7a596-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="7a596-137">Les erreurs de validation levées sur la base des configurations de l’API Fluent n’atteignent pas automatiquement l’interface utilisateur, mais vous pouvez la capturer dans le code et y répondre en conséquence.</span><span class="sxs-lookup"><span data-stu-id="7a596-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="7a596-138">Voici un code d’erreur de gestion des exceptions dans la classe BlogController de l’application qui capture cette erreur de validation quand Entity Framework tente d’enregistrer un blog avec un BloggerName qui dépasse la valeur maximale de 10 caractères.</span><span class="sxs-lookup"><span data-stu-id="7a596-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="7a596-139">La validation n’est pas renvoyée automatiquement dans la vue, c’est pourquoi le code supplémentaire qui utilise `ModelState.AddModelError` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="7a596-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="7a596-140">Cela permet de s’assurer que les détails de l’erreur sont affichés dans la vue qui utilisera ensuite le `ValidationMessageFor` HtmlHelper pour afficher l’erreur.</span><span class="sxs-lookup"><span data-stu-id="7a596-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="7a596-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="7a596-141">IValidatableObject</span></span>

<span data-ttu-id="7a596-142">`IValidatableObject` est une interface qui réside dans `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="7a596-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="7a596-143">Bien qu’il ne fasse pas partie de l’API Entity Framework, vous pouvez toujours l’utiliser pour la validation côté serveur dans vos classes Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7a596-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="7a596-144">`IValidatableObject` fournit une méthode `Validate` que Entity Framework appellera au cours de l’opération SaveChanges ou vous pouvez appeler vous-même chaque fois que vous souhaitez valider les classes.</span><span class="sxs-lookup"><span data-stu-id="7a596-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="7a596-145">Les configurations telles que `Required` et `MaxLength` effectuent la validation sur un champ unique.</span><span class="sxs-lookup"><span data-stu-id="7a596-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="7a596-146">Dans la méthode `Validate` vous pouvez avoir une logique encore plus complexe, par exemple, en comparant deux champs.</span><span class="sxs-lookup"><span data-stu-id="7a596-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="7a596-147">Dans l’exemple suivant, la classe `Blog` a été étendue pour implémenter `IValidatableObject`, puis fournir une règle selon laquelle les `Title` et `BloggerName` ne peuvent pas correspondre.</span><span class="sxs-lookup"><span data-stu-id="7a596-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="7a596-148">Le constructeur `ValidationResult` prend un `string` qui représente le message d’erreur et un tableau de `string`s qui représentent les noms de membres associés à la validation.</span><span class="sxs-lookup"><span data-stu-id="7a596-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="7a596-149">Étant donné que cette validation vérifie à la fois les `Title` et les `BloggerName`, les deux noms de propriété sont retournés.</span><span class="sxs-lookup"><span data-stu-id="7a596-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="7a596-150">Contrairement à la validation fournie par l’API Fluent, ce résultat de validation est reconnu par la vue et le gestionnaire d’exceptions que j’ai utilisé précédemment pour ajouter l’erreur dans `ModelState` n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7a596-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="7a596-151">Étant donné que je définis les deux noms de propriété dans le `ValidationResult`, les HtmlHelper MVC affichent le message d’erreur pour ces deux propriétés.</span><span class="sxs-lookup"><span data-stu-id="7a596-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Figure 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="7a596-153">DbContext. ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="7a596-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="7a596-154">`DbContext` a une méthode substituable appelée `ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="7a596-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="7a596-155">Lorsque vous appelez `SaveChanges`, Entity Framework appelle cette méthode pour chaque entité dans son cache dont l’État n’est pas `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="7a596-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="7a596-156">Vous pouvez placer la logique de validation directement ici ou même utiliser cette méthode pour appeler, par exemple, la méthode `Blog.Validate` ajoutée dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="7a596-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="7a596-157">Voici un exemple de `ValidateEntity` remplacement qui valide les nouveaux `Post`s pour s’assurer que le titre de publication n’a pas déjà été utilisé.</span><span class="sxs-lookup"><span data-stu-id="7a596-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="7a596-158">Il commence par vérifier si l’entité est une publication et que son état est ajouté.</span><span class="sxs-lookup"><span data-stu-id="7a596-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="7a596-159">Si c’est le cas, il recherche dans la base de données s’il existe déjà une publication avec le même titre.</span><span class="sxs-lookup"><span data-stu-id="7a596-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="7a596-160">S’il existe déjà une publication existante, une nouvelle `DbEntityValidationResult` est créée.</span><span class="sxs-lookup"><span data-stu-id="7a596-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="7a596-161">`DbEntityValidationResult` héberge un `DbEntityEntry` et un `ICollection<DbValidationErrors>` pour une entité unique.</span><span class="sxs-lookup"><span data-stu-id="7a596-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="7a596-162">Au début de cette méthode, un `DbEntityValidationResult` est instancié, puis toutes les erreurs découvertes sont ajoutées à sa collection `ValidationErrors`.</span><span class="sxs-lookup"><span data-stu-id="7a596-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="7a596-163">Déclenchement explicite de la validation</span><span class="sxs-lookup"><span data-stu-id="7a596-163">Explicitly triggering validation</span></span>

<span data-ttu-id="7a596-164">Un appel à `SaveChanges` déclenche toutes les validations traitées dans cet article.</span><span class="sxs-lookup"><span data-stu-id="7a596-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="7a596-165">Mais vous n’avez pas besoin de vous appuyer sur `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7a596-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="7a596-166">Vous préférerez peut-être valider ailleurs dans votre application.</span><span class="sxs-lookup"><span data-stu-id="7a596-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="7a596-167">`DbContext.GetValidationErrors` déclenchera toutes les validations, celles définies par les annotations ou l’API Fluent, la validation créée dans `IValidatableObject` (par exemple, `Blog.Validate`) et les validations effectuées dans la méthode `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="7a596-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="7a596-168">Le code suivant appellera `GetValidationErrors` sur l’instance actuelle d’une `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7a596-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="7a596-169">les `ValidationErrors` sont regroupés par type d’entité en `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="7a596-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="7a596-170">Le code itère d’abord au `DbEntityValidationResult`s retourné par la méthode, puis à travers chaque `DbValidationError` à l’intérieur de.</span><span class="sxs-lookup"><span data-stu-id="7a596-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="7a596-171">Autres considérations relatives à l’utilisation de la validation</span><span class="sxs-lookup"><span data-stu-id="7a596-171">Other considerations when using validation</span></span>

<span data-ttu-id="7a596-172">Voici quelques autres points à prendre en compte lors de l’utilisation de la validation Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="7a596-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="7a596-173">Le chargement différé est désactivé au cours de la validation</span><span class="sxs-lookup"><span data-stu-id="7a596-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="7a596-174">EF valide les annotations de données sur les propriétés non mappées (propriétés qui ne sont pas mappées à une colonne de la base de données)</span><span class="sxs-lookup"><span data-stu-id="7a596-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="7a596-175">La validation est effectuée après la détection des modifications au cours de `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7a596-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="7a596-176">Si vous apportez des modifications au cours de la validation, il vous incombe de notifier le dispositif de suivi des modifications</span><span class="sxs-lookup"><span data-stu-id="7a596-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="7a596-177">`DbUnexpectedValidationException` est levée si des erreurs se produisent pendant la validation</span><span class="sxs-lookup"><span data-stu-id="7a596-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="7a596-178">Les facettes qui Entity Framework incluses dans le modèle (longueur maximale, obligatoire, etc.) entraînent la validation, même s’il n’y a pas d’annotations de données dans vos classes et/ou si vous avez utilisé le concepteur EF pour créer votre modèle</span><span class="sxs-lookup"><span data-stu-id="7a596-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="7a596-179">Règles de précédence :</span><span class="sxs-lookup"><span data-stu-id="7a596-179">Precedence rules:</span></span>
  - <span data-ttu-id="7a596-180">Les appels de l’API Fluent remplacent les annotations de données correspondantes</span><span class="sxs-lookup"><span data-stu-id="7a596-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="7a596-181">Ordre d’exécution :</span><span class="sxs-lookup"><span data-stu-id="7a596-181">Execution order:</span></span>
  - <span data-ttu-id="7a596-182">La validation de la propriété se produit avant la validation du type</span><span class="sxs-lookup"><span data-stu-id="7a596-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="7a596-183">La validation de type se produit uniquement si la validation de la propriété a échoué</span><span class="sxs-lookup"><span data-stu-id="7a596-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="7a596-184">Si une propriété est complexe, sa validation inclut également les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="7a596-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="7a596-185">Validation au niveau de la propriété sur les propriétés de type complexe</span><span class="sxs-lookup"><span data-stu-id="7a596-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="7a596-186">Validation au niveau du type sur le type complexe, y compris `IValidatableObject` validation sur le type complexe</span><span class="sxs-lookup"><span data-stu-id="7a596-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="7a596-187">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7a596-187">Summary</span></span>

<span data-ttu-id="7a596-188">L’API de validation de Entity Framework s’exécute très bien avec la validation côté client dans MVC, mais vous n’avez pas à vous appuyer sur la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="7a596-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="7a596-189">Entity Framework s’occupe de la validation côté serveur pour DataAnnotations ou les configurations que vous avez appliquées avec l’API Fluent code First.</span><span class="sxs-lookup"><span data-stu-id="7a596-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="7a596-190">Vous avez également vu un certain nombre de points d’extensibilité pour personnaliser le comportement, que vous utilisiez l’interface `IValidatableObject` ou que vous appuyiez sur la méthode `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="7a596-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="7a596-191">Et ces deux derniers moyens de validation sont disponibles via le `DbContext`, que vous utilisiez le flux de travail Code First, Model First ou Database First pour décrire votre modèle conceptuel.</span><span class="sxs-lookup"><span data-stu-id="7a596-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
