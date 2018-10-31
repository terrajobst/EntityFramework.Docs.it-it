# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="6fb11-101">Contributo alla documentazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6fb11-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="6fb11-102">Questo documento illustra il processo per offrire il proprio contributo per gli articoli e gli esempi di codice ospitati nel [sito della documentazione di Entity Framework](https://docs.microsoft.com/ef).</span><span class="sxs-lookup"><span data-stu-id="6fb11-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="6fb11-103">I contributi possono essere semplici come le correzioni di errori di ortografia o complessi, ad esempio nuovi articoli.</span><span class="sxs-lookup"><span data-stu-id="6fb11-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="6fb11-104">Come effettuare una semplice correzione o suggerire una modifica</span><span class="sxs-lookup"><span data-stu-id="6fb11-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="6fb11-105">Gli articoli sono archiviati nel repository come file markdown.</span><span class="sxs-lookup"><span data-stu-id="6fb11-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="6fb11-106">È possibile apportare modifiche semplici al contenuto di un file markdown nel browser toccando il collegamento **Edit** (Modifica) nell'angolo superiore destro della finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="6fb11-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="6fb11-107">In una finestra del browser ridotta, potrebbe essere necessario espandere la barra delle **opzioni** per visualizzare il collegamento **Edit** (Modifica). Seguire le istruzioni per creare una richiesta pull.</span><span class="sxs-lookup"><span data-stu-id="6fb11-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="6fb11-108">La richiesta pull verrà esaminata dal team EF e accettata oppure verranno suggerite modifiche.</span><span class="sxs-lookup"><span data-stu-id="6fb11-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="6fb11-109">Come effettuare un invio più complesso</span><span class="sxs-lookup"><span data-stu-id="6fb11-109">How to make a more complex submission</span></span>

