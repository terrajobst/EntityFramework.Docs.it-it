# <a name="contributing-to-the-entity-framework-documentation"></a>Contributo alla documentazione di Entity Framework

Questo documento illustra il processo per offrire il proprio contributo per gli articoli e gli esempi di codice ospitati nel [sito della documentazione di Entity Framework](https://docs.microsoft.com/ef). I contributi possono essere semplici come le correzioni di errori di ortografia o complessi, ad esempio nuovi articoli.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>Come effettuare una semplice correzione o suggerire una modifica

Gli articoli sono archiviati nel repository come file markdown. È possibile apportare modifiche semplici al contenuto di un file markdown nel browser toccando il collegamento **Edit** (Modifica) nell'angolo superiore destro della finestra del browser. In una finestra del browser ridotta, potrebbe essere necessario espandere la barra delle **opzioni** per visualizzare il collegamento **Edit** (Modifica). Seguire le istruzioni per creare una richiesta pull. La richiesta pull verrà esaminata dal team EF e accettata oppure verranno suggerite modifiche.

## <a name="how-to-make-a-more-complex-submission"></a>Come effettuare un invio più complesso

Sono necessarie delle conoscenze di base di [Git e GitHub.com](https://guides.github.com/activities/hello-world/).

* Aprire un [problema](https://github.com/aspnet/EntityFramework.Docs/issues/new) che descriva ciò che si vuole fare, ad esempio modificare un articolo esistente o crearne uno nuovo. Attendere l'approvazione dal team EF prima di investire molto tempo.
* Creare una copia tramite fork del repository [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) e creare un ramo per le modifiche.
* Inviare una richiesta pull (PR) al master con le modifiche.
* Rispondere ai commenti e suggerimenti per la richiesta pull.

## <a name="markdown-syntax"></a>Sintassi di Markdown

Gli articoli sono scritti nel linguaggio [DocFx Flavored Markdown (DFM)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), che è un superset di [GitHub Flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/). Per esempi della sintassi DFM per le funzionalità dell'interfaccia utente comunemente usate nella documentazione di EF vedere il [modello di metadati e Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) nella guida di stile del repository .NET Core. 

## <a name="folder-structure-conventions"></a>Convenzioni per la struttura di cartelle

Le immagini e altri contenuti statici, vengono archiviati in una cartella `_static` all'interno di ogni cartella area/del sito.

Gli esempi di codice vengono archiviati nella cartella radice `samples`. Sono organizzati in una struttura di cartelle che riproduce la struttura della documentazione (disponibile sotto la cartella radice `entity-framework`).

## <a name="code-snippets"></a>Frammenti di codice

Gli articoli contengono spesso frammenti di codice per illustrare gli argomenti discussi. DFM consente di copiare codice nel file markdown o di fare riferimento a un file di codice separato. È preferibile usare file di codice separati quando possibile, per ridurre al minimo la probabilità di errori nel codice. I file di codice vengono archiviati nel repository usando la struttura di cartelle descritta in precedenza per i progetti di esempio.

Di seguito sono riportati alcuni esempi di [sintassi di frammenti di codice DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).

Per eseguire il rendering di un intero file di codice come un frammento:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

Per eseguire il rendering di una parte di un file come un frammento usando i numeri di riga:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

Per i frammenti di codice C#, è possibile fare riferimento a un'[area C#](https://msdn.microsoft.com/library/9a1ybwek.aspx). Quando possibile, usare le aree anziché i numeri di riga, perché i numeri di riga in un file di codice tendono a cambiare e perdere la sincronizzazione con i riferimenti ai numeri di riga in Markdown. Le aree C# possono essere annidate e, se si fa riferimento all'area esterna, le direttive interne `#region` e `#endregion` non vengono sottoposte a rendering in un frammento di codice.

Per eseguire il rendering di un'area C# denominata "snippet_Example":

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

Per evidenziare le righe selezionate in un frammento di codice sottoposto a rendering (in genere il rendering viene eseguito con un colore di sfondo giallo):

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a>Eseguire il test delle modifiche con DocFX

Eseguire il test delle modifiche con lo [strumento da riga di comando DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), che consente di creare una versione ospitata in locale del sito. DocFX non esegue il rendering delle estensioni di stile e del sito create per docs.microsoft.com.

DocFX richiede .NET Framework in Windows o Mono (per Linux o macOS).

### <a name="windows-instructions"></a>Istruzioni per Windows

* Scaricare e decomprimere *docfx.zip* da [DocFX releases](https://github.com/dotnet/docfx/releases) (Versioni di DocFX).
* Aggiungere DocFX a PATH.
* In una finestra della riga di comando, passare al repository clonato (che contiene il file *docfx.json*) ed eseguire il comando seguente:

   ``` console
   docfx -t default --serve
   ```

* In un browser passare a `http://localhost:8080`.

### <a name="mono-instructions"></a>Istruzioni per Mono

* Installare Mono tramite Homebrew: `brew install mono`.
* Scaricare la [versione più recente di DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).
* Estrarre in `\bin\docfx`.
* Creare un alias per **docfx**:

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* Eseguire **docfx** nel repository clonato per compilare il sito e **docfx-serve** per visualizzare il sito all'indirizzo `http://localhost:8080`.

## <a name="voice-and-tone"></a>Voce e tono

L'obiettivo è quello di scrivere documentazione facilmente comprensibile per il più ampio pubblico possibile. A tale scopo, sono state definite linee guida per lo stile di scrittura che i collaboratori dovranno seguire. Per altre informazioni, vedere [Linee guida per la voce e il tono](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) nel repository .NET Core.
