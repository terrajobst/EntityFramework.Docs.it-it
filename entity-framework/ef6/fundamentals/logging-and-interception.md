---
title: Registrazione e l'intercettazione di operazioni di database - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 9a8be81af45d9f27caa8c26f66d219dc568b6604
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251271"
---
# <a name="logging-and-intercepting-database-operations"></a>Registrazione e l'intercettazione di operazioni di database
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

A partire da Entity Framework 6, ogni volta che Entity Framework comando viene inviato un comando nel database possono essere intercettati dal codice dell'applicazione. Viene in genere utilizzato per l'accesso SQL, ma può anche essere utilizzato per modificare o interrompere il comando.  

In particolare, Entity Framework include:  

- Una proprietà di Log per il contesto simile a DataContext.Log in LINQ to SQL  
- Un meccanismo per personalizzare la formattazione dell'output inviato al Registro  
- Blocchi predefiniti di basso livello per l'intercettazione consente una maggiore flessibilità di controllo /  

## <a name="context-log-property"></a>Proprietà di contesto del Log  

La proprietà DbContext.Database.Log può essere impostata su un delegato per qualsiasi metodo che accetta una stringa. In genere viene utilizzato con qualsiasi TextWriter impostandolo sul metodo di "Scrittura" di tale oggetto TextWriter. Tutti i SQL generato dal contesto corrente verrà registrato in tale writer. Ad esempio, il codice seguente registrerà SQL nella console:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Si noti che tale contesto. Database. log è impostato su console. Write. Questo è tutto ciò che è necessaria per l'accesso SQL nella console.  

Aggiungere un semplice codice di aggiornamento/inserimento/query in modo che possiamo vedere alcuni output:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Verrà generato l'output seguente:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Si noti che questo è l'output presupponendo che le operazioni di inizializzazione del database è già verificato. Se l'inizializzazione del database non era già verificato, vi sarà molto più output che mostra tutte le operazioni migrazioni non dietro le quinte per cercare o creare un nuovo database.)  

## <a name="what-gets-logged"></a>Ciò che viene registrato?  

Viene registrata quando è impostata la proprietà del Log di tutti gli elementi seguenti:  

- SQL per tutti i diversi tipi di comandi. Ad esempio:  
    - Query, incluse le query LINQ normali, query eSQL e query non elaborate dai metodi, ad esempio SqlQuery  
    - Gli inserimenti, aggiornamenti ed eliminazioni generate come parte del metodo SaveChanges  
    - Relazioni caricano query, ad esempio quelli generati dal caricamento lazy  
- Parametri  
- Se viene eseguito in modo asincrono il comando  
- Un timestamp che indica quando il comando avviato l'esecuzione  
- Se il comando completato correttamente, non è riuscita generando un'eccezione o, per async, è stata annullata  
- Alcune indicazioni del valore del risultato  
- La quantità approssimativa del tempo che necessario per eseguire il comando. Si noti che questo è il tempo dall'invio del comando per ottenere l'oggetto del risultato. Non include il tempo per leggere i risultati.  

Esaminando l'output di esempio sopra riportato, ognuno dei quattro comandi registrati sono:  

- La query risultante dalla chiamata al contesto. Blogs.First  
    - Si noti che il metodo ToString dell'introduzione di SQL non avrebbero funzionato per questa query perché "First" non è incluso un oggetto IQueryable su cui chiamare ToString  
- La query risultante dal caricamento lazy di blog. Post  
    - Si noti che i dettagli dei parametri per il valore della chiave per cui il caricamento lazy è in corso  
    - Vengono registrate solo le proprietà del parametro che sono impostate su valori non predefiniti. Ad esempio, la proprietà dimensioni solo viene visualizzata se è diverso da zero.  
- Due comandi risultanti da SaveChangesAsync; una per l'aggiornamento modificare un titolo di post, l'altro per un'operazione di inserimento aggiungere un nuovo post  
    - Si noti che i dettagli dei parametri per le proprietà di chiave esterna e il titolo  
    - Si noti che questi comandi vengono eseguiti in modo asincrono  