<span data-ttu-id="6fb11-110">Sono necessarie delle conoscenze di base di [Git e GitHub.com](https://guides.github.com/activities/hello-world/).</span><span class="sxs-lookup"><span data-stu-id="6fb11-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="6fb11-111">Aprire un [problema](https://github.com/aspnet/EntityFramework.Docs/issues/new) che descriva ciò che si vuole fare, ad esempio modificare un articolo esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="6fb11-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="6fb11-112">Attendere l'approvazione dal team EF prima di investire molto tempo.</span><span class="sxs-lookup"><span data-stu-id="6fb11-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="6fb11-113">Creare una copia tramite fork del repository [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) e creare un ramo per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6fb11-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="6fb11-114">Inviare una richiesta pull (PR) al master con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6fb11-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="6fb11-115">Rispondere ai commenti e suggerimenti per la richiesta pull.</span><span class="sxs-lookup"><span data-stu-id="6fb11-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="6fb11-116">Sintassi di Markdown</span><span class="sxs-lookup"><span data-stu-id="6fb11-116">Markdown syntax</span></span>

<span data-ttu-id="6fb11-117">Gli articoli sono scritti nel linguaggio [DocFx Flavored Markdown (DFM)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), che è un superset di [GitHub Flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span><span class="sxs-lookup"><span data-stu-id="6fb11-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="6fb11-118">Per esempi della sintassi DFM per le funzionalità dell'interfaccia utente comunemente usate nella documentazione di EF vedere il [modello di metadati e Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) nella guida di stile del repository .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6fb11-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="6fb11-119">Convenzioni per la struttura di cartelle</span><span class="sxs-lookup"><span data-stu-id="6fb11-119">Folder structure conventions</span></span>

<span data-ttu-id="6fb11-120">Le immagini e altri contenuti statici, vengono archiviati in una cartella `_static` all'interno di ogni cartella area/del sito.</span><span class="sxs-lookup"><span data-stu-id="6fb11-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="6fb11-121">Gli esempi di codice vengono archiviati nella cartella radice `samples`.</span><span class="sxs-lookup"><span data-stu-id="6fb11-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="6fb11-122">Sono organizzati in una struttura di cartelle che riproduce la struttura della documentazione (disponibile sotto la cartella radice `entity-framework`).</span><span class="sxs-lookup"><span data-stu-id="6fb11-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="6fb11-123">Frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="6fb11-123">Code snippets</span></span>

<span data-ttu-id="6fb11-124">Gli articoli contengono spesso frammenti di codice per illustrare gli argomenti discussi.</span><span class="sxs-lookup"><span data-stu-id="6fb11-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="6fb11-125">DFM consente di copiare codice nel file markdown o di fare riferimento a un file di codice separato.</span><span class="sxs-lookup"><span data-stu-id="6fb11-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="6fb11-126">È preferibile usare file di codice separati quando possibile, per ridurre al minimo la probabilità di errori nel codice.</span><span class="sxs-lookup"><span data-stu-id="6fb11-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="6fb11-127">I file di codice vengono archiviati nel repository usando la struttura di cartelle descritta in precedenza per i progetti di esempio.</span><span class="sxs-lookup"><span data-stu-id="6fb11-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="6fb11-128">Di seguito sono riportati alcuni esempi di [sintassi di frammenti di codice DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span><span class="sxs-lookup"><span data-stu-id="6fb11-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="6fb11-129">Per eseguire il rendering di un intero file di codice come un frammento:</span><span class="sxs-lookup"><span data-stu-id="6fb11-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="6fb11-130">Per eseguire il rendering di una parte di un file come un frammento usando i numeri di riga:</span><span class="sxs-lookup"><span data-stu-id="6fb11-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="6fb11-131">Per i frammenti di codice C#, è possibile fare riferimento a un'[area C#](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fb11-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="6fb11-132">Quando possibile, usare le aree anziché i numeri di riga, perché i numeri di riga in un file di codice tendono a cambiare e perdere la sincronizzazione con i riferimenti ai numeri di riga in Markdown.</span><span class="sxs-lookup"><span data-stu-id="6fb11-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="6fb11-133">Le aree C# possono essere annidate e, se si fa riferimento all'area esterna, le direttive interne `#region` e `#endregion` non vengono sottoposte a rendering in un frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="6fb11-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="6fb11-134">Per eseguire il rendering di un'area C# denominata "snippet_Example":</span><span class="sxs-lookup"><span data-stu-id="6fb11-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="6fb11-135">Per evidenziare le righe selezionate in un frammento di codice sottoposto a rendering (in genere il rendering viene eseguito con un colore di sfondo giallo):</span><span class="sxs-lookup"><span data-stu-id="6fb11-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="6fb11-136">Eseguire il test delle modifiche con DocFX</span><span class="sxs-lookup"><span data-stu-id="6fb11-136">Test your changes with DocFX</span></span>

<span data-ttu-id="6fb11-137">Eseguire il test delle modifiche con lo [strumento da riga di comando DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), che consente di creare una versione ospitata in locale del sito.</span><span class="sxs-lookup"><span data-stu-id="6fb11-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="6fb11-138">DocFX non esegue il rendering delle estensioni di stile e del sito create per docs.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="6fb11-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="6fb11-139">DocFX richiede .NET Framework in Windows o Mono (per Linux o macOS).</span><span class="sxs-lookup"><span data-stu-id="6fb11-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="6fb11-140">Istruzioni per Windows</span><span class="sxs-lookup"><span data-stu-id="6fb11-140">Windows instructions</span></span>

* <span data-ttu-id="6fb11-141">Scaricare e decomprimere *docfx.zip* da [DocFX releases](https://github.com/dotnet/docfx/releases) (Versioni di DocFX).</span><span class="sxs-lookup"><span data-stu-id="6fb11-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="6fb11-142">Aggiungere DocFX a PATH.</span><span class="sxs-lookup"><span data-stu-id="6fb11-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="6fb11-143">In una finestra della riga di comando, passare al repository clonato (che contiene il file *docfx.json*) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6fb11-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="6fb11-144">In un browser passare a `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="6fb11-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="6fb11-145">Istruzioni per Mono</span><span class="sxs-lookup"><span data-stu-id="6fb11-145">Mono instructions</span></span>

* <span data-ttu-id="6fb11-146">Installare Mono tramite Homebrew: `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="6fb11-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="6fb11-147">Scaricare la [versione più recente di DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span><span class="sxs-lookup"><span data-stu-id="6fb11-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="6fb11-148">Estrarre in `\bin\docfx`.</span><span class="sxs-lookup"><span data-stu-id="6fb11-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="6fb11-149">Creare un alias per **docfx**:</span><span class="sxs-lookup"><span data-stu-id="6fb11-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="6fb11-150">Eseguire **docfx** nel repository clonato per compilare il sito e **docfx-serve** per visualizzare il sito all'indirizzo `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="6fb11-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="6fb11-151">Voce e tono</span><span class="sxs-lookup"><span data-stu-id="6fb11-151">Voice and tone</span></span>

<span data-ttu-id="6fb11-152">L'obiettivo è quello di scrivere documentazione facilmente comprensibile per il più ampio pubblico possibile.</span><span class="sxs-lookup"><span data-stu-id="6fb11-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="6fb11-153">A tale scopo, sono state definite linee guida per lo stile di scrittura che i collaboratori dovranno seguire.</span><span class="sxs-lookup"><span data-stu-id="6fb11-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="6fb11-154">Per altre informazioni, vedere [Linee guida per la voce e il tono](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) nel repository .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6fb11-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
