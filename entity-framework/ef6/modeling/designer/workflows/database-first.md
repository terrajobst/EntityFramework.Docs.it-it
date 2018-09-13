---
title: Prima di tutto - database Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: b499dea02cbeaa64f6ef87bf89cc739110c8b560
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490922"
---
# <a name="database-first"></a>Prima di tutto di database
Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo di Database First con Entity Framework. Database prima di tutto consente di decodificare un modello da un database esistente. Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer. Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.

## <a name="watch-the-video"></a>Guarda il video
Questo video viene fornita un'introduzione allo sviluppo di Database First usando Entity Framework. Database prima di tutto consente di decodificare un modello da un database esistente. Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer. Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.

**Presentato da**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario avere almeno Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.

Se si usa Visual Studio 2010, è necessario anche avere [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.

 

## <a name="1-create-an-existing-database"></a>1. Creare un Database esistente

In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.

Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si usa Visual Studio 2010 si creerà un database SQL Express.
-   Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.

 

È possibile procedere e generare il database.

-   Aprire Visual Studio
-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**
-   Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati

    ![Seleziona l'origine dati](~/ef6/media/selectdatasource.png)

-   Connettersi a LocalDB o SQL Express, in base alla quale è stato installato e immettere **DatabaseFirst.Blogging** come nome del database

    ![Connessione di SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Connessione di Local DB DF](~/ef6/media/localdbconnectiondf.png)

-   Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**

    ![Creare una finestra di dialogo Database](~/ef6/media/createdatabasedialog.png)

-   Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**
-   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a>2. Creare l'applicazione

Per semplificare le operazioni verranno creare un'applicazione console di base che usa il Database First di eseguire l'accesso ai dati:

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel menu a sinistra e **applicazione Console**
-   Immettere **DatabaseFirstSample** come nome
-   Scegliere **OK**.

 

## <a name="3-reverse-engineer-model"></a>3. Modelli di reverse engineering

Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.

-   **Progetto -&gt; Aggiungi nuovo elemento...**
-   Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **BloggingModel** come nome, quindi fare clic su **OK**
-   Verrà avviata la **procedura guidata Entity Data Model**
-   Selezionare **genera da Database** e fare clic su **successivo**

    ![Passaggio della procedura guidata 1](~/ef6/media/wizardstep1.png)

-   Selezionare la connessione al database creato nella prima sezione, immettere **BloggingContext** come nome della stringa di connessione, scegliere **successivo**

    ![Passaggio della procedura guidata 2](~/ef6/media/wizardstep2.png)

-   Selezionare la casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'

    ![Passaggio della procedura guidata 3](~/ef6/media/wizardstep3.png)

 

Una volta completato il processo di reverse engineering il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer. Un file app. config è stato aggiunto anche al progetto con i dettagli della connessione per il database.

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

 

## <a name="4-reading--writing-data"></a>4. La lettura e scrittura dei dati

Ora che abbiamo un modello è possibile usarlo per accedere ai dati. Le classi andremo a utilizzare per accedere ai dati vengono generati automaticamente in base al file con estensione EDMX.

*Questa schermata è da Visual Studio 2012, se si usa Visual Studio 2010 il BloggingModel.tt e file BloggingModel.Context.tt saranno direttamente sotto il progetto anziché da annidato in file con estensione EDMX.*

![Classi generate DF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Gestione di modifiche del Database

A questo punto è possibile apportare alcune modifiche per lo schema di database, quando si apportano queste modifiche è necessario anche aggiornare il modello per riflettere tali modifiche.

Il primo passaggio è necessario apportare modifiche allo schema del database. Dobbiamo aggiungere una tabella di utenti allo schema.

-   Fare clic sui **DatabaseFirst.Blogging** del database in Esplora Server e selezionare **nuova Query**
-   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Ora che lo schema viene aggiornato, è possibile aggiornare il modello con tali modifiche.

-   Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare 'Aggiorna modello da Database... ', verrà avviata la procedura guidata Aggiorna
-   Nella scheda Aggiungi dei controlli effettuati dalla procedura guidata Aggiorna la casella accanto alle tabelle, ciò indica che si desidera aggiungere nuove tabelle dallo schema.
    *La scheda aggiornamento Mostra le tabelle esistenti nel modello che verrà controllato per le modifiche durante l'aggiornamento. Le schede Delete mostrano tutte le tabelle che sono stati rimossi dallo schema e verranno anche rimosso dal modello come parte dell'aggiornamento. Le informazioni su queste due schede viene rilevate automaticamente e viene fornite esclusivamente per scopi informativi, è possibile modificare le impostazioni.*

    ![Procedura guidata di aggiornamento](~/ef6/media/refreshwizard.png)

-   Fare clic su Fine della procedura guidata aggiornamento

 

Il modello è ora aggiornato per includere una nuova entità utente che esegue il mapping alla tabella Users che viene aggiunto al database.

![Modello aggiornato](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata che è stato esaminato lo sviluppo di Database First, che ha permesso di creare un modello nella finestra di progettazione di Entity Framework basato su un database esistente. Tale modello viene quindi usato per leggere e scrivere alcuni dati dal database. Infine, abbiamo aggiornato il modello per riflettere le modifiche sono apportate allo schema del database.
