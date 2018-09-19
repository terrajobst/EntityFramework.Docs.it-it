---
title: Code First per un nuovo Database - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: a19db575b685cde98509fff4a0efaf26106b26bc
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284122"
---
# <a name="code-first-to-a-new-database"></a>Code First per un nuovo Database
Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Code First come destinazione di un nuovo database. Questo scenario include come destinazione di un database che non esiste e Code First creerà un database vuoto che Code First la aggiungerà nuove tabelle per. Codice prima di tutto consente di definire il modello usando C\# o classi di Visual Basic.NET. Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.

## <a name="watch-the-video"></a>Guarda il video
Questo video viene fornita un'introduzione allo sviluppo Code First come destinazione di un nuovo database. Questo scenario include come destinazione di un database che non esiste e Code First creerà un database vuoto che Code First la aggiungerà nuove tabelle per. Codice prima di tutto consente di definire il modello di utilizzo di classi c# o VB.Net. Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.

**Presentato da**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario avere almeno Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.

Se si usa Visual Studio 2010, è necessario anche avere [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.

## <a name="1-create-the-application"></a>1. Creare l'applicazione

Per semplificare le operazioni verranno creare un'applicazione console di base che usa Code First per eseguire l'accesso ai dati.

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel menu a sinistra e **applicazione Console**
-   Immettere **CodeFirstNewDatabaseSample** come nome
-   Scegliere **OK**.

## <a name="2-create-the-model"></a>2. Creare il modello

È importante definire un modello molto semplice usando le classi. Abbiamo appena stiamo definirli nel file Program.cs, ma in un'applicazione reale che si sarebbero suddividere le classi out in file separati e potenzialmente un progetto separato.

Sotto la definizione della classe Program nel file Program.cs aggiungere le due classi seguenti.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Si noterà che stiamo rendendo le due proprietà di navigazione (Blog.Posts e Post.Blog) virtuale. In questo modo la funzionalità di caricamento Lazy di Entity Framework. Il caricamento lazy significa che il contenuto di queste proprietà verrà caricato automaticamente dal database quando si tenta di accedervi.

## <a name="3-create-a-context"></a>3. Creare un contesto

A questo punto è possibile definire un contesto derivato che rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati. Definiamo un contesto che deriva da DbContext ed espone un elemento DbSet tipizzato&lt;TEntity&gt; per ogni classe nel modello.

Stiamo iniziando adesso a usare tipi da Entity Framework, pertanto è necessario aggiungere il pacchetto EntityFramework NuGet.

-   **Progetto –&gt; Gestisci pacchetti NuGet...**
    Nota: Se non si dispone di **Gestisci pacchetti NuGet...** opzione, è necessario installare il [versione più recente di NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Selezionare il **Online** scheda
-   Selezionare il **EntityFramework** pacchetto
-   Fare clic su **installare**

Aggiungere un tramite l'istruzione per Data. Entity nella parte superiore del file Program.cs.

``` csharp
using System.Data.Entity;
```

Sotto il Post di classe nel file Program.cs aggiungere contesto derivato seguente.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Ecco un elenco completo delle novità Program.cs dovrebbe ora contenere.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

Questo è tutto il codice che è necessario iniziare l'archiviazione e recupero dei dati. Ovviamente non c'è qualche accade dietro le quinte e prenderemo in esame che a breve ma prima è opportuno vederlo in azione.

## <a name="4-reading--writing-data"></a>4. La lettura e scrittura dei dati

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
### <a name="wheres-my-data"></a>Dove si trova i miei dati?

Per convenzione DbContext ha creato un database.

-   Se è disponibile un'istanza di SQL Express locale (installato per impostazione predefinita con Visual Studio 2010) quindi Code First ha creato il database in tale istanza
-   Se SQL Express non è disponibile, Code First tenta di usare [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installato per impostazione predefinita con Visual Studio 2012)
-   Il database è denominato dopo il nome completo del contesto derivato, in questo caso che è **CodeFirstNewDatabaseSample.BloggingContext**

Questi sono solo le convenzioni predefinite e sono disponibili vari modi per modificare il database che usa Code First, altre informazioni sono disponibili nel **come DbContext consente di individuare il modello e connessione al Database** argomento.
È possibile connettersi al database tramite Esplora Server in Visual Studio

-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati** e selezionare **Aggiungi connessione...**
-   Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati

    ![Seleziona l'origine dati](~/ef6/media/selectdatasource.png)

-   Connettersi a LocalDB o SQL Express, in base alla quale è stato installato

È ora possibile controllare lo schema che Code First ha creato.

![Schema iniziale](~/ef6/media/schemainitial.png)

DbContext elaborati quali classi da includere nel modello esaminando le proprietà DbSet che è stato definito. Viene quindi utilizzato il set predefinito di convenzioni Code First per determinare i nomi di tabella e colonna, determinare i tipi di dati, trovare le chiavi primarie e così via. Più avanti in questa procedura dettagliata esamineremo come è possibile eseguire l'override di queste convenzioni.

## <a name="5-dealing-with-model-changes"></a>5. Gestione di modifiche al modello

A questo punto è possibile apportare alcune modifiche per il modello, quando si apportano queste modifiche è necessario anche aggiornare lo schema del database. Per eseguire questa operazione si intende utilizzare una funzionalità denominata migrazioni Code First o migrazioni per brevità.

Le migrazioni consente di disporre di un set ordinato di procedure che descrivono come aggiornare lo schema di database (e il downgrade). Ognuno di questi passaggi, noti come una migrazione, contiene un codice che descrive le modifiche da applicare. 

Il primo passaggio è abilitare migrazioni Code First per nostro BloggingContext.

-   **Strumenti -&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**
-   Eseguire il comando **Enable-Migrations** nella console di Gestione pacchetti
-   È stata aggiunta una nuova cartella Migrations per il progetto che contiene due elementi:
    -   **Configuration.cs** : questo file contiene le impostazioni che userà Migrations per eseguire la migrazione BloggingContext. Non è necessario modificare alcun valore per questa procedura dettagliata, ma Ecco dove è possibile specificare dati di seeding, i provider di registrazione per gli altri database, viene modificato lo spazio dei nomi che vengono generate le migrazioni e così via.
    -   **&lt;timestamp&gt;\_InitialCreate.cs** – si tratta di una prima migrazione, rappresenta le modifiche che sono già state applicate al database per portarlo da in corso di un database vuoto che includa le tabelle di blog e post . Anche se viene consentito di Code First di creare automaticamente le tabelle seguenti, ora che si è scelto alle migrazioni sono stati convertiti in una migrazione. Prima di tutto i codice anche è registrato nel nostro database locale che questa migrazione è già stata applicata. Il timestamp nel nome file viene usato per scopi di ordinamento.

    A questo punto è possibile apportare una modifica al modello, aggiungere una proprietà Url per la classe di Blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Eseguire la **Add-Migration AddUrl** comando nella Console di gestione pacchetti.
    Il comando Add-Migration controlli le modifiche apportate dopo l'ultima migrazione ed esegue scaffolding di una nuova migrazione con eventuali modifiche che si trovano. In questo modo, le migrazioni da un nome. In questo caso stiamo chiamando il 'AddUrl' la migrazione.
    Il codice con scaffolding è che informa che è necessario aggiungere una colonna di Url, che può contenere dati di tipo stringa, dbo. Blog su tabella. Se necessario, è stato possibile modificare il codice con scaffolding ma che in questo caso non è obbligatorio.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Eseguire la **Update-Database** comando nella Console di gestione pacchetti. Questo comando applicherà eventuali migrazioni in sospeso al database. La nostra migrazione InitialCreate è già stato applicato in modo che le migrazioni solo applicherà la nostra nuova migrazione AddUrl.
    Suggerimento: È possibile usare la **– Verbose** quando si chiama Update-Database per visualizzare il codice SQL in esecuzione sul database.

La nuova colonna Url viene ora aggiunto alla tabella nel database di blog:

![Schema con Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Annotazioni dei dati

Finora abbiamo semplicemente facciamo EF individuare il modello usando le convenzioni predefinite, ma vi saranno ora le classi non seguono le convenzioni e dobbiamo essere in grado di eseguire un'ulteriore configurazione. Sono disponibili due opzioni per questo oggetto. Esamineremo le annotazioni dei dati in questa sezione e quindi l'API fluent nella sezione successiva.

-   È possibile aggiungere una classe utente per il nostro modello

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   È inoltre necessario aggiungere un set per il contesto derivato

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Se si tentasse di aggiungere una migrazione, si otterrebbe un errore che indica che "*EntityType 'User' non ha chiavi definite. Definire la chiave per EntityType."* Poiché Entity Framework non ha modo di sapere che il nome utente deve essere la chiave primaria per l'utente.
-   Si intende usare le annotazioni dei dati a questo punto dobbiamo aggiungere using all'inizio del file Program.cs l'istruzione

```
using System.ComponentModel.DataAnnotations;
```

-   A questo punto annotare la proprietà Username per identificare la chiave primaria

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Usare la **Add-Migration AddUser** comando per eseguire lo scaffolding di una migrazione per applicare queste modifiche al database
-   Eseguire la **Update-Database** comando per applicare la nuova migrazione al database

La nuova tabella viene ora aggiunto al database:

![Schema con gli utenti](~/ef6/media/schemawithusers.png)

L'elenco completo di annotazioni supportate da Entity Framework è:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [Oggetto StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. API Fluent

Nella sezione precedente è stato illustrato l'utilizzo delle annotazioni dei dati per integrare o sostituire elementi rilevati mediante la convenzione. L'altro modo per configurare il modello è tramite l'API Office fluent Code First.

La maggior parte delle configurazione del modello può essere eseguita tramite le annotazioni di dati semplici. L'API fluent è un modo più avanzato di specifica di configurazione del modello che vengono trattate tutte le annotazioni dei dati possono eseguire anche per alcune configurazioni più avanzate non sono disponibili con le annotazioni dei dati. Le annotazioni dei dati e l'API fluent possono essere usati insieme.

Per accedere all'API fluent è eseguire l'override del metodo OnModelCreating in DbContext. Si supponga di voler rinominare la colonna archiviata in User. DisplayName per visualizzare\_nome.

-   Eseguire l'override del metodo OnModelCreating in BloggingContext con il codice seguente

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Usare la **Add-Migration ChangeDisplayName** comando per eseguire lo scaffolding di una migrazione per applicare queste modifiche al database.
-   Eseguire la **Update-Database** comando per applicare la nuova migrazione al database.

La colonna DisplayName è stata rinominata per visualizzare\_nome:

![Schema con il nome visualizzato rinominato](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata viene esaminato lo sviluppo Code First con un nuovo database. Viene definito un modello usando le classi quindi tale modello consente di creare un database e archiviare e recuperare i dati. Dopo che è stato creato il database viene usato migrazioni Code First per modificare lo schema come il nostro modello sviluppato. È stato anche illustrato come configurare un modello tramite le annotazioni dei dati e l'API Fluent.