## <a name="logging-to-different-places"></a>Registrazione in posizioni diverse  

Come illustrato in precedenza la registrazione per la console è estremamente facile. È inoltre facile accedere alla memoria, file e così via usando diversi tipi di [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Se si ha familiarità con LINQ to SQL è possibile notare che in LINQ to SQL con la proprietà di Log è impostata su TextWriter oggetto effettivo (ad esempio, console. Out) mentre in Entity Framework è impostata la proprietà del Log a un metodo che accetta una stringa (ad esempio Write o Console.Out.Write). Il motivo è separare EF da TextWriter mediante l'accettazione di un delegato che può agire come un sink per le stringhe. Ad esempio, si supponga che si dispone già di alcuni framework di registrazione e definisce un metodo di registrazione come illustrato di seguito:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Ciò può essere associata a proprietà EF Log simile al seguente:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Registrazione risultati  

Il logger predefinito registra il testo del comando (SQL), parametri e la riga "Executing" con un timestamp prima il comando viene inviato al database. Una riga "completata" che contiene il tempo trascorso è connesso seguendo l'esecuzione del comando.  

Si noti che per i comandi asincroni la riga "completata" non è connesso fino a quando l'attività asincrona in realtà viene completata, ha esito negativo o viene annullato.  

La riga "completata" contiene informazioni diverse a seconda del tipo di comando e indica se l'esecuzione è stata completata.  

### <a name="successful-execution"></a>Il completamento dell'esecuzione  

Per i comandi che completano correttamente l'output è "completato in x ms con risultato:" seguita da alcune indicazioni relative Qual era il risultato. Per i comandi che restituiscono il risultato di un lettore dati indicazione è il tipo della [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) restituito. Per i comandi che restituiscono un valore integer, ad esempio l'aggiornamento comando illustrato in precedenza il risultato visualizzato è integer.  

### <a name="failed-execution"></a>Esecuzione non riuscita  

Per i comandi che non soddisfano generando un'eccezione, l'output contiene il messaggio dell'eccezione. Ad esempio, uso SqlQuery per query su una tabella esistente verrà risultato nel registro output simile al seguente:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Esecuzione annullata  

Per i comandi asincroni in cui l'attività viene annullata il risultato potrebbe essere un errore con un'eccezione, poiché si tratta di provider ADO.NET sottostante spesso funzionamento quando viene effettuato un tentativo di annullamento. Se ciò non accade e l'attività viene annullata normalmente quindi l'output avrà un aspetto simile al seguente:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Impostazione del contenuto dei log e la formattazione  

Dietro le quinte il database. log proprietà rende l'utilizzo di un oggetto DatabaseLogFormatter. Questo oggetto associa in modo efficace un'implementazione IDbCommandInterceptor (vedere sotto) a un delegato che accetta stringhe e un oggetto DbContext. Ciò significa che in DatabaseLogFormatter vengono chiamati prima e dopo l'esecuzione di comandi da Entity Framework. Questi metodi DatabaseLogFormatter raccogliere e formattare l'output di log e inviarlo al delegato.  

### <a name="customizing-databaselogformatter"></a>Personalizzazione DatabaseLogFormatter  

Modificando ciò che viene registrato e come viene formattato può essere ottenuto creando una nuova classe che deriva da DatabaseLogFormatter e sovrascrive i metodi appropriati. I metodi più comuni per eseguire l'override sono:  

- LogCommand: eseguire l'Override di questa opzione per cambiare modalità di registrazione comandi prima che vengano eseguite. Per impostazione predefinita LogCommand chiama LogParameter per ogni parametro. è possibile scegliere di eseguire la stessa operazione nell'override o gestire parametri invece in modo diverso.  
- LogResult: eseguire l'Override per modificare la modalità in cui è connesso il risultato dall'esecuzione di un comando.  
- LogParameter: eseguire l'Override per modificare la formattazione e il contenuto della registrazione di parametro.  

Ad esempio, di voler accedere un'unica riga prima di ogni comando viene inviato al database. Questa operazione può essere eseguita con due sostituzioni:  

- Eseguire l'override LogCommand per formattare e scrivere la riga singola di SQL  
- Eseguire l'override LogResult per non eseguire alcuna operazione.  

Il codice sarebbe simile al seguente:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Per registrare l'output semplicemente chiamare il metodo di scrittura che invia output al delegato scrittura configurata.  

(Si noti che questo codice esegue semplicistico per la rimozione di interruzioni di riga solo come esempio. Probabilmente non funzionerà correttamente per la visualizzazione di istruzioni SQL complesse.)  

### <a name="setting-the-databaselogformatter"></a>Impostazione di DatabaseLogFormatter  

Dopo aver creata una nuova classe DatabaseLogFormatter deve essere registrata con Entity Framework. Questa operazione viene eseguita utilizzando la configurazione basata su codice. In poche parole, ciò significa creare una nuova classe che deriva da DbConfiguration nello stesso assembly come la classe DbContext e chiamando quindi SetDatabaseLogFormatter nel costruttore di questa nuova classe. Ad esempio:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Usando il nuovo DatabaseLogFormatter  

Questo nuovo DatabaseLogFormatter ora essere usato ogni volta che viene impostato database. log. Quindi, eseguire il codice della parte 1 ora genererà l'output seguente:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Blocchi predefiniti di intercettazione  

Finora abbiamo esaminato come utilizzare DbContext.Database.Log per registrare il codice SQL generato da Entity Framework. Ma questo codice è effettivamente una facciata relativamente thin su alcuni blocchi predefiniti di basso livello per l'intercettazione più generale.  

### <a name="interception-interfaces"></a>Interfacce di intercettazione  

Il codice di intercettazione è basato sul concetto di interfacce di intercettazione. Queste interfacce di ereditano da IDbInterceptor e definiscono i metodi che vengono chiamati quando Entity Framework esegue una determinata azione. Si desidera avere un'interfaccia per ogni tipo di oggetto intercettato. Ad esempio, l'interfaccia IDbCommandInterceptor definisce i metodi che vengono chiamati prima EF effettua una chiamata a metodi correlati, ExecuteReader, ExecuteScalar ed ExecuteNonQuery. Analogamente, l'interfaccia definisce i metodi che vengono chiamati al completamento di ognuna di queste operazioni. La classe DatabaseLogFormatter che abbiamo visto in precedenza implementa questa interfaccia per registrare i comandi.  

### <a name="the-interception-context"></a>Il contesto di intercettazione  

Osservando i metodi definiti in una delle interfacce degli intercettori, è evidente che ogni chiamata è dato un oggetto di tipo DbInterceptionContext o un tipo derivato da questo oggetto, ad esempio DbCommandInterceptionContext\<\>. Questo oggetto contiene informazioni contestuali relativi all'azione che richiede di Entity Framework. Ad esempio, se viene intrapresa l'azione per conto di un oggetto DbContext, DbContext è incluso nel DbInterceptionContext. Analogamente, per i comandi che vengono eseguiti in modo asincrono, il flag IsAsync è nastavit DbCommandInterceptionContext.  

### <a name="result-handling"></a>Gestione di risultati  

Il DbCommandInterceptionContext\< \> classe contiene una proprietà denominata risultati, OriginalResult, eccezioni e OriginalException. Queste proprietà vengono impostate su null/zero per le chiamate ai metodi che vengono chiamati prima che l'operazione viene eseguita l'intercettazione, vale a dire, per il... Esecuzione dei metodi. Se l'operazione viene eseguita e ha esito positivo, quindi risultato e OriginalResult vengono impostate sul risultato dell'operazione. Questi valori possono quindi essere osservati nel metodo di intercettazione che viene chiamati dopo che è stata eseguita l'operazione, vale a dire nel... Metodi eseguiti. Analogamente, se l'operazione genera, quindi le proprietà di eccezione e OriginalException verranno impostate.  

#### <a name="suppressing-execution"></a>Eliminazione di esecuzione  

Se un intercettore imposta la proprietà Result prima ha eseguito il comando (in uno di... Esecuzione di metodi) quindi Entity Framework non tenta di eseguire effettivamente il comando, ma verrà usato invece solo il set di risultati. In altre parole, l'intercettore può eliminare l'esecuzione del comando ma EF continua come se fosse stato eseguito il comando.  

Un esempio di come ciò è possibile usare è il comando batch che è stati condotti in genere con un provider di ritorno a capo. L'intercettore sarebbe per memorizzare il comando per un'esecuzione successiva come batch, ma sarebbe "fingono" di Entity Framework che ha eseguito il comando come di consueto. Si noti che richiede più di ciò per implementare l'invio in batch, ma questo è un esempio di come modificare il risultato di intercettazione è possibile usare.  

L'esecuzione può essere evitata impostando la proprietà di eccezione in uno del... Esecuzione dei metodi. Ciò fa sì che EF continuare come se l'esecuzione dell'operazione non è riuscito generando l'eccezione specificata. Ciò, naturalmente, potrebbe essere l'applicazione in modo anomalo, ma potrebbe anche essere un'eccezione temporanea o un'altra eccezione che viene gestita da Entity Framework. Ad esempio, ciò potrebbe utilizzabile in ambienti di test per testare il comportamento di un'applicazione quando l'esecuzione del comando ha esito negativo.  

#### <a name="changing-the-result-after-execution"></a>Modificare il risultato dopo l'esecuzione  

Se un intercettore imposta la proprietà del risultato dopo avere eseguito il comando (in uno di... Eseguire i metodi) Entity Framework userà il risultato modificato anziché il risultato che è stata effettivamente restituito dall'operazione. Analogamente, se un intercettore imposta la proprietà di eccezione dopo avere eseguito il comando, quindi Entity Framework genererà l'eccezione di set come se l'operazione ha generato l'eccezione.  

Un intercettore può anche impostare la proprietà dell'eccezione su null per indicare che deve essere generata alcuna eccezione. Ciò può essere utile se l'esecuzione dell'operazione non riuscita, ma l'intercettore intenda continuare come se l'operazione ha avuto esito positivo a Entity Framework. Ciò comporta in genere anche impostando il risultato in modo che Entity Framework ha un valore di risultato per lavorare con man mano che continuano.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult e OriginalException  

Dopo avere eseguito un'operazione di Entity Framework imposta le proprietà di risultato e OriginalResult se l'esecuzione ha avuto esito positivo o le proprietà di eccezione e OriginalException se l'esecuzione non è riuscita con un'eccezione.  

La proprietà OriginalResult e OriginalException sono di sola lettura e vengono impostate solo da Entity Framework dopo effettivamente l'esecuzione di un'operazione. Queste proprietà non possono essere impostate dagli intercettori. Ciò significa che qualsiasi intercettore può distinguere un'eccezione o un risultato che è stata impostata da alcuni altri intercettore anziché l'eccezione reale o un risultato che si è verificato durante l'operazione è stata eseguita.  

### <a name="registering-interceptors"></a>La registrazione degli intercettori  

Dopo aver creata una classe che implementa uno o più delle interfacce di intercettazione può essere registrato con Entity Framework usando la classe DbInterception. Ad esempio:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Gli intercettori possono essere registrati a livello di dominio applicazione usando il meccanismo di configurazione basato sul codice DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Esempio: La registrazione NLog  

È possibile inserire tutte queste insieme in un esempio che l'utilizzo IDbCommandInterceptor e [NLog](http://nlog-project.org/) per:  

- Registra un avviso per qualsiasi comando che viene eseguito in modalità non asincrona  
- Registrare un errore per qualsiasi comando che genera un'eccezione quando eseguita  

Questa è la classe che esegue la registrazione, che deve essere registrata come illustrato in precedenza:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Si noti come questo codice Usa il contesto di intercettazione per rilevare quando un comando viene eseguito in modo non asincrono e per scoprire quando si è verificato un errore durante l'esecuzione di un comando.  
