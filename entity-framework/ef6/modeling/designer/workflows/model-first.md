---
title: Model-First - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 8e010f95db40261073b4af80a3c0e3225a2cd1cf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490480"
---
# <a name="model-first"></a>A partire dal modello
Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Model First usando Entity Framework. Modello prima di tutto consente di creare un nuovo modello utilizzando Entity Framework Designer e quindi generare uno schema di database dal modello. Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer. Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.

## <a name="watch-the-video"></a>Guarda il video
Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Model First usando Entity Framework. Modello prima di tutto consente di creare un nuovo modello utilizzando Entity Framework Designer e quindi generare uno schema di database dal modello. Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer. Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.

**Presentato da**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Prerequisiti

Dovrai disporre di Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.

Se si usa Visual Studio 2010, è necessario anche avere [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.

## <a name="1-create-the-application"></a>1. Creare l'applicazione

Per semplificare le operazioni verranno creare un'applicazione console di base che usa il primo modello per eseguire l'accesso ai dati:

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel menu a sinistra e **applicazione Console**
-   Immettere **ModelFirstSample** come nome
-   Scegliere **OK**.

## <a name="2-create-model"></a>2. Crea modello

Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.

-   **Progetto -&gt; Aggiungi nuovo elemento...**
-   Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **BloggingModel** come nome, quindi fare clic su **OK**, verrà avviata la procedura guidata Entity Data Model
-   Selezionare **modello vuoto** e fare clic su **fine**

    ![Creare un modello vuoto](~/ef6/media/createemptymodel.png)

Entity Framework Designer viene aperto con un modello vuoto. A questo punto è possibile iniziare l'aggiunta di entità, proprietà e associazioni nel modello.

-   Fare clic su area di progettazione e seleziona **proprietà**
-   La modifica della finestra delle proprietà di **nome contenitore entità** al **BloggingContext**
    *si tratta del nome del contesto derivato che verrà generato automaticamente, il contesto rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati*
-   Fare clic su area di progettazione e seleziona **Aggiungi nuovo -&gt; Entity...**
-   Immettere **Blog** come nome dell'entità e **BlogId** come nome della chiave e fare clic su **OK**

    ![Aggiungere entità Blog](~/ef6/media/addblogentity.png)

-   Pulsante destro del mouse sulla nuova entità in area di progettazione e seleziona **Aggiungi nuovo -&gt; proprietà scalare**, immettere **nome** come il nome della proprietà.
-   Ripetere questo processo per aggiungere un **Url** proprietà.
-   Fare clic sul **Url** proprietà nell'area di progettazione e seleziona **proprietà**, la modifica della finestra delle proprietà di **Nullable** impostazione su **True** 
     *Ciò ci consente di risparmiare un Blog per il database senza assegnarle un Url*
-   Utilizzando le tecniche che appena appreso, aggiungere un **Post** entità con un **PostId** proprietà chiave
-   Aggiungere **Title** e **contenuto** alle proprietà scalari per il **Post** entità

Ora che abbiamo un paio di entità, è possibile aggiungere un'associazione (o la relazione) tra di essi.

-   Fare clic su area di progettazione e seleziona **Aggiungi nuovo -&gt; Association...**
-   Rendere un'estremità della relazione punto a **Blog** con una molteplicità pari **uno** e l'altro punto di fine per **Post** con una molteplicità pari a **molti** 
     *Ciò significa che un Blog dispone di molti inserimenti e una richiesta Post a cui appartiene un Blog*
-   Verificare che il **aggiungere le proprietà di chiave esterna 'Post' entità** casella è selezionata e fare clic su **OK**

    ![Aggiungere un'associazione MF](~/ef6/media/addassociationmf.png)

È ora disponibile un modello semplice che è possibile generare un database e usare per leggere e scrivere dati.

![Modello iniziale](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Passaggi aggiuntivi in Visual Studio 2010

Se si lavora in Visual Studio 2010, esistono alcuni passaggi aggiuntivi da seguire per eseguire l'aggiornamento alla versione più recente di Entity Framework. L'aggiornamento è importante perché consente di accedere a una superficie dell'API migliorata, che è molto più semplice da utilizzare, nonché le correzioni di bug più recenti.

Innanzitutto, è necessario ottenere la versione più recente di Entity Framework da NuGet.

-   **Progetto –&gt; Gestisci pacchetti NuGet... ** 
     *Se non si dispone di **Gestisci pacchetti NuGet... ** opzione, è necessario installare il [versione più recente di NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Selezionare il **Online** scheda
-   Selezionare il **EntityFramework** pacchetto
-   Fare clic su **installare**

Successivamente, è necessario scambiare il nostro modello per generare il codice che usa l'API DbContext, che è stato introdotto nelle versioni più recenti di Entity Framework.

-   Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**
-   Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**
-   Selezionare il EF **5.x generatore di DbContext per C\#**, immettere **BloggingModel** come nome, quindi fare clic su **Add**

    ![Modello di DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Generazione del Database

Dato il modello, Entity Framework consente di calcolare uno schema di database che ci consentirà di archiviare e recuperare dati usando il modello.

Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si usa Visual Studio 2010 si creerà un database SQL Express.
-   Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.

È possibile procedere e generare il database.

-   Fare clic su area di progettazione e seleziona **genera Database da modello...**
-   Fare clic su **nuova connessione...** e specificare LocalDB o SQL Express, a seconda di quale versione di Visual Studio in uso, immettere **ModelFirst.Blogging** come nome del database.

    ![Connessione di Local DB MF](~/ef6/media/localdbconnectionmf.png)

    ![Connessione di SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**
-   Selezionare **successivo** ed Entity Framework Designer consentirà di calcolare uno script per creare lo schema del database
-   Quando viene visualizzato lo script, fare clic su **fine** e lo script verrà aggiunto al progetto e aperto
-   Fare clic su script e selezionare **Execute**, verrà richiesto di specificare il database a cui connettersi, specificare LocalDB o Express di SQL Server, a seconda di quale versione di Visual Studio in uso

## <a name="4-reading--writing-data"></a>4. La lettura e scrittura dei dati

Ora che abbiamo un modello è possibile usarlo per accedere ai dati. Le classi andremo a utilizzare per accedere ai dati vengono generati automaticamente in base al file con estensione EDMX.

*Questa schermata è da Visual Studio 2012, se si usa Visual Studio 2010 il BloggingModel.tt e file BloggingModel.Context.tt saranno direttamente sotto il progetto anziché da annidato in file con estensione EDMX.*

![Classi generate](~/ef6/media/generatedclasses.png)

Implementare il metodo Main in Program.cs, come illustrato di seguito. Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo post di Blog. Usa quindi una query LINQ per recuperare tutti i blog dal database ordinato alfabeticamente in base al titolo.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

È ora possibile eseguire l'applicazione e testarlo.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a>5. Gestione di modifiche al modello

A questo punto è possibile apportare alcune modifiche per il modello, quando si apportano queste modifiche è necessario anche aggiornare lo schema del database.

Si inizierà aggiungendo una nuova entità utente per il modello.

-   Aggiungere un nuovo **utente** nome dell'entità con **Username** come nome della chiave e **stringa** come tipo di proprietà per la chiave

    ![Aggiungere l'entità User](~/ef6/media/adduserentity.png)

-   Fare clic sul **Username** proprietà nell'area di progettazione e seleziona **proprietà**, modificano proprietà il **MaxLength** se si imposta su **50 ** 
     *Limita i dati che possono essere archiviati in nome utente da 50 caratteri*
-   Aggiungere un **DisplayName** proprietà scalare per il **utente** entità

È ora disponibile un modello aggiornato e siamo pronti aggiornare il database per contenere il nuovo tipo di entità utente.

-   Fare clic su area di progettazione e seleziona **genera Database da modello...** , Entity Framework calcolerà uno script per ricreare uno schema basato sul modello aggiornato.
-   Fare clic su **fine**
-   Potrebbe ricevere avvisi relativi a sovrascrivere lo script DDL esistente e le parti di mapping e di archiviazione del modello, fare clic su **Sì** per entrambi questi avvisi
-   Lo script aggiornato SQL per creare il database viene aperto automaticamente  
    *Lo script generato verrà eliminare tutte le tabelle esistenti e quindi ricreare lo schema da zero. Questo potrebbe funzionare per lo sviluppo locale ma non è un valido per il push delle modifiche a un database che è già stato distribuito. Se si desidera pubblicare le modifiche in un database in cui è già stato distribuito, è necessario modificare lo script o usare uno strumento di confronto schema per la quale calcolare uno script di migrazione.*
-   Fare clic su script e selezionare **Execute**, verrà richiesto di specificare il database a cui connettersi, specificare LocalDB o Express di SQL Server, a seconda di quale versione di Visual Studio in uso

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata che è stata esaminata sviluppo Model First, che ha permesso di creare un modello nella finestra di progettazione di Entity Framework e quindi generare un database da tale modello. Il modello viene quindi usato per leggere e scrivere alcuni dati dal database. Infine, abbiamo aggiornato il modello e quindi ricreata lo schema del database in modo che corrisponda il modello.
