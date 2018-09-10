---
title: Testabilità ed Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 2a2384c7868ae3cf6af4f915c06ae9fdb622634c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251323"
---
# <a name="testability-and-entity-framework-40"></a>Testabilità ed Entity Framework 4.0
Scott Allen

Data di pubblicazione: Maggio 2010

## <a name="introduction"></a>Introduzione

In questo white paper viene descritto e illustrato come scrivere codice non testabile con di ADO.NET Entity Framework 4.0 e Visual Studio 2010. In questo documento non tenta di concentrarsi su una metodologia di test specifica, ad esempio Progettazione basato su test (TDD) o di design basato su comportamenti (BDD). In questo documento verrà invece concentrarsi su come scrivere codice che utilizza ADO.NET Entity Framework, ma rimane semplice isolare e testare in modo automatico. Esamineremo i modelli di progettazione comuni che facilitano scenari di test nei dati di accesso e informazioni su come applicare questi modelli quando si usa il framework. Esamineremo anche le funzionalità specifiche del framework per vedere funzionamento di tali funzionalità nel codice non testabile.

## <a name="what-is-testable-code"></a>Che cos'è codice non testabile?

La possibilità di verificare un componente software con unit test automatizzati offre molti vantaggi auspicabili. Tutti gli utenti sa che i test positivi ridurrà il numero di difetti del software in un'applicazione e aumentare la qualità dell'applicazione, ma si verificano gli unit test posto va ben oltre appena individuando bug.

Un gruppo di test unità buona consente risparmiare tempo e rimanere in controllo del software che viene creato un team di sviluppo. Un team può apportare modifiche al codice esistente, eseguire il refactoring, la riprogettazione e la ristrutturazione software per soddisfare nuovi requisiti e aggiungere nuovi componenti in un'applicazione di tutto, sapendo il gruppo di test può verificare il comportamento dell'applicazione. Unit test facciano parte di un ciclo rapido feedback per facilitare la modifica e mantenere la manutenibilità man mano che aumenta la complessità del software.

Gli unit test include tuttavia un prezzo. Un team deve investire il tempo necessario per creare e gestire unit test. L'entità degli sforzi necessari per creare questi test è direttamente correlato per il **testabilità** del software sottostante. Per testare il software è semplice? Un team di progettazione del software con particolare attenzione alla testabilità creerà più team che lavorano con il software non verificabile velocemente test efficaci.

Microsoft ha progettato ADO.NET Entity Framework 4.0 (EF4) con particolare attenzione alla testabilità. Ciò non significa che gli sviluppatori scrivono gli unit test con codice del framework stesso. Al contrario, gli obiettivi di testabilità di EF4 rendono semplice creare codice verificabile che il framework si basa. Prima di esaminare esempi specifici, vale la pena conoscere le qualità del codice non testabile.

### <a name="the-qualities-of-testable-code"></a>Le qualità del codice non testabile

Codice semplice eseguire il test presenterà sempre almeno due tratti. In primo luogo, testabile codice è facile **osservare**. Dato un set di input, deve essere facile da osservare l'output del codice. Ad esempio, il seguente metodo di test è facile, poiché il metodo restituisce direttamente il risultato di un calcolo.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Un metodo di test è difficile se il metodo scrive il valore calcolato in un socket di rete, una tabella di database o un file, ad esempio il codice seguente. Il test deve eseguire operazioni aggiuntive per recuperare il valore.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

In secondo luogo, è facile da codice non testabile **isolare**. Utilizziamo lo pseudo-codice seguente come esempio di codice non testabile non valido.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

Il metodo è facile concludere, possiamo passare una polizza assicurativa e verificare il valore restituito corrisponde a un risultato previsto. Tuttavia, per testare il metodo è necessario disporre di un database installato con lo schema corretto e configurare il server SMTP, nel caso in cui il metodo tenta di inviare un messaggio di posta elettronica.

Lo unit test vuole solo verificare che la logica di calcolo all'interno del metodo, ma il test potrebbe non riuscire perché il server di posta elettronica è offline o perché il server di database spostato. Entrambi questi errori sono correlati al comportamento che è necessario verificare il test. È difficile isolare il comportamento.

Gli sviluppatori di software che si impegnano per scrivere codice verificabile spesso cercano di mantenere una separazione delle problematiche nel codice che scrivono. Il metodo sopra indicato deve concentrarsi su calcoli aziendali e delegare i dettagli di implementazione del database e alla posta elettronica ad altri componenti. Robert C. Martin viene chiamato il principio di singola responsabilità. Un oggetto deve incapsulare una responsabilità singola, narrow, come calcolare il valore di un criterio. Tutte le altre operazioni di database e la notifica devono essere la responsabilità di un altro oggetto. Il codice scritto in questo modo è più semplice isolare perché si occupa di una singola attività.

In .NET sono disponibili le astrazioni, dobbiamo seguire il principio di singola responsabilità e ottenere l'isolamento. È possibile usare le definizioni di interfaccia e forzare il codice per usare l'astrazione dell'interfaccia invece di un tipo concreto. Più avanti in questo articolo si vedrà come un metodo, ad esempio il cattivo esempio presentato in precedenza può funzionare con interfacce che dovranno *aspetto* come che comunicherà con il database. In fase di test, tuttavia, è possibile sostituire con un'implementazione fittizia non comunicare con il database, ma contiene invece i dati nella memoria. Questa implementazione fittizia verrà isolare il codice da problemi non correlati nel codice di accesso ai dati o nella configurazione del database.

Esistono vantaggi aggiuntivi all'isolamento. Il calcolo di business in quest'ultimo metodo dovrebbe richiedere solo pochi millisecondi per l'esecuzione, ma potrebbe essere eseguiti per diversi secondi come gli hop di codice in base alla rete e comunica con il test stesso per i vari server. Gli unit test devono eseguire fast per facilitare piccole modifiche. Gli unit test devono anche essere ripetibile e non restituisce alcun errore perché un componente non correlato al test presenta un problema. La scrittura di codice che è facile da osservare e isolare significa che gli sviluppatori disporranno di un'ora più facile scrivere test per il codice, dedicare meno tempo in attesa di test da eseguire e più importante, dedicare meno tempo tenere traccia dei bug che non esistono.

Si spera è possibile apprezzare i vantaggi di test e comprendere le qualità che presenta codice verificabile. Si sta tentando di risolvere come scrivere codice che funziona con EF4 per salvare i dati in un database, pur rimanendo observable e isolare facilmente, ma prima di tutto si sarà ridurre l'obiettivo per discutere le progettazioni testabili per accedere ai dati.

