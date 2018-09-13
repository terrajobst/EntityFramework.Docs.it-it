---
title: Code First per un Database esistente - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490610"
---
# <a name="code-first-to-an-existing-database"></a>Code First per un Database esistente
Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Code First come destinazione di un database esistente. Codice prima di tutto consente di definire il modello usando C\# o classi di Visual Basic.NET. Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.

## <a name="watch-the-video"></a>Guarda il video
In questo video viene [ora disponibile su Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Prerequisiti

Dovrai disporre **Visual Studio 2012** oppure **Visual Studio 2013** per completare questa procedura dettagliata.

È anche necessaria versione **6.1** (o versioni successive) delle **Entity Framework Tools per Visual Studio** installato. Visualizzare [ottenere Entity Framework](~/ef6/fundamentals/install.md) per informazioni sull'installazione della versione più recente di Entity Framework Tools.

## <a name="1-create-an-existing-database"></a>1. Creare un Database esistente

In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.

È possibile procedere e generare il database.

-   Aprire Visual Studio
-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**
-   Se si è ancora connessi a un database da **Esplora Server** prima di, devi selezionare **Microsoft SQL Server** come origine dati

    ![Seleziona l'origine dati](~/ef6/media/selectdatasource.png)

-   Connettersi all'istanza di Local DB e immettere **Blogging** come nome del database

    ![Connessione di Local DB](~/ef6/media/localdbconnection.png)

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Creare l'applicazione

Per semplificare le operazioni verranno creare un'applicazione console di base che usa Code First per eseguire l'accesso ai dati:

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel menu a sinistra e **applicazione Console**
-   Immettere **CodeFirstExistingDatabaseSample** come nome
-   Scegliere **OK**.

 

## <a name="3-reverse-engineer-model"></a>3. Modelli di reverse engineering

Dobbiamo usare Entity Framework Tools per Visual Studio per consentire agli utenti di generare il codice iniziale per eseguire il mapping al database. Questi strumenti sono semplicemente la generazione del codice che è possibile anche digitare manualmente se si preferisce.

-   **Progetto -&gt; Aggiungi nuovo elemento...**
-   Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **BloggingContext** come nome, quindi fare clic su **OK**
-   Verrà avviata la **procedura guidata Entity Data Model**
-   Selezionare **Code First dal Database** e fare clic su **successivo**

    ![Cluster virtuali una CFE procedura guidata](~/ef6/media/wizardonecfe.png)

-   Selezionare la connessione al database creato nella prima sezione e fare clic su **successivo**

    ![Cluster virtuali CFE due guidata](~/ef6/media/wizardtwocfe.png)

-   Fare clic sulla casella di controllo accanto a **tabelle** per importare tutte le tabelle e fare clic su **fine**

    ![Cluster virtuali CFE tre guidata](~/ef6/media/wizardthreecfe.png)

Una volta che un numero di elementi viene completato il processo di reverse engineering verranno aggiunti al progetto, è possibile ottenere un quadro di ciò che è stato aggiunto.

### <a name="configuration-file"></a>File di configurazione

Un file app. config è stato aggiunto al progetto, questo file contiene la stringa di connessione al database esistente.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Si noterà anche diverse altre impostazioni nel file di configurazione, queste sono impostazioni di Entity Framework predefinite che indicano a Code First in cui creare i database. Poiché viene mappato a un database esistente, queste impostazioni verranno ignorate nella nostra applicazione.*

### <a name="derived-context"></a>Contesto derivato

Oggetto **BloggingContext** classe è stato aggiunto al progetto. Il contesto rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati.
Espone il contesto di un **DbSet&lt;TEntity&gt;**  per ogni tipo nel modello. Si noterà anche che il costruttore predefinito chiama un costruttore di base usando la **nome =** sintassi. Indica a Code First che la stringa di connessione da utilizzare per questo contesto deve essere caricata dal file di configurazione.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*È consigliabile usare sempre la **nome =** sintassi quando si usa una stringa di connessione nel file di configurazione. In questo modo si garantisce che se la stringa di connessione non è presente l'Entity Framework genererà invece di creare un nuovo database per convenzione.*

### <a name="model-classes"></a>Classi del modello

Infine, un **Blog** e **Post** classe sono stati aggiunti anche al progetto. Queste sono le classi di dominio che costituiscono il modello. Si noterà annotazioni dei dati applicate alle classi per specificare la configurazione in cui le convenzioni Code First non sarà allineata con la struttura del database esistente. Ad esempio, si noterà il **StringLength** annotazione in **Blog.Name** e **Blog.Url** poiché presentano una lunghezza massima di **200** nel database (il valore predefinito di Code First consiste nell'usare la lunghezza massima supportata dal provider di database - **nvarchar (max)** in SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. La lettura e scrittura dei dati

Ora che abbiamo un modello è possibile usarlo per accedere ai dati. Implementare il **Main** metodo nella **Program.cs** come illustrato di seguito. Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo **Blog**. Quindi, viene utilizzata una query LINQ per recuperare tutti **blog** dal database ordinato alfabeticamente in base **titolo**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Cosa accade se cambia mio Database?

Code First guidata Database è progettato per generare un set di punto di partenza di classi che è possibile quindi perfezionare e modificare. Se viene modificato lo schema del database è possibile manualmente modificare le classi o eseguire un altro reverse engineering per sovrascrivere le classi.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Con migrazioni Code First per un Database esistente

Se si desidera utilizzare migrazioni Code First con un database esistente, vedere [migrazioni Code First per un database esistente](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata viene esaminato lo sviluppo Code First usando un database esistente. Abbiamo usato Entity Framework Tools per Visual Studio per decompilare un set di classi che è mappato al database e poteva essere usato per archiviare e recuperare i dati.
