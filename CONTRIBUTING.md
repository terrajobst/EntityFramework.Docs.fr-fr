# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="c1b0a-101">Contribution à la documentation Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c1b0a-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="c1b0a-102">Ce document traite du processus de contribution aux articles et exemples de code hébergés sur le [site de la documentation Entity Framework](https://docs.microsoft.com/ef).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="c1b0a-103">Les contributions peuvent aller de la simple correction de fautes de frappe à la rédaction complexe de nouveaux articles.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="c1b0a-104">Comment apporter une correction ou suggestion simple</span><span class="sxs-lookup"><span data-stu-id="c1b0a-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="c1b0a-105">Les articles sont stockés dans le dépôt en tant que fichiers Markdown.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="c1b0a-106">Vous pouvez apporter des modifications simples au contenu d’un fichier Markdown dans le navigateur en appuyant sur le lien **Modifier** en haut à droite de la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="c1b0a-107">(Dans les fenêtres de navigateur étroites, vous devrez peut-être développer la barre **Options** pour voir le lien **Modifier**.) Suivez les instructions pour créer une demande de tirage (pull request).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="c1b0a-108">Après avoir examiné la demande de tirage, l’équipe EF l’accepte ou suggère des modifications.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="c1b0a-109">Comment effectuer une soumission plus complexe</span><span class="sxs-lookup"><span data-stu-id="c1b0a-109">How to make a more complex submission</span></span>

<span data-ttu-id="c1b0a-110">Vous devez avoir une connaissance de base de [Git et GitHub.com](https://guides.github.com/activities/hello-world/).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="c1b0a-111">Ouvrez un [dossier](https://github.com/aspnet/EntityFramework.Docs/issues/new) décrivant ce que vous voulez faire, par exemple modifier un article existant ou en créer un.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="c1b0a-112">Attendez l’approbation de l’équipe EF avant de vous investir davantage.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="c1b0a-113">Dupliquez (fork) le dépôt [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) et créez une branche pour vos modifications.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="c1b0a-114">Soumettez une demande de tirage dans la branche principale avec vos modifications.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="c1b0a-115">Répondez aux commentaires sur la demande de tirage.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="c1b0a-116">Syntaxe Markdown</span><span class="sxs-lookup"><span data-stu-id="c1b0a-116">Markdown syntax</span></span>

<span data-ttu-id="c1b0a-117">Les articles sont écrits en [DFM (DocFX Flavored Markdown)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), qui est un sur-ensemble de [GFM (GitHub Flavored Markdown)](https://guides.github.com/features/mastering-markdown/).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="c1b0a-118">Pour obtenir des exemples de syntaxe DFM illustrant les fonctionnalités de l’interface utilisateur couramment utilisées dans la documentation EF, consultez [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) dans le guide de style du dépôt .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="c1b0a-119">Conventions relatives à la structure des dossiers</span><span class="sxs-lookup"><span data-stu-id="c1b0a-119">Folder structure conventions</span></span>

<span data-ttu-id="c1b0a-120">Les images et autres formes de contenu statique sont stockées dans un dossier `_static` dans chaque zone/dossier du site.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="c1b0a-121">Les exemples de code sont stockés dans le dossier racine `samples`.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="c1b0a-122">Ils sont organisés dans une structure de dossiers qui reproduit la structure de la documentation (figurant sous le dossier racine `entity-framework`).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="c1b0a-123">Extraits de code</span><span class="sxs-lookup"><span data-stu-id="c1b0a-123">Code snippets</span></span>

<span data-ttu-id="c1b0a-124">Les articles contiennent souvent des extraits de code pour illustrer les points abordés.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="c1b0a-125">DFM vous permet de copier du code dans le fichier Markdown ou de faire référence à un fichier de code distinct.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="c1b0a-126">Dans la mesure du possible, nous préférons utiliser des fichiers de code distincts pour limiter les risques d’erreurs dans le code.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="c1b0a-127">Les fichiers de code doivent être stockés dans le dépôt, conformément à la structure de dossiers décrite ci-dessus pour les exemples de projets.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="c1b0a-128">Voici quelques exemples de [syntaxe d’extrait de code DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="c1b0a-129">Pour afficher un fichier de code entier en tant qu’extrait de code :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="c1b0a-130">Pour afficher une partie d’un fichier en tant qu’extrait de code en utilisant des numéros de ligne :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="c1b0a-131">Pour les extraits de code C#, vous pouvez référencer une [région C#](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="c1b0a-132">Si possible, utilisez des régions plutôt que des numéros de ligne, car les numéros de ligne dans un fichier de code ont tendance à changer au point de ne plus être synchronisés avec les références de numéro de ligne dans un fichier Markdown.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="c1b0a-133">Les régions C# peuvent être imbriquées, et si vous référencez la région externe, les directives interne `#region` et `#endregion` ne sont pas affichées dans un extrait de code.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="c1b0a-134">Pour afficher une région C# nommée « snippet_Example » :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="c1b0a-135">Pour mettre en surbrillance les lignes sélectionnées dans un extrait de code affiché (généralement sous la forme d’un arrière-plan jaune) :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="c1b0a-136">Tester vos modifications avec DocFX</span><span class="sxs-lookup"><span data-stu-id="c1b0a-136">Test your changes with DocFX</span></span>

<span data-ttu-id="c1b0a-137">Testez vos modifications avec l’[outil en ligne de commande DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), qui crée une version hébergée localement du site.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="c1b0a-138">DocFX n’affiche pas le style et les extensions de site créés pour docs.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="c1b0a-139">DocFX nécessite le .NET Framework sur Windows ou Mono pour Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="c1b0a-140">Instructions pour Windows</span><span class="sxs-lookup"><span data-stu-id="c1b0a-140">Windows instructions</span></span>

* <span data-ttu-id="c1b0a-141">Téléchargez et décompressez *docfx.zip* à partir des [versions DocFX](https://github.com/dotnet/docfx/releases).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="c1b0a-142">Ajoutez DocFX à votre chemin (PATH).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="c1b0a-143">Dans une fenêtre de ligne de commande, accédez au dépôt cloné (qui contient le fichier *docfx.json*) et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="c1b0a-144">Dans un navigateur, accédez à `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="c1b0a-145">Instructions pour Mono</span><span class="sxs-lookup"><span data-stu-id="c1b0a-145">Mono instructions</span></span>

* <span data-ttu-id="c1b0a-146">Installez Mono via Homebrew : `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="c1b0a-147">Téléchargez la [dernière version de DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span><span class="sxs-lookup"><span data-stu-id="c1b0a-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="c1b0a-148">Extrayez dans `\bin\docfx`.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="c1b0a-149">Créez un alias pour **docfx** :</span><span class="sxs-lookup"><span data-stu-id="c1b0a-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="c1b0a-150">Exécutez **docfx** dans le dépôt cloné pour générer le site, puis **docfx servi** pour afficher le site sur `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="c1b0a-151">Style et ton</span><span class="sxs-lookup"><span data-stu-id="c1b0a-151">Voice and tone</span></span>

<span data-ttu-id="c1b0a-152">Notre objectif est d’écrire une documentation facile à comprendre par le plus grand nombre.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="c1b0a-153">À cette fin, nous avons établi des recommandations pour le style rédactionnel que nous invitons nos contributeurs à suivre.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="c1b0a-154">Pour plus d’informations, consultez [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) dans le dépôt .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1b0a-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