## <a name="design-patterns-for-data-persistence"></a>Schemi progettuali per la persistenza dei dati

Entrambi gli esempi non validi presentati in precedenza era troppe responsabilità. Nel primo esempio errato era necessario eseguire un calcolo *e* scrivere in un file. Nel secondo esempio errato era necessario leggere i dati da un database *e* eseguono un calcolo business *e* Invia messaggio di posta elettronica. Progettando i metodi più piccoli che separano le problematiche e delegare la responsabilità ad altri componenti verranno apportate notevoli progressi nella scrittura di codice non testabile. L'obiettivo è di creare funzionalità tramite la composizione di azioni da astrazioni più circoscritto e mirate.

Quando si tratta di persistenza dei dati il piccolo e mirate astrazioni ricerchiamo sono molto comuni sono stati documentati come modelli di progettazione. Libro di Fowler modelli di architettura delle applicazioni aziendali è stato il primo lavoro per descrivere questi modelli di stampa. Prima di illustrare come questi ADO.NET Entity Framework implementa e funziona con questi modelli, forniremo una breve descrizione di questi modelli nelle sezioni seguenti.

### <a name="the-repository-pattern"></a>Lo schema Repository

Fowler afferma un repository "funge da intermediario tra il dominio e i dati dei livelli di mapping tramite un'interfaccia simile a raccolta per l'accesso a oggetti di dominio". L'obiettivo di schema repository è per isolare il codice da una chiara la panoramica dell'accesso ai dati e come abbiamo visto isolamento precedenti è una caratteristica necessaria per la verificabilità.

La chiave per l'isolamento è come il repository espone tramite un'interfaccia simile a raccolta di oggetti. La logica che scrivere da non utilizzare nel repository avrà idea di come il repository verrà materializzare gli oggetti che richiesta. Il repository può comunicare con un database oppure potrebbe semplicemente restituire gli oggetti da una raccolta in memoria. Tutto il codice deve conoscere è il repository viene visualizzato per gestire la raccolta che è possibile recuperare, aggiungere ed eliminare oggetti dalla raccolta.

In applicazioni .NET esistenti in un repository concreto spesso eredita da un'interfaccia generica simile al seguente:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Si renderà alcune modifiche alla definizione dell'interfaccia quando è fornire un'implementazione per EF4, ma il concetto di base rimane invariato. Codice può usare un repository concreto che implementa questa interfaccia per recuperare un'entità dal valore della chiave primario, per recuperare una raccolta di entità in base alla valutazione di un predicato, o semplicemente recuperare tutte le entità disponibili. Il codice può anche aggiungere e rimuovere entità tramite l'interfaccia del repository.

Dato un oggetti IRepository del dipendente, codice può eseguire le operazioni seguenti.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Poiché il codice usa un'interfaccia (IRepository del dipendente), possiamo fornire il codice con diverse implementazioni dell'interfaccia. Un'implementazione potrebbe essere un'implementazione fornita con EF4 e oggetti persistenti in un database Microsoft SQL Server. Un'implementazione diversa (uno viene usato durante il test) potrebbe essere supportata da oggetti un elenco di dipendenti in memoria. L'interfaccia consente di ottenere l'isolamento del codice.

Si noti il IRepository&lt;T&gt; interfaccia non espone un'operazione di salvataggio. Come si aggiorna gli oggetti esistenti? Potrebbe riscontrare IRepository definizioni includono l'operazione di salvataggio e implementazioni di questi repository saranno necessario rendere subito persistente un oggetto nel database. Tuttavia, in molte applicazioni non vogliamo rendere persistenti gli oggetti singolarmente. Al contrario, vogliamo dare vita oggetti, ad esempio dal repository diversi, modificare tali oggetti come parte di un'attività di business e mentenere tutti gli oggetti come parte di una singola operazione atomica. Fortunatamente, è previsto un modello per consentire questo tipo di comportamento.

### <a name="the-unit-of-work-pattern"></a>La schema Unit of Work

Fowler afferma un'unità di lavoro "manterrà un elenco di oggetti interessati da una transazione di business e coordina la scrittura all'esterno delle modifiche e la risoluzione dei problemi di concorrenza". È responsabilità dell'unità di lavoro per rilevare le modifiche agli oggetti è dare vita da un repository e persistenti le modifiche sono state apportate agli oggetti quando viene informato che l'unità di lavoro per salvare le modifiche. È anche la responsabilità dell'unità di lavoro per rendere i nuovi oggetti è stato aggiunto a tutti i repository e inserire gli oggetti in un database e anche l'eliminazione di gestire.

Se si rivelerà un'operazione qualsiasi lavoro con i set di dati ADO.NET quindi già sarà familiarità con la schema unit of work. Set di dati ADO.NET aveva la possibilità di rilevare i nostri aggiornamenti, eliminazioni e l'inserimento di oggetti DataRow e potrebbe (con l'aiuto di un oggetto TableAdapter) risolvere le differenze tra tutte le modifiche a un database. Oggetti DataSet modelli, tuttavia, un subset disconnesso del database sottostante. La schema unit of work presenta lo stesso comportamento, ma funziona con oggetti business e gli oggetti di dominio che sono a conoscenza del database e isolati dal codice di accesso ai dati.

Un'astrazione per modellare l'unità di lavoro nel codice .NET potrebbe essere simile al seguente:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Esponendo i riferimenti di repository dall'unità di lavoro che è possibile assicurarsi una singola unità di lavoro oggetto ha la possibilità di tenere traccia di tutte le entità materializzate durante una transazione di business. L'implementazione del metodo di Commit per un'unità di lavoro reale è dove molto importante per riconciliare le modifiche in memoria con il database. 

Dato un riferimento a IUnitOfWork, codice possa apportare modifiche agli oggetti business recuperati da uno o più repository e salvare tutte le modifiche tramite l'operazione di Commit atomico.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Il modello di caricamento Lazy

Fowler utilizza il caricamento lazy di nome per descrivere "un oggetto che non contiene tutti i dati è necessario, ma in grado di eseguire questa operazione". Il caricamento lazy trasparente è una caratteristica importante avere quando la scrittura di codice testabile business e l'utilizzo di un database relazionale. Ad esempio, si consideri il codice seguente.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Modalità la raccolta di schede di popolamento? Esistono due possibili risposte. Una risposta è che il repository dipendente, quando viene richiesto di recuperare un dipendente, invia una query per recuperare sia il dipendente insieme a informazioni sulle schede associate del dipendente. Nei database relazionali questo in genere richiede una query con una clausola JOIN e può comportare il recupero di più informazioni rispetto a un'applicazione è necessario. Cosa accade se l'applicazione non deve mai la proprietà di schede di tocco?

Una risposta alla seconda consiste nel caricare la proprietà schede "su richiesta". Il caricamento lazy è implicita e trasparente per la logica di business perché il codice non richiama le API speciale per recuperare informazioni sulle schede. Il codice presuppone che le informazioni sulle schede è presente quando necessario. Prevede alcune magic con il caricamento lazy che implica in genere l'intercettazione di runtime delle chiamate al metodo. Il codice di intercettazione è responsabile della comunicazione con il database e il recupero di informazioni sulle schede lasciando la logica di business gratuita per la logica di business. Questo magic di caricamento lazy consente di isolare se stesso da operazioni di recupero dei dati e i risultati in codice non testabile più il codice di business.

Lo svantaggio di un caricamento lazy è che quando un'applicazione *viene* necessarie le informazioni di smart card ora il codice eseguirà un'altra query. Ciò non è un problema per molte applicazioni, ma per applicazioni sensibili alle prestazioni o le applicazioni a scorrimento in ciclo tra un numero di oggetti dipendenti e l'esecuzione di una query per recuperare le schede di tempo durante ogni iterazione del ciclo (un problema noto anche come N + 1 problema di query), il caricamento lazy è un trascinamento. In questi scenari un'applicazione potrebbe essere necessario caricare accuratamente le informazioni sulla scheda nel modo più efficiente possibile.

Fortunatamente, si vedrà come EF4 supporta entrambi carichi lazy implicite e carica impazientemente efficiente quando si spostano nella sezione successiva e implementazione di questi modelli.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementazione di modelli con Entity Framework

La buona notizia è che tutti i modelli di progettazione che è descritto nella sezione precedente sono semplici da implementare con EF4. Per illustrare che verrà usata una semplice applicazione di ASP.NET MVC per modificare e visualizzare i dipendenti e le informazioni sulle schede associate. Si inizierà con il seguente "plain old CLR Object" (poco). 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Queste definizioni di classe verranno modificato leggermente esamineremo diversi approcci e le funzionalità di EF4, ma si desidera mantenere queste classi come non riconoscono la persistenza (PI) come possibili. Un oggetto dispositivo PI all'oscuro *modo in cui*, o addirittura *se*, lo stato, contiene si trova all'interno di un database. PI e poco vanno di pari passo con il software testabile. Gli oggetti utilizzando un approccio POCO sono meno vincolata, più flessibile e più facilmente testabili perché possono operare senza un database presentano.

Con gli oggetti poco posto è possibile creare un modello EDM (Entity Data Model) in Visual Studio (vedere la figura 1). Non utilizzeremo il modello EDM per generare il codice per le nostre entità. Al contrario, si vuole usare le entità che si lovingly elaborare manualmente. Si userà solo il modello EDM per generare lo schema di database e fornire i metadati che ef4 deve eseguire il mapping di oggetti nel database.

![test_01 Entity Framework](~/ef6/media/eftest-01.jpg)

**Figura 1**

Nota: se si desidera sviluppare prima di tutto il modello EDM, è possibile generare pulito e il codice POCO da EDM. È possibile farlo con un'estensione di Visual Studio 2010 fornita dal team di programmabilità dei dati. Per scaricare l'estensione, avviare il gestore estensioni dal menu Strumenti in Visual Studio e cercare "POCO" (vedere la figura 2) la raccolta di modelli online. Sono disponibili diversi modelli di POCO per Entity Framework. Per altre informazioni sull'utilizzo del modello, vedere " [procedura dettagliata: modello POCO per Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![test_02 Entity Framework](~/ef6/media/eftest-02.png)

**Figura 2**

Da questo punto di partenza di POCO verranno discussi i due diversi approcci per codice non testabile. Il primo approccio viene chiamato l'approccio Entity Framework poiché sfrutta astrazioni dall'API Entity Framework per implementare le unità di lavoro e i repository. Nel secondo approccio verrà creato un'astrazioni dei repository personalizzato e quindi visualizzare i vantaggi e svantaggi di ogni approccio. Si inizierà esaminando l'approccio Entity Framework.  

### <a name="an-ef-centric-implementation"></a>Un'implementazione incentrato sugli elementi di Entity Framework

Si consideri l'azione del controller seguente da un progetto ASP.NET MVC. L'azione recupera un oggetto dipendente e restituisce un risultato per visualizzare una visualizzazione dettagliata del dipendente.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

È il codice da testare? Sono presenti almeno due test, che dobbiamo verificare il comportamento dell'azione. In primo luogo, vorremmo verificare che l'azione restituisce la visualizzazione corretta: un semplice test. È anche preferibile scrivere un test per verificare l'azione recupera il dipendente corretto e si vuole eseguire questa operazione senza l'esecuzione di codice per eseguire query sul database. Tenere presente che si desidera isolare codice sottoposto a test. Isolamento garantirà che il test non avere esito negativo a causa di un bug nel codice di accesso ai dati o nella configurazione del database. Se il test ha esito negativo, si saprà che abbiamo un bug nella logica del controller e non in qualche componente di sistema di livello inferiore.

Per ottenere l'isolamento che è necessario alcune astrazioni, come le interfacce abbiamo presentati in precedenza per i repository e unità di lavoro. Tenere presente che lo schema repository è progettato per eseguire la mediazione tra gli oggetti di dominio e il livello di mapping dei dati. In questo scenario EF4 *viene* i dati eseguendo il mapping di livello e già fornisce un'astrazione simile a repository denominata IObjectSet&lt;T&gt; (dallo spazio dei nomi System.Data.Objects). La definizione dell'interfaccia è simile al seguente.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt; soddisfi i requisiti per un repository perché è simile a una raccolta di oggetti (tramite IEnumerable&lt;T&gt;) e fornisce metodi per aggiungere e rimuovere gli oggetti dalla raccolta simulata. I metodi di Attach e Detach espongono funzionalità aggiuntive dell'API EF4. Usare IObjectSet&lt;T&gt; come interfaccia per i repository è necessaria un'unità di lavoro astrazione per associare tra loro i repository.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Un'implementazione concreta di questa interfaccia comunicherà con SQL Server ed è facile creare utilizzando la classe ObjectContext EF4. La classe ObjectContext è l'unità di lavoro nell'API EF4 reale.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Portare un IObjectSet&lt;T&gt; vita consiste semplicemente nella chiamata del metodo CreateObjectSet dell'oggetto ObjectContext. Dietro le quinte il framework userà i metadati è disponibile nel modello EDM per produrre un concreto ObjectSet&lt;T&gt;. Vengono illustrati i criteri con restituzione di IObjectSet&lt;T&gt; interfaccia perché aiuterà a mantenere la testabilità nel codice client.

Questa implementazione concreta è utile nell'ambiente di produzione, ma è necessario concentrare l'attenzione sul modo in cui si userà l'astrazione IUnitOfWork per facilitare il test.

### <a name="the-test-doubles"></a>Le copie di Test

Per isolare l'azione del controller è necessario passare tra le reali unità di lavoro (supportato da un oggetto ObjectContext) e un'unità di double o "fittizio" test di lavoro (eseguendo operazioni in memoria). L'approccio comune per eseguire questo tipo di trasferimento è per non consentire il controller MVC crea un'istanza di un'unità di lavoro, ma passare all'unità di lavoro nel controller come parametro costruttore.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Il codice riportato sopra è riportato un esempio di inserimento delle dipendenze. Il controller crea relativa dipendenza (l'unità di lavoro), ma inserisce la dipendenza nel controller non è consentita. In un progetto MVC accade spesso di usare una factory del controller personalizzato in combinazione con un'inversione del contenitore del controllo (IoC) per automatizzare l'inserimento di dipendenze. Questi argomenti sono esula dall'ambito di questo articolo, ma è possibile leggere più da seguendo i riferimenti alla fine di questo articolo.

Un'unità fittizio di implementazione di lavoro che è possibile usare per il test potrebbe essere simile al seguente.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Si noti che l'unità di lavoro fittizio espone una proprietà eseguito il commit. È talvolta utile aggiungere funzionalità a una classe fittizio che facilitano il test. In questo caso è facile concludere se codice esegue il commit di un'unità di lavoro selezionando il commit di una proprietà.

Sono necessari anche un IObjectSet fittizio&lt;T&gt; di conservare gli oggetti dipendenti e dipendente in memoria. Possiamo fornire un'implementazione singola usano i generics.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Questo test double delega la maggior parte del lavoro a un HashSet sottostante&lt;T&gt; oggetto. Si noti che IObjectSet&lt;T&gt; richiede un vincolo generico applicando T come una classe (un tipo riferimento) e inoltre forzata l'implementazione di IQueryable&lt;T&gt;. È facile visualizzare una raccolta in memoria come un'interfaccia IQueryable&lt;T&gt; utilizzando l'operatore LINQ standard AsQueryable.

### <a name="the-tests"></a>I test

Unit test tradizionali userà una singola classe di test per contenere tutti i test per tutte le azioni in un singolo controller MVC. È possibile scrivere questi test, o qualsiasi tipo di unit test, usando in memoria fakes è stata compilata. Tuttavia, per questo articolo che si eviterà monolitico contrapposto approccio della classe di test e invece raggruppare i test per concentrarsi su una parte specifica della funzionalità.  Ad esempio, "Crea nuovo dipendente" potrebbe essere la funzionalità di cui che si vuole testare, quindi si userà una singola classe di test per verificare l'azione del controller unico responsabile per la creazione di un nuovo dipendente.

È disponibile del codice il programma di installazione comuni che necessaria per tutte queste classi di test granulare. Ad esempio, è sempre necessario creare i repository in memoria e fittizio unità di lavoro. È necessario anche un'istanza del controller dipendente con l'unità di lavoro inserito fittizio. Abbiamo potrai condividere questo codice di programma di installazione comuni tra le classi di test usando una classe di base.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

Il "madre oggetto" viene usato nella classe di base è un modello comune per la creazione di dati di test. Madre un oggetto contiene i metodi factory per creare un'istanza di entità di test da utilizzare in più strumenti di test.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

È possibile usare il EmployeeControllerTestBase come classe base per un numero di Fixture di test (vedere la figura 3). Ogni fixture di test verrà testata una specifica azione del controller. Ad esempio, una fixture di test sarà incentrata sul test l'azione di creazione usata durante una richiesta HTTP GET (per visualizzare la visualizzazione per la creazione di un dipendente) e una fixture diversi sarà incentrata sull'azione di creazione usato in una richiesta HTTP POST (per richiedere informazioni trasmesse dal utente di creare un dipendente). Ogni classe derivata solo è responsabile per la configurazione necessaria nel relativo contesto specifico e per fornire le asserzioni necessarie per verificare i risultati per il contesto di test specifico.

![test_03 Entity Framework](~/ef6/media/eftest-03.png)

**Figura 3**

Lo stile di test e convenzione di denominazione presentato in questo articolo non è necessario per codice non testabile: si tratta di un solo approccio. Figura 4 mostra i test in esecuzione in Resharper il cervello Jet test runner plug-in per Visual Studio 2010.

![test_04 Entity Framework](~/ef6/media/eftest-04.png)

**Figura 4**

Con una classe base per gestire il codice di installazione condivisa, gli unit test per ogni azione del controller sono ridotte e facile da scrivere. I test verranno eseguito rapidamente (Poiché stiamo eseguendo operazioni in memoria) e non devono avere esito negativo a causa non correlati dell'infrastruttura o preoccupazioni ambientali (perché è stato isolato dell'unità sottoposta a test).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

In questi test, la classe base svolge la maggior parte delle operazioni di configurazione. Tenere presente che il costruttore di classe di base viene creato il repository in memoria, un'unità di lavoro fittizio e un'istanza della classe EmployeeController. La classe di test deriva da questa classe di base e si concentra sulle specifiche del metodo di creazione di test. In questo caso le specifiche riducono alla procedura "Disponi, agire e di asserzione" verrà visualizzato in qualsiasi unit test di procedure:

-   Creare un oggetto Nuovo_dipendente per simulare i dati in ingresso.
-   Richiamare l'azione di creazione del EmployeeController e passare il Nuovo_dipendente.
-   Verificare che l'azione di creazione produce i risultati previsti (il dipendente viene visualizzato nel repository).

Che cosa abbiamo creato consente di testare qualsiasi azione EmployeeController. Ad esempio, quando si scrivono test per l'azione Index del controller dipendente è possibile ereditare dalla classe di base di test per stabilire la stessa configurazione di base per i test. Anche in questo caso la classe di base verrà creato il repository in memoria, l'unità di lavoro fittizio e un'istanza di EmployeeController. I test per l'azione di indice necessario concentrarsi solo su come richiamare l'operazione sull'indice e restituisce la qualità del modello di azione di test.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

I test che stiamo creando con fakes in memoria sono orientati ai test il *stato* del software. Ad esempio, quando si desidera controllare lo stato del repository dopo che viene eseguita l'azione di creazione, l'azione di creazione di test repository contenere il nuovo dipendente?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

In un secondo momento vedremo interazione basato su test. Test di interazione basati su chiederà se il codice sottoposto a test richiamati i metodi appropriati dei nostri oggetti e passati i parametri corretti. Per il momento si passerà il coperchio un altro modello di progettazione: il caricamento lazy.

## <a name="eager-loading-and-lazy-loading"></a>Il caricamento eager e il caricamento Lazy

In un determinato web ASP.NET MVC dell'applicazione che potrebbe essere piacerebbe per visualizzare le informazioni del dipendente e includere il dipendente associato schede. Ad esempio, potremmo avere una visualizzazione di riepilogo da schede che mostra il nome del dipendente e il numero totale di schede nel sistema. Esistono diversi approcci che è possibile eseguire per implementare questa funzionalità.

### <a name="projection"></a>Proiezione

Un approccio semplice per creare il riepilogo consiste nel creare un modello dedicato alle informazioni di cui che si vuole visualizzare nella visualizzazione. In questo scenario il modello potrebbe essere simile al seguente.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Si noti che il EmployeeSummaryViewModel non è un'entità – in altre parole non che si vogliono rendere persistenti nel database. Si intende solo utilizzare questa classe in modo casuale i dati nella visualizzazione in modo fortemente tipizzato. Il modello di visualizzazione è simile a un trasferimento dati (DTO) oggetto perché non contiene alcun comportamento (nessun metodi) – solo le proprietà. Le proprietà saranno contenuti i dati, occorre spostare. È facile creare un'istanza di questo modello di visualizzazione usando l'operatore di proiezione standard di LINQ: l'operatore Select.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Esistono due importanti funzionalità per il codice sopra riportato. Prima di tutto, il codice è facile eseguire il test perché è ancora facile osservare e isolare. L'operatore Select funziona altrettanto bene con i nostri fakes in memoria a quanto accade con l'unità di lavoro reale.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

La seconda funzionalità rilevanti è come il codice consente EF4 generare una query singola ed efficiente per assemblare dipendente e schede informazioni. È stato caricato informazioni sui dipendenti e le informazioni sulla scheda in allo stesso oggetto senza usare alcuna API speciali. Il codice espresso semplicemente le informazioni necessarie tramite operatori LINQ standard che funzionano con origini dati in memoria, nonché a origini dati remote. È stato in grado di convertire gli alberi delle espressioni create dalle query LINQ e C EF4\# compilatore in una query T-SQL unica ed efficiente.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Sono disponibili in altri casi quando non si desidera lavorare con un modello di visualizzazione o di un oggetto DTO, ma con le entità reali. Quando si conosce abbiamo bisogno di un dipendente *e* schede del dipendente, è possibile caricare rapidamente i dati correlati in modo efficiente e non intrusivo.

### <a name="explicit-eager-loading"></a>Caricamento Eager esplicito

Quando si vuole caricare rapidamente le informazioni sulle entità correlate occorre un meccanismo per la logica di business (o in questo scenario, logica di azione del controller) per esprimere il desiderio di repository. L'oggetto ObjectQuery EF4&lt;T&gt; classe definisce un metodo Include per specificare gli oggetti correlati da recuperare durante una query. Ricordare l'ObjectContext EF4 espone entità tramite ObjectSet concreta&lt;T&gt; classe che eredita da ObjectQuery&lt;T&gt;.  Se si stesse usando ObjectSet&lt;T&gt; riferimenti nel nostro azione del controller è stato possibile scrivere il codice seguente per specificare un caricamento eager delle informazioni sulle schede per ogni dipendente.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Tuttavia, poiché Stiamo tentando di mantenere il nostro codice verificabile è sta esponendo non ObjectSet&lt;T&gt; all'esterno dell'unità reali della classe di lavoro. Al contrario, ci si affida la IObjectSet&lt;T&gt; interfaccia che è più facile da simulare, ma IObjectSet&lt;T&gt; non definisce un metodo di inclusione. La bellezza di LINQ è che possiamo creare un operatore di inclusione.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Si noti che questo operatore inclusione è definito come metodo di estensione per interfaccia IQueryable&lt;T&gt; anziché IObjectSet&lt;T&gt;. Ciò offre la possibilità di usare il metodo con una vasta gamma di tipi possibili, tra cui IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;e ObjectSet&lt;T&gt;. Nel caso in cui la sequenza sottostante non è un oggetto ObjectQuery EF4 autentico&lt;T&gt;, non esiste alcun modo e l'operatore di inclusione è no-op. Se l'oggetto sottostante di sequenza *viene* un ObjectQuery&lt;T&gt; (o derivato da ObjectQuery&lt;T&gt;), quindi EF4 vedrà il requisito per i dati aggiuntivi e formulare SQL corretto query.

Con questo nuovo operatore posto è possibile richiedere in modo esplicito un caricamento eager delle informazioni sulle schede dal repository.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Se viene eseguita in un ObjectContext reale, il codice genera la seguente query singola. La query raccoglie le informazioni necessarie dal database in un unico round trip per materializzare gli oggetti dipendenti e completamente popolare le proprietà delle schede.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

La buona notizia è che resta testabile completamente, il codice all'interno del metodo di azione. Non è necessario fornire le funzionalità aggiuntive per i nostri fakes supportare l'operatore di inclusione. Quelle cattive sono che abbiamo dovuto utilizzare l'operatore di inclusione all'interno di codice che si desiderava mantenere che non riconoscono la persistenza. Questo è un ottimo esempio del tipo di compromessi da valutare durante la compilazione di codice non testabile. Esistono casi è necessario per consentire la persistenza delle perdite di problematiche di fuori l'astrazione di repository per soddisfare gli obiettivi di prestazioni.

L'alternativa per il caricamento eager è il caricamento lazy. Indica di caricamento lazy facciamo *non* necessario il codice di business per annunciare in modo esplicito il requisito per i dati associati. Invece, utilizziamo le nostre entità nell'applicazione e se i dati aggiuntivi sono necessari Entity Framework verranno caricati i dati su richiesta.

### <a name="lazy-loading"></a>Caricamento lazy

È facile immaginare uno scenario in cui non si sa cosa saranno necessario dati in un componente della logica di business. Potrebbe essere sappiamo che la logica richiede un oggetto dipendente, ma si può creare rami in diversi percorsi di esecuzione in cui alcuni di questi percorsi richiedono informazioni sulle schede del dipendente e alcune non lo sono. Scenari simili sono perfetti per implicita il caricamento lazy poiché i dati vengono visualizzati magicamente in base alle esigenze.

Il caricamento lazy, noto anche come posticipato il caricamento, posizionare alcuni requisiti dei nostri oggetti entità. Oggetti poco con mancato riconoscimento della persistenza true non dovrebbe affrontare tutti i requisiti dal livello di persistenza, ma è praticamente impossibile raggiungere mancato riconoscimento della persistenza true.  In alternativa viene misurata mancato riconoscimento della persistenza in gradi relativi. Sarebbe sfortunato se fosse necessario ereditare una classe di base di persistenza orientata ai servizi o usare un insieme specifico per ottenere il caricamento lazy di oggetti poco. Fortunatamente, EF4 è una soluzione meno intrusiva.

### <a name="virtually-undetectable"></a>Rilevabili

Quando si usano gli oggetti POCO, EF4 può generare in modo dinamico i proxy di runtime per le entità. Questi proxy in modo invisibile il wrapping di oggetti poco materializzati e forniscono servizi aggiuntivi mediante l'intercettazione di ogni proprietà ottenere e impostare operazione da eseguire operazioni aggiuntive. Uno di questi servizi è la funzionalità di caricamento lazy che stiamo cercando. Un altro servizio è una modifica efficiente meccanismo che consentono di registrare quando i valori delle proprietà di un'entità viene modificato il programma di rilevamento. L'elenco delle modifiche viene utilizzata da ObjectContext durante il metodo SaveChanges per rendere persistente qualsiasi entità modificate usando i comandi di aggiornamento.

Per questi proxy a funzionare, tuttavia, hanno bisogno di hook in routine property get in modo e set di operazioni su un'entità e i proxy raggiungere questo obiettivo eseguendo l'override di membri virtuali. Di conseguenza, se si vuole che il caricamento lazy implicito e il rilevamento delle modifiche efficienti è necessario tornare indietro ai nostri le definizioni delle classi POCO e contrassegnare le proprietà come virtuale.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

È comunque possibile affermare che l'entità dipendente è principalmente non riconoscono la a livello di persistenza. L'unico requisito consiste nell'usare i membri virtuali e non influisce la testabilità del codice. Non è necessario derivare da una speciale classe di base, o addirittura usare una raccolta speciale dedicata per il caricamento lazy. Come illustrato nel codice, qualsiasi classe che implementa ICollection&lt;T&gt; è disponibile per includere le entità correlate.

È inoltre disponibile una modifica secondaria, che è necessario apportare all'interno di nostra unità di lavoro. Il caricamento lazy costituisce *disattivata* per impostazione predefinita quando si lavora direttamente con un oggetto ObjectContext. È disponibile una proprietà che è possibile impostare sulla proprietà ContextOptions per abilitare il caricamento posticipato ed è possibile impostare questa proprietà all'interno di nostra unità di lavoro reale Se si vuole abilitare ovunque il caricamento lazy.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Con l'abilitazione del caricamento lazy implicito, il codice dell'applicazione può usare un dipendente e del dipendente associati schede tempo pur rimanendo inconsapevoli del lavoro richiesto per Entity Framework caricare i dati aggiuntivi.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Il caricamento lazy rende più semplice scrivere il codice dell'applicazione e con il magic proxy resta testabile completamente, il codice. Fakes in memoria dell'unità di lavoro può essere precaricata semplicemente fittizio entità con dati associati secondo necessità durante un test.

A questo punto è possibile concentrare l'attenzione dalla creazione di repository con IObjectSet&lt;T&gt; ed esaminare le astrazioni per nascondere tutti i segni di framework di persistenza.

## <a name="custom-repositories"></a>Repository personalizzati

Quando l'unità di lavoro schema progettuale è prima di tutto presentati in questo articolo che viene fornito un codice di esempio per ciò che potrebbe essere simile all'unità di lavoro. Verrà ora nuovamente presentare questa idea originale usando il dipendente e lo scenario da schede dipendente che questo abbiamo lavorato con.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

La differenza principale tra questa unità di lavoro e l'unità di lavoro è stato creato nell'ultima sezione è come questa unità di lavoro non usa alcun astrazioni da framework EF4 (non vi è alcun IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; funziona bene come un'interfaccia del repository, ma l'API espone può non allineare perfettamente alle esigenze dell'applicazione. In questo approccio imminente rappresentiamo repository tramite un IRepository personalizzato&lt;T&gt; astrazione.

Molti sviluppatori che seguono test-driven design, progettazione basata sul comportamento e la progettazione di metodologie basata su domini preferiscono la IRepository&lt;T&gt; approccio per diversi motivi. Primo, il IRepository&lt;T&gt; interfaccia rappresenta un livello "anti-danneggiamento". Come descritto da Eric Evans nel suo libro Domain Driven Design un livello antidanneggiamento mantiene il codice di dominio dall'infrastruttura API, ad esempio un'API di persistenza. In secondo luogo, gli sviluppatori possono compilare i metodi nel repository che soddisfano le esigenze specifiche di un'applicazione (come individuato durante la scrittura di test). Ad esempio, potrebbe essere necessario spesso a individuare una singola entità tramite un valore di ID, in modo che è possibile aggiungere un metodo FindById per l'interfaccia del repository.  Nostro IRepository&lt;T&gt; definizione avrà un aspetto simile al seguente.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Si noti che verrà eliminato nuovamente all'uso di un'interfaccia IQueryable&lt;T&gt; interfaccia da esporre le raccolte di entità. Oggetto IQueryable&lt;T&gt; consente gli alberi delle espressioni LINQ al flusso in un provider EF4 e fornire una visione olistica del query il provider. Una seconda opzione, è possibile restituire IEnumerable&lt;T&gt;, ovvero il provider LINQ EF4 visualizzerà solo le espressioni compilate all'interno del repository. Qualsiasi raggruppamento, ordinamento e proiezione effettuata all'esterno del repository non comporranno il comando SQL inviato al database, che può influire negativamente sulle prestazioni. D'altra parte, un repository restituisca solo IEnumerable&lt;T&gt; risultati non verranno mai strano, ma con un nuovo comando SQL. Funzioneranno entrambi gli approcci e rimangono testabili entrambi gli approcci.

Si tratta di semplice fornire una singola implementazione del IRepository&lt;T&gt; interfaccia usando l'API di ObjectContext EF4 e generics.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

Il IRepository&lt;T&gt; approccio offre un maggiore controllo sulle query poiché dispone di un client per richiamare un metodo per ottenere un'entità. All'interno del metodo potremmo offriamo controlli aggiuntivi e gli operatori LINQ per applicare i vincoli dell'applicazione. Si noti che l'interfaccia dispone di due vincoli sul parametro di tipo generico. Il primo vincolo è l'aspetto degli svantaggi classe richiesto da ObjectSet&lt;T&gt;, e il secondo vincolo forza le nostre entità implementare IEntity – un'astrazione creata per l'applicazione. L'interfaccia IEntity impone le entità abbiano una proprietà Id leggibile e quindi è possibile usare questa proprietà nel metodo FindById. IEntity viene definita con il codice seguente.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity può essere considerato una violazione di piccole dimensioni del mancato riconoscimento della persistenza poiché le nostre entità necessarie per implementare questa interfaccia. Tenere presente mancato riconoscimento della persistenza riguarda i compromessi e per molte funzionalità FindById ottenuti superino il vincolo imposto dall'interfaccia. L'interfaccia non ha alcun impatto su testabilità.

Creare un'istanza di un live IRepository&lt;T&gt; richiede un oggetto ObjectContext EF4, in modo che un'unità concreta dell'implementazione di lavoro deve gestire la creazione di istanze.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>Usare il Repository personalizzato

Usando il repository personalizzato non è significativamente diversa dall'uso del repository basato su IObjectSet&lt;T&gt;. Anziché applicare operatori LINQ direttamente a una proprietà, è necessario innanzitutto uno richiamare i metodi del repository per recuperare un oggetto IQueryable&lt;T&gt; riferimento.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Si noti che l'operatore di inclusione personalizzato che viene implementati in precedenza funzionerà senza alcuna modifica. Metodo FindById del repository rimuove i duplicati per la logica da azioni sta tentando di recuperare una singola entità.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Non c'è alcuna differenza significativo in fase di testabilità di aver esaminato i due approcci. È stato possibile fornire implementazioni fittizie di IRepository&lt;T&gt; creando classi concrete supportate da HashSet&lt;dipendente&gt; : analogamente a ciò che abbiamo fatto nell'ultima sezione. Tuttavia, alcuni sviluppatori preferiscono usare oggetti fittizi e simulare il framework di oggetti invece di compilare fakes. Esamineremo utilizzando oggetti simulati per testare l'implementazione e illustra le differenze tra gli oggetti fittizi e fake nella sezione successiva.

### <a name="testing-with-mocks"></a>Simulazioni e test

Esistono diversi approcci per la creazione di un "test double" quali chiamate di Martin Fowler. Un test double (ad esempio, un stunt film double) è un oggetto compilato per "in evidenza" per il real, gli oggetti di produzione durante i test. I repository in memoria che è stata creata sono copie di test per i repository che comunicano con SQL Server. Abbiamo visto come utilizzare queste copie di test durante gli unit test per isolare il codice e mantenere i test che vengono eseguiti rapidamente.

Le copie di test che è stata compilata dispongono di implementazioni reali, l'utilizzo. Dietro le quinte ciascuno di essi archivia una raccolta di oggetti concreta e verranno aggiungere e rimuovere gli oggetti da questa raccolta come manipolare il repository durante un test. Alcuni sviluppatori preferiscono creare le copie di test in questo modo, con le implementazioni di lavoro e codice reale.  Queste copie di test sono quelle che chiamiamo *fakes*. Dispongono di implementazioni di lavoro, ma non sono sufficientemente reali per la produzione. Il repository fittizio non essendo possibile scrivere nel database. Il server SMTP fittizio non effettivamente invia un messaggio di posta elettronica sulla rete.

### <a name="mocks-versus-fakes"></a>Oggetti fittizi e Fakes

Esiste anche un altro tipo di test double noto come un *simulare*. Mentre fakes dispongono di implementazioni di lavoro, gli oggetti fittizi sono dotate di alcuna implementazione. Con l'aiuto di un framework di oggetti fittizi è costruire questi oggetti fittizi in fase di esecuzione e usarli come copie di test. In questa sezione verrà usato il comportamento fittizio Moq framework open source. Ecco un semplice esempio dell'uso Moq per creare dinamicamente un test double per un repository dipendente.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Verranno chiesti Moq un IRepository&lt;dipendente&gt; implementazione e viene creato uno in modo dinamico. È possibile ottenere per l'oggetto che implementa IRepository&lt;dipendente&gt; accedendo alle proprietà dell'oggetto della simulazione&lt;T&gt; oggetto. Si tratta di questo oggetto interno che è possibile passare al nostro controller e non sapranno se si tratta di una copia di test o il repository reale. È possibile richiamare i metodi sull'oggetto esattamente come si potrebbero richiamare metodi su un oggetto con un'implementazione reale.

È necessario chiedersi cosa il repository fittizio farà quando si richiama il metodo Add. Poiché non esiste alcuna implementazione sottostante l'oggetto fittizio, Add non esegue alcuna operazione. Non è Nessuna raccolta concreti dietro le quinte come avevamo la fakes che è stato scritto, in modo che il dipendente viene eliminato. Per quanto riguarda il valore restituito di FindById? In questo caso l'oggetto fittizio esegue l'unica cosa che può eseguire, viene restituita un valore predefinito. Poiché si sta restituendo un tipo riferimento (un dipendente), il valore restituito è un valore null.

Gli oggetti fittizi potrebbero sembrare inutili; Tuttavia, esistono due altre funzionalità di simulazioni su che non abbiamo parlato. In primo luogo, il framework Moq registra tutte le chiamate effettuate per l'oggetto fittizio. In un secondo momento nel codice è possibile porre Moq se chiunque richiamato il metodo Add, o se tutti gli utenti ha richiamato il metodo FindById. Si vedrà più avanti come è possibile usare questa funzionalità di registrazione "black box" nei test.

La seconda funzionalità ideale è che modo utilizzare Moq programmare un oggetto fittizio con *aspettative*. Un'aspettativa indica l'oggetto fittizio come rispondere a qualsiasi interazione specificato. Ad esempio, è possibile programmare un'aspettativa nel nostro simulazione e ordinargli di restituire un oggetto dipendente, quando un utente richiama FindById. Il framework Moq utilizza un'API di installazione e le espressioni lambda per queste aspettative del programma.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

In questo esempio chiediamo Moq creare dinamicamente un repository e abbiamo programmare il repository con una previsione. L'aspettativa indica l'oggetto fittizio per restituire un nuovo oggetto dipendente con Id uguale a 5, quando un utente richiama il metodo FindById passando un valore pari a 5. Questo test venga superato e non è stato necessario creare un'implementazione completa per IRepository fittizio&lt;T&gt;.

È possibile rivedere i test che è stata scritta in precedenza e modifiche in modo che usino le simulazioni anziché fakes. Esattamente come prima, si userà una classe di base per configurare i componenti comuni dell'infrastruttura che necessaria per tutti i test del controller.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

Il codice di installazione rimane prevalentemente invariato. Invece di usare fakes, useremo Moq per costruire oggetti fittizi. La classe di base predispone l'unità di lavoro fittizio restituire un repository fittizio quando codice richiama le proprietà dipendenti. Il resto dell'installazione fittizio avrà luogo all'interno di Fixture di test dedicati per ogni scenario specifico. Ad esempio, la fixture di test per l'azione Index configurerà il repository fittizio per restituire un elenco di dipendenti quando l'azione richiama il metodo FindAll del repository fittizio.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Tranne le aspettative, i nostri test hanno un aspetto simile ai test creato in precedenza. Tuttavia, con la possibilità di registrazione di un framework di simulazione è possiamo approccio test da un angolo diverso. Esamineremo questa nuova prospettiva nella sezione successiva.

### <a name="state-versus-interaction-testing"></a>Stato e l'interazione di test

Esistono diverse tecniche utilizzabili per testare il software con oggetti fittizi. Uno degli approcci è usare lo stato basato su test, che è quanto è stato fatto in questo articolo finora. Stato basato su test rende asserzioni sullo stato del software. Nell'ultima dei test è richiamato un metodo di azione nel controller ed effettuata un'asserzione relativi al modello che la compilazione dovrebbe. Ecco alcuni altri esempi dello stato di test:

-   Verificare che il repository contiene il nuovo oggetto dipendente dopo l'esecuzione di Create.
-   Verificare che il modello contiene un elenco di tutti i dipendenti dopo l'esecuzione di indice.
-   Verificare che il repository non contenga un dipendente specificato dopo l'esecuzione di eliminazione.

Verrà visualizzato con oggetti fittizi un altro approccio consiste nel verificare *interazioni*. Mentre lo stato basato su test rende asserzioni sullo stato degli oggetti, l'interazione basato su test rende asserzioni, sull'interagiscano tra oggetti. Ad esempio:

-   Verificare che il controller Richiama metodo Add del repository quando si esegue Create.
-   Verificare che il controller Richiama metodo FindAll del repository durante l'esecuzione dell'indice.
-   Verificare che il controller richiama l'unità di metodo di Commit del lavoro per salvare le modifiche apportate durante l'esecuzione di modifica.

Interazione test spesso richiede meno dati di test, perché è non siano in grado di all'interno di raccolte e verificare i conteggi. Ad esempio, se si conosce che l'azione Details Richiama metodo FindById del repository con il valore corretto, quindi l'azione è probabilmente il corretto comportamento. È possibile verificare questo comportamento senza dover configurare tutti i dati di test da restituire dalla FindById.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

Il programma di installazione solo richiesto nella fixture di test precedente è il programma di installazione fornito dalla classe di base. Quando si richiama l'azione del controller, Moq registrerà le interazioni con il repository fittizio. Usando l'API di Moq verificare, possiamo porre Moq se il controller di richiamata FindById con il valore dell'ID corretto. Se il controller non ha richiamato il metodo o ha richiamato il metodo con un valore di parametro imprevisto, il metodo di verifica genera un'eccezione e il test avrà esito negativo.

Ecco un altro esempio per verificare che l'azione di creazione richiama Commit in unità di lavoro corrente.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Un aspetto con il testing di interazione è la tendenza per specificare le interazioni tramite. La capacità dell'oggetto fittizio per registrare e verificare che ogni interazione con l'oggetto simulato non significa che il test è consigliabile provare a verificare ogni interazione. Alcune interazioni sono dettagli di implementazione ed è consigliabile verificare solo le interazioni *obbligatorio* per soddisfare il test corrente.

La scelta tra oggetti fittizi o simulazioni dipende in gran parte del sistema si esegue il test e il personale (o team) delle preferenze. Gli oggetti fittizi possono ridurre drasticamente la quantità di codice che è necessario implementare le copie di test, ma non tutti gli utenti è comodo le aspettative di programmazione e la verifica delle interazioni.

## <a name="conclusions"></a>Conclusioni

In questo white paper sono stati illustrati diversi approcci per la creazione di codice non testabile quando si usa ADO.NET Entity Framework per la persistenza dei dati. È possibile sfruttare astrazioni incorporate, ad esempio IObjectSet&lt;T&gt;, oppure creare proprie astrazioni come IRepository&lt;T&gt;.  In entrambi i casi, il supporto POCO in ADO.NET Entity Framework 4.0 consente ai consumer di queste astrazioni di rimanere permanente che non riconoscono la e altamente testabile. Funzionalità EF4 aggiuntive, ad esempio il caricamento lazy implicito consente al servizio business e dell'applicazione del codice senza preoccuparsi dei dettagli di un archivio dati relazionale. Infine, le astrazioni che creiamo sono facili da fittizio o fittizio all'interno di unit test e possiamo usare queste copie di test per ottenere tempi di esecuzione veloce, elevata isolata e i test.

### <a name="additional-resources"></a>Risorse aggiuntive

-   Robert C. Martin, " [il principio di singola responsabilità](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler [catalogo dei modelli](http://www.martinfowler.com/eaaCatalog/index.html) da *modelli di architettura dell'applicazione Enterprise*
-   Caprio Griffin, " [inserimento delle dipendenze](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog di programmabilità dei dati, " [procedura dettagliata: sviluppo con Entity Framework 4.0 basato su Test](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Blog di programmabilità dei dati, " [modelli con Repository e unità di lavoro con Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels, " [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen, " [Introduzione a macchine specifiche](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [BDD con MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler, " [fittizi e stub](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler, " [Test Double](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, " [lo stato rispetto a test l'interazione](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Biografia

Scott Allen è un membro del personale tecnico in Pluralsight e fondatore di OdeToCode.com. In 15 anni di sviluppo del software commerciale, Scott ha lavorato su soluzioni per tutti i dati da dispositivi con embedded 8 bit per le applicazioni web ASP.NET altamente scalabile. È possibile contattare Scott sul suo blog all'indirizzo OdeToCode o su Twitter all'indirizzo [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).
