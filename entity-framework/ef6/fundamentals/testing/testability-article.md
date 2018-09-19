---
title: Testabilità ed Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: aec177438004fd255bef85a5e5047cf6b5a6f782
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284044"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="42a77-102">Testabilità ed Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="42a77-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="42a77-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="42a77-103">Scott Allen</span></span>

<span data-ttu-id="42a77-104">Data di pubblicazione: Maggio 2010</span><span class="sxs-lookup"><span data-stu-id="42a77-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="42a77-105">Introduzione</span><span class="sxs-lookup"><span data-stu-id="42a77-105">Introduction</span></span>

<span data-ttu-id="42a77-106">In questo white paper viene descritto e illustrato come scrivere codice non testabile con di ADO.NET Entity Framework 4.0 e Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="42a77-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="42a77-107">In questo documento non tenta di concentrarsi su una metodologia di test specifica, ad esempio Progettazione basato su test (TDD) o di design basato su comportamenti (BDD).</span><span class="sxs-lookup"><span data-stu-id="42a77-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="42a77-108">In questo documento verrà invece concentrarsi su come scrivere codice che utilizza ADO.NET Entity Framework, ma rimane semplice isolare e testare in modo automatico.</span><span class="sxs-lookup"><span data-stu-id="42a77-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="42a77-109">Esamineremo i modelli di progettazione comuni che facilitano scenari di test nei dati di accesso e informazioni su come applicare questi modelli quando si usa il framework.</span><span class="sxs-lookup"><span data-stu-id="42a77-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="42a77-110">Esamineremo anche le funzionalità specifiche del framework per vedere funzionamento di tali funzionalità nel codice non testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="42a77-111">Che cos'è codice non testabile?</span><span class="sxs-lookup"><span data-stu-id="42a77-111">What Is Testable Code?</span></span>

<span data-ttu-id="42a77-112">La possibilità di verificare un componente software con unit test automatizzati offre molti vantaggi auspicabili.</span><span class="sxs-lookup"><span data-stu-id="42a77-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="42a77-113">Tutti gli utenti sa che i test positivi ridurrà il numero di difetti del software in un'applicazione e aumentare la qualità dell'applicazione, ma si verificano gli unit test posto va ben oltre appena individuando bug.</span><span class="sxs-lookup"><span data-stu-id="42a77-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="42a77-114">Un gruppo di test unità buona consente risparmiare tempo e rimanere in controllo del software che viene creato un team di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="42a77-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="42a77-115">Un team può apportare modifiche al codice esistente, eseguire il refactoring, la riprogettazione e la ristrutturazione software per soddisfare nuovi requisiti e aggiungere nuovi componenti in un'applicazione di tutto, sapendo il gruppo di test può verificare il comportamento dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="42a77-116">Unit test facciano parte di un ciclo rapido feedback per facilitare la modifica e mantenere la manutenibilità man mano che aumenta la complessità del software.</span><span class="sxs-lookup"><span data-stu-id="42a77-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="42a77-117">Gli unit test include tuttavia un prezzo.</span><span class="sxs-lookup"><span data-stu-id="42a77-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="42a77-118">Un team deve investire il tempo necessario per creare e gestire unit test.</span><span class="sxs-lookup"><span data-stu-id="42a77-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="42a77-119">L'entità degli sforzi necessari per creare questi test è direttamente correlato per il **testabilità** del software sottostante.</span><span class="sxs-lookup"><span data-stu-id="42a77-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="42a77-120">Per testare il software è semplice?</span><span class="sxs-lookup"><span data-stu-id="42a77-120">How easy is the software to test?</span></span> <span data-ttu-id="42a77-121">Un team di progettazione del software con particolare attenzione alla testabilità creerà più team che lavorano con il software non verificabile velocemente test efficaci.</span><span class="sxs-lookup"><span data-stu-id="42a77-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="42a77-122">Microsoft ha progettato ADO.NET Entity Framework 4.0 (EF4) con particolare attenzione alla testabilità.</span><span class="sxs-lookup"><span data-stu-id="42a77-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="42a77-123">Ciò non significa che gli sviluppatori scrivono gli unit test con codice del framework stesso.</span><span class="sxs-lookup"><span data-stu-id="42a77-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="42a77-124">Al contrario, gli obiettivi di testabilità di EF4 rendono semplice creare codice verificabile che il framework si basa.</span><span class="sxs-lookup"><span data-stu-id="42a77-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="42a77-125">Prima di esaminare esempi specifici, vale la pena conoscere le qualità del codice non testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="42a77-126">Le qualità del codice non testabile</span><span class="sxs-lookup"><span data-stu-id="42a77-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="42a77-127">Codice semplice eseguire il test presenterà sempre almeno due tratti.</span><span class="sxs-lookup"><span data-stu-id="42a77-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="42a77-128">In primo luogo, testabile codice è facile **osservare**.</span><span class="sxs-lookup"><span data-stu-id="42a77-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="42a77-129">Dato un set di input, deve essere facile da osservare l'output del codice.</span><span class="sxs-lookup"><span data-stu-id="42a77-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="42a77-130">Ad esempio, il seguente metodo di test è facile, poiché il metodo restituisce direttamente il risultato di un calcolo.</span><span class="sxs-lookup"><span data-stu-id="42a77-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="42a77-131">Un metodo di test è difficile se il metodo scrive il valore calcolato in un socket di rete, una tabella di database o un file, ad esempio il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="42a77-132">Il test deve eseguire operazioni aggiuntive per recuperare il valore.</span><span class="sxs-lookup"><span data-stu-id="42a77-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="42a77-133">In secondo luogo, è facile da codice non testabile **isolare**.</span><span class="sxs-lookup"><span data-stu-id="42a77-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="42a77-134">Utilizziamo lo pseudo-codice seguente come esempio di codice non testabile non valido.</span><span class="sxs-lookup"><span data-stu-id="42a77-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="42a77-135">Il metodo è facile concludere, possiamo passare una polizza assicurativa e verificare il valore restituito corrisponde a un risultato previsto.</span><span class="sxs-lookup"><span data-stu-id="42a77-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="42a77-136">Tuttavia, per testare il metodo è necessario disporre di un database installato con lo schema corretto e configurare il server SMTP, nel caso in cui il metodo tenta di inviare un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="42a77-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="42a77-137">Lo unit test vuole solo verificare che la logica di calcolo all'interno del metodo, ma il test potrebbe non riuscire perché il server di posta elettronica è offline o perché il server di database spostato.</span><span class="sxs-lookup"><span data-stu-id="42a77-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="42a77-138">Entrambi questi errori sono correlati al comportamento che è necessario verificare il test.</span><span class="sxs-lookup"><span data-stu-id="42a77-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="42a77-139">È difficile isolare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="42a77-140">Gli sviluppatori di software che si impegnano per scrivere codice verificabile spesso cercano di mantenere una separazione delle problematiche nel codice che scrivono.</span><span class="sxs-lookup"><span data-stu-id="42a77-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="42a77-141">Il metodo sopra indicato deve concentrarsi su calcoli aziendali e delegare i dettagli di implementazione del database e alla posta elettronica ad altri componenti.</span><span class="sxs-lookup"><span data-stu-id="42a77-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="42a77-142">Robert C. Martin viene chiamato il principio di singola responsabilità.</span><span class="sxs-lookup"><span data-stu-id="42a77-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="42a77-143">Un oggetto deve incapsulare una responsabilità singola, narrow, come calcolare il valore di un criterio.</span><span class="sxs-lookup"><span data-stu-id="42a77-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="42a77-144">Tutte le altre operazioni di database e la notifica devono essere la responsabilità di un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="42a77-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="42a77-145">Il codice scritto in questo modo è più semplice isolare perché si occupa di una singola attività.</span><span class="sxs-lookup"><span data-stu-id="42a77-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="42a77-146">In .NET sono disponibili le astrazioni, dobbiamo seguire il principio di singola responsabilità e ottenere l'isolamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="42a77-147">È possibile usare le definizioni di interfaccia e forzare il codice per usare l'astrazione dell'interfaccia invece di un tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="42a77-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="42a77-148">Più avanti in questo articolo si vedrà come un metodo, ad esempio il cattivo esempio presentato in precedenza può funzionare con interfacce che dovranno *aspetto* come che comunicherà con il database.</span><span class="sxs-lookup"><span data-stu-id="42a77-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="42a77-149">In fase di test, tuttavia, è possibile sostituire con un'implementazione fittizia non comunicare con il database, ma contiene invece i dati nella memoria.</span><span class="sxs-lookup"><span data-stu-id="42a77-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="42a77-150">Questa implementazione fittizia verrà isolare il codice da problemi non correlati nel codice di accesso ai dati o nella configurazione del database.</span><span class="sxs-lookup"><span data-stu-id="42a77-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="42a77-151">Esistono vantaggi aggiuntivi all'isolamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="42a77-152">Il calcolo di business in quest'ultimo metodo dovrebbe richiedere solo pochi millisecondi per l'esecuzione, ma potrebbe essere eseguiti per diversi secondi come gli hop di codice in base alla rete e comunica con il test stesso per i vari server.</span><span class="sxs-lookup"><span data-stu-id="42a77-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="42a77-153">Gli unit test devono eseguire fast per facilitare piccole modifiche.</span><span class="sxs-lookup"><span data-stu-id="42a77-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="42a77-154">Gli unit test devono anche essere ripetibile e non restituisce alcun errore perché un componente non correlato al test presenta un problema.</span><span class="sxs-lookup"><span data-stu-id="42a77-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="42a77-155">La scrittura di codice che è facile da osservare e isolare significa che gli sviluppatori disporranno di un'ora più facile scrivere test per il codice, dedicare meno tempo in attesa di test da eseguire e più importante, dedicare meno tempo tenere traccia dei bug che non esistono.</span><span class="sxs-lookup"><span data-stu-id="42a77-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="42a77-156">Si spera è possibile apprezzare i vantaggi di test e comprendere le qualità che presenta codice verificabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="42a77-157">Si sta tentando di risolvere come scrivere codice che funziona con EF4 per salvare i dati in un database, pur rimanendo observable e isolare facilmente, ma prima di tutto si sarà ridurre l'obiettivo per discutere le progettazioni testabili per accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="42a77-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="42a77-158">Schemi progettuali per la persistenza dei dati</span><span class="sxs-lookup"><span data-stu-id="42a77-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="42a77-159">Entrambi gli esempi non validi presentati in precedenza era troppe responsabilità.</span><span class="sxs-lookup"><span data-stu-id="42a77-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="42a77-160">Nel primo esempio errato era necessario eseguire un calcolo *e* scrivere in un file.</span><span class="sxs-lookup"><span data-stu-id="42a77-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="42a77-161">Nel secondo esempio errato era necessario leggere i dati da un database *e* eseguono un calcolo business *e* Invia messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="42a77-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="42a77-162">Progettando i metodi più piccoli che separano le problematiche e delegare la responsabilità ad altri componenti verranno apportate notevoli progressi nella scrittura di codice non testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="42a77-163">L'obiettivo è di creare funzionalità tramite la composizione di azioni da astrazioni più circoscritto e mirate.</span><span class="sxs-lookup"><span data-stu-id="42a77-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="42a77-164">Quando si tratta di persistenza dei dati il piccolo e mirate astrazioni ricerchiamo sono molto comuni sono stati documentati come modelli di progettazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="42a77-165">Libro di Fowler modelli di architettura delle applicazioni aziendali è stato il primo lavoro per descrivere questi modelli di stampa.</span><span class="sxs-lookup"><span data-stu-id="42a77-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="42a77-166">Prima di illustrare come questi ADO.NET Entity Framework implementa e funziona con questi modelli, forniremo una breve descrizione di questi modelli nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="42a77-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="42a77-167">Lo schema Repository</span><span class="sxs-lookup"><span data-stu-id="42a77-167">The Repository Pattern</span></span>

<span data-ttu-id="42a77-168">Fowler afferma un repository "funge da intermediario tra il dominio e i dati dei livelli di mapping tramite un'interfaccia simile a raccolta per l'accesso a oggetti di dominio".</span><span class="sxs-lookup"><span data-stu-id="42a77-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="42a77-169">L'obiettivo di schema repository è per isolare il codice da una chiara la panoramica dell'accesso ai dati e come abbiamo visto isolamento precedenti è una caratteristica necessaria per la verificabilità.</span><span class="sxs-lookup"><span data-stu-id="42a77-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="42a77-170">La chiave per l'isolamento è come il repository espone tramite un'interfaccia simile a raccolta di oggetti.</span><span class="sxs-lookup"><span data-stu-id="42a77-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="42a77-171">La logica che scrivere da non utilizzare nel repository avrà idea di come il repository verrà materializzare gli oggetti che richiesta.</span><span class="sxs-lookup"><span data-stu-id="42a77-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="42a77-172">Il repository può comunicare con un database oppure potrebbe semplicemente restituire gli oggetti da una raccolta in memoria.</span><span class="sxs-lookup"><span data-stu-id="42a77-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="42a77-173">Tutto il codice deve conoscere è il repository viene visualizzato per gestire la raccolta che è possibile recuperare, aggiungere ed eliminare oggetti dalla raccolta.</span><span class="sxs-lookup"><span data-stu-id="42a77-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="42a77-174">In applicazioni .NET esistenti in un repository concreto spesso eredita da un'interfaccia generica simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="42a77-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="42a77-175">Si renderà alcune modifiche alla definizione dell'interfaccia quando è fornire un'implementazione per EF4, ma il concetto di base rimane invariato.</span><span class="sxs-lookup"><span data-stu-id="42a77-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="42a77-176">Codice può usare un repository concreto che implementa questa interfaccia per recuperare un'entità dal valore della chiave primario, per recuperare una raccolta di entità in base alla valutazione di un predicato, o semplicemente recuperare tutte le entità disponibili.</span><span class="sxs-lookup"><span data-stu-id="42a77-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="42a77-177">Il codice può anche aggiungere e rimuovere entità tramite l'interfaccia del repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="42a77-178">Dato un oggetti IRepository del dipendente, codice può eseguire le operazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="42a77-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="42a77-179">Poiché il codice usa un'interfaccia (IRepository del dipendente), possiamo fornire il codice con diverse implementazioni dell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="42a77-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="42a77-180">Un'implementazione potrebbe essere un'implementazione fornita con EF4 e oggetti persistenti in un database Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a77-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="42a77-181">Un'implementazione diversa (uno viene usato durante il test) potrebbe essere supportata da oggetti un elenco di dipendenti in memoria.</span><span class="sxs-lookup"><span data-stu-id="42a77-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="42a77-182">L'interfaccia consente di ottenere l'isolamento del codice.</span><span class="sxs-lookup"><span data-stu-id="42a77-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="42a77-183">Si noti il IRepository&lt;T&gt; interfaccia non espone un'operazione di salvataggio.</span><span class="sxs-lookup"><span data-stu-id="42a77-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="42a77-184">Come si aggiorna gli oggetti esistenti?</span><span class="sxs-lookup"><span data-stu-id="42a77-184">How do we update existing objects?</span></span> <span data-ttu-id="42a77-185">Potrebbe riscontrare IRepository definizioni includono l'operazione di salvataggio e implementazioni di questi repository saranno necessario rendere subito persistente un oggetto nel database.</span><span class="sxs-lookup"><span data-stu-id="42a77-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="42a77-186">Tuttavia, in molte applicazioni non vogliamo rendere persistenti gli oggetti singolarmente.</span><span class="sxs-lookup"><span data-stu-id="42a77-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="42a77-187">Al contrario, vogliamo dare vita oggetti, ad esempio dal repository diversi, modificare tali oggetti come parte di un'attività di business e mentenere tutti gli oggetti come parte di una singola operazione atomica.</span><span class="sxs-lookup"><span data-stu-id="42a77-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="42a77-188">Fortunatamente, è previsto un modello per consentire questo tipo di comportamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="42a77-189">La schema Unit of Work</span><span class="sxs-lookup"><span data-stu-id="42a77-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="42a77-190">Fowler afferma un'unità di lavoro "manterrà un elenco di oggetti interessati da una transazione di business e coordina la scrittura all'esterno delle modifiche e la risoluzione dei problemi di concorrenza".</span><span class="sxs-lookup"><span data-stu-id="42a77-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="42a77-191">È responsabilità dell'unità di lavoro per rilevare le modifiche agli oggetti è dare vita da un repository e persistenti le modifiche sono state apportate agli oggetti quando viene informato che l'unità di lavoro per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="42a77-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="42a77-192">È anche la responsabilità dell'unità di lavoro per rendere i nuovi oggetti è stato aggiunto a tutti i repository e inserire gli oggetti in un database e anche l'eliminazione di gestire.</span><span class="sxs-lookup"><span data-stu-id="42a77-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="42a77-193">Se si rivelerà un'operazione qualsiasi lavoro con i set di dati ADO.NET quindi già sarà familiarità con la schema unit of work.</span><span class="sxs-lookup"><span data-stu-id="42a77-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="42a77-194">Set di dati ADO.NET aveva la possibilità di rilevare i nostri aggiornamenti, eliminazioni e l'inserimento di oggetti DataRow e potrebbe (con l'aiuto di un oggetto TableAdapter) risolvere le differenze tra tutte le modifiche a un database.</span><span class="sxs-lookup"><span data-stu-id="42a77-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="42a77-195">Oggetti DataSet modelli, tuttavia, un subset disconnesso del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="42a77-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="42a77-196">La schema unit of work presenta lo stesso comportamento, ma funziona con oggetti business e gli oggetti di dominio che sono a conoscenza del database e isolati dal codice di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="42a77-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="42a77-197">Un'astrazione per modellare l'unità di lavoro nel codice .NET potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="42a77-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="42a77-198">Esponendo i riferimenti di repository dall'unità di lavoro che è possibile assicurarsi una singola unità di lavoro oggetto ha la possibilità di tenere traccia di tutte le entità materializzate durante una transazione di business.</span><span class="sxs-lookup"><span data-stu-id="42a77-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="42a77-199">L'implementazione del metodo di Commit per un'unità di lavoro reale è dove molto importante per riconciliare le modifiche in memoria con il database.</span><span class="sxs-lookup"><span data-stu-id="42a77-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="42a77-200">Dato un riferimento a IUnitOfWork, codice possa apportare modifiche agli oggetti business recuperati da uno o più repository e salvare tutte le modifiche tramite l'operazione di Commit atomico.</span><span class="sxs-lookup"><span data-stu-id="42a77-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="42a77-201">Il modello di caricamento Lazy</span><span class="sxs-lookup"><span data-stu-id="42a77-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="42a77-202">Fowler utilizza il caricamento lazy di nome per descrivere "un oggetto che non contiene tutti i dati è necessario, ma in grado di eseguire questa operazione".</span><span class="sxs-lookup"><span data-stu-id="42a77-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="42a77-203">Il caricamento lazy trasparente è una caratteristica importante avere quando la scrittura di codice testabile business e l'utilizzo di un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="42a77-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="42a77-204">Ad esempio, si consideri il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="42a77-205">Modalità la raccolta di schede di popolamento?</span><span class="sxs-lookup"><span data-stu-id="42a77-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="42a77-206">Esistono due possibili risposte.</span><span class="sxs-lookup"><span data-stu-id="42a77-206">There are two possible answers.</span></span> <span data-ttu-id="42a77-207">Una risposta è che il repository dipendente, quando viene richiesto di recuperare un dipendente, invia una query per recuperare sia il dipendente insieme a informazioni sulle schede associate del dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="42a77-208">Nei database relazionali questo in genere richiede una query con una clausola JOIN e può comportare il recupero di più informazioni rispetto a un'applicazione è necessario.</span><span class="sxs-lookup"><span data-stu-id="42a77-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="42a77-209">Cosa accade se l'applicazione non deve mai la proprietà di schede di tocco?</span><span class="sxs-lookup"><span data-stu-id="42a77-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="42a77-210">Una risposta alla seconda consiste nel caricare la proprietà schede "su richiesta".</span><span class="sxs-lookup"><span data-stu-id="42a77-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="42a77-211">Il caricamento lazy è implicita e trasparente per la logica di business perché il codice non richiama le API speciale per recuperare informazioni sulle schede.</span><span class="sxs-lookup"><span data-stu-id="42a77-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="42a77-212">Il codice presuppone che le informazioni sulle schede è presente quando necessario.</span><span class="sxs-lookup"><span data-stu-id="42a77-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="42a77-213">Prevede alcune magic con il caricamento lazy che implica in genere l'intercettazione di runtime delle chiamate al metodo.</span><span class="sxs-lookup"><span data-stu-id="42a77-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="42a77-214">Il codice di intercettazione è responsabile della comunicazione con il database e il recupero di informazioni sulle schede lasciando la logica di business gratuita per la logica di business.</span><span class="sxs-lookup"><span data-stu-id="42a77-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="42a77-215">Questo magic di caricamento lazy consente di isolare se stesso da operazioni di recupero dei dati e i risultati in codice non testabile più il codice di business.</span><span class="sxs-lookup"><span data-stu-id="42a77-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="42a77-216">Lo svantaggio di un caricamento lazy è che quando un'applicazione *viene* necessarie le informazioni di smart card ora il codice eseguirà un'altra query.</span><span class="sxs-lookup"><span data-stu-id="42a77-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="42a77-217">Ciò non è un problema per molte applicazioni, ma per applicazioni sensibili alle prestazioni o le applicazioni a scorrimento in ciclo tra un numero di oggetti dipendenti e l'esecuzione di una query per recuperare le schede di tempo durante ogni iterazione del ciclo (un problema noto anche come N + 1 problema di query), il caricamento lazy è un trascinamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="42a77-218">In questi scenari un'applicazione potrebbe essere necessario caricare accuratamente le informazioni sulla scheda nel modo più efficiente possibile.</span><span class="sxs-lookup"><span data-stu-id="42a77-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="42a77-219">Fortunatamente, si vedrà come EF4 supporta entrambi carichi lazy implicite e carica impazientemente efficiente quando si spostano nella sezione successiva e implementazione di questi modelli.</span><span class="sxs-lookup"><span data-stu-id="42a77-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="42a77-220">Implementazione di modelli con Entity Framework</span><span class="sxs-lookup"><span data-stu-id="42a77-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="42a77-221">La buona notizia è che tutti i modelli di progettazione che è descritto nella sezione precedente sono semplici da implementare con EF4.</span><span class="sxs-lookup"><span data-stu-id="42a77-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="42a77-222">Per illustrare che verrà usata una semplice applicazione di ASP.NET MVC per modificare e visualizzare i dipendenti e le informazioni sulle schede associate.</span><span class="sxs-lookup"><span data-stu-id="42a77-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="42a77-223">Si inizierà con il seguente "plain old CLR Object" (poco).</span><span class="sxs-lookup"><span data-stu-id="42a77-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="42a77-224">Queste definizioni di classe verranno modificato leggermente esamineremo diversi approcci e le funzionalità di EF4, ma si desidera mantenere queste classi come non riconoscono la persistenza (PI) come possibili.</span><span class="sxs-lookup"><span data-stu-id="42a77-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="42a77-225">Un oggetto dispositivo PI all'oscuro *modo in cui*, o addirittura *se*, lo stato, contiene si trova all'interno di un database.</span><span class="sxs-lookup"><span data-stu-id="42a77-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="42a77-226">PI e poco vanno di pari passo con il software testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="42a77-227">Gli oggetti utilizzando un approccio POCO sono meno vincolata, più flessibile e più facilmente testabili perché possono operare senza un database presentano.</span><span class="sxs-lookup"><span data-stu-id="42a77-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="42a77-228">Con gli oggetti poco posto è possibile creare un modello EDM (Entity Data Model) in Visual Studio (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="42a77-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="42a77-229">Non utilizzeremo il modello EDM per generare il codice per le nostre entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="42a77-230">Al contrario, si vuole usare le entità che si lovingly elaborare manualmente.</span><span class="sxs-lookup"><span data-stu-id="42a77-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="42a77-231">Si userà solo il modello EDM per generare lo schema di database e fornire i metadati che ef4 deve eseguire il mapping di oggetti nel database.</span><span class="sxs-lookup"><span data-stu-id="42a77-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![test_01 Entity Framework](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="42a77-233">**Figura 1**</span><span class="sxs-lookup"><span data-stu-id="42a77-233">**Figure 1**</span></span>

<span data-ttu-id="42a77-234">Nota: se si desidera sviluppare prima di tutto il modello EDM, è possibile generare pulito e il codice POCO da EDM.</span><span class="sxs-lookup"><span data-stu-id="42a77-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="42a77-235">È possibile farlo con un'estensione di Visual Studio 2010 fornita dal team di programmabilità dei dati.</span><span class="sxs-lookup"><span data-stu-id="42a77-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="42a77-236">Per scaricare l'estensione, avviare il gestore estensioni dal menu Strumenti in Visual Studio e cercare "POCO" (vedere la figura 2) la raccolta di modelli online.</span><span class="sxs-lookup"><span data-stu-id="42a77-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="42a77-237">Sono disponibili diversi modelli di POCO per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="42a77-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="42a77-238">Per altre informazioni sull'utilizzo del modello, vedere " [procedura dettagliata: modello POCO per Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="42a77-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![test_02 Entity Framework](~/ef6/media/eftest-02.png)

<span data-ttu-id="42a77-240">**Figura 2**</span><span class="sxs-lookup"><span data-stu-id="42a77-240">**Figure 2**</span></span>

<span data-ttu-id="42a77-241">Da questo punto di partenza di POCO verranno discussi i due diversi approcci per codice non testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="42a77-242">Il primo approccio viene chiamato l'approccio Entity Framework poiché sfrutta astrazioni dall'API Entity Framework per implementare le unità di lavoro e i repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="42a77-243">Nel secondo approccio verrà creato un'astrazioni dei repository personalizzato e quindi visualizzare i vantaggi e svantaggi di ogni approccio.</span><span class="sxs-lookup"><span data-stu-id="42a77-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="42a77-244">Si inizierà esaminando l'approccio Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="42a77-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="42a77-245">Un'implementazione incentrato sugli elementi di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="42a77-245">An EF Centric Implementation</span></span>

<span data-ttu-id="42a77-246">Si consideri l'azione del controller seguente da un progetto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="42a77-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="42a77-247">L'azione recupera un oggetto dipendente e restituisce un risultato per visualizzare una visualizzazione dettagliata del dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="42a77-248">È il codice da testare?</span><span class="sxs-lookup"><span data-stu-id="42a77-248">Is the code testable?</span></span> <span data-ttu-id="42a77-249">Sono presenti almeno due test, che dobbiamo verificare il comportamento dell'azione.</span><span class="sxs-lookup"><span data-stu-id="42a77-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="42a77-250">In primo luogo, vorremmo verificare che l'azione restituisce la visualizzazione corretta: un semplice test.</span><span class="sxs-lookup"><span data-stu-id="42a77-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="42a77-251">È anche preferibile scrivere un test per verificare l'azione recupera il dipendente corretto e si vuole eseguire questa operazione senza l'esecuzione di codice per eseguire query sul database.</span><span class="sxs-lookup"><span data-stu-id="42a77-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="42a77-252">Tenere presente che si desidera isolare codice sottoposto a test.</span><span class="sxs-lookup"><span data-stu-id="42a77-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="42a77-253">Isolamento garantirà che il test non avere esito negativo a causa di un bug nel codice di accesso ai dati o nella configurazione del database.</span><span class="sxs-lookup"><span data-stu-id="42a77-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="42a77-254">Se il test ha esito negativo, si saprà che abbiamo un bug nella logica del controller e non in qualche componente di sistema di livello inferiore.</span><span class="sxs-lookup"><span data-stu-id="42a77-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="42a77-255">Per ottenere l'isolamento che è necessario alcune astrazioni, come le interfacce abbiamo presentati in precedenza per i repository e unità di lavoro.</span><span class="sxs-lookup"><span data-stu-id="42a77-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="42a77-256">Tenere presente che lo schema repository è progettato per eseguire la mediazione tra gli oggetti di dominio e il livello di mapping dei dati.</span><span class="sxs-lookup"><span data-stu-id="42a77-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="42a77-257">In questo scenario EF4 *viene* i dati eseguendo il mapping di livello e già fornisce un'astrazione simile a repository denominata IObjectSet&lt;T&gt; (dallo spazio dei nomi System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="42a77-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="42a77-258">La definizione dell'interfaccia è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="42a77-259">IObjectSet&lt;T&gt; soddisfi i requisiti per un repository perché è simile a una raccolta di oggetti (tramite IEnumerable&lt;T&gt;) e fornisce metodi per aggiungere e rimuovere gli oggetti dalla raccolta simulata.</span><span class="sxs-lookup"><span data-stu-id="42a77-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="42a77-260">I metodi di Attach e Detach espongono funzionalità aggiuntive dell'API EF4.</span><span class="sxs-lookup"><span data-stu-id="42a77-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="42a77-261">Usare IObjectSet&lt;T&gt; come interfaccia per i repository è necessaria un'unità di lavoro astrazione per associare tra loro i repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="42a77-262">Un'implementazione concreta di questa interfaccia comunicherà con SQL Server ed è facile creare utilizzando la classe ObjectContext EF4.</span><span class="sxs-lookup"><span data-stu-id="42a77-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="42a77-263">La classe ObjectContext è l'unità di lavoro nell'API EF4 reale.</span><span class="sxs-lookup"><span data-stu-id="42a77-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="42a77-264">Portare un IObjectSet&lt;T&gt; vita consiste semplicemente nella chiamata del metodo CreateObjectSet dell'oggetto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="42a77-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="42a77-265">Dietro le quinte il framework userà i metadati è disponibile nel modello EDM per produrre un concreto ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="42a77-266">Vengono illustrati i criteri con restituzione di IObjectSet&lt;T&gt; interfaccia perché aiuterà a mantenere la testabilità nel codice client.</span><span class="sxs-lookup"><span data-stu-id="42a77-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="42a77-267">Questa implementazione concreta è utile nell'ambiente di produzione, ma è necessario concentrare l'attenzione sul modo in cui si userà l'astrazione IUnitOfWork per facilitare il test.</span><span class="sxs-lookup"><span data-stu-id="42a77-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="42a77-268">Le copie di Test</span><span class="sxs-lookup"><span data-stu-id="42a77-268">The Test Doubles</span></span>

<span data-ttu-id="42a77-269">Per isolare l'azione del controller è necessario passare tra le reali unità di lavoro (supportato da un oggetto ObjectContext) e un'unità di double o "fittizio" test di lavoro (eseguendo operazioni in memoria).</span><span class="sxs-lookup"><span data-stu-id="42a77-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="42a77-270">L'approccio comune per eseguire questo tipo di trasferimento è per non consentire il controller MVC crea un'istanza di un'unità di lavoro, ma passare all'unità di lavoro nel controller come parametro costruttore.</span><span class="sxs-lookup"><span data-stu-id="42a77-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="42a77-271">Il codice riportato sopra è riportato un esempio di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="42a77-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="42a77-272">Il controller crea relativa dipendenza (l'unità di lavoro), ma inserisce la dipendenza nel controller non è consentita.</span><span class="sxs-lookup"><span data-stu-id="42a77-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="42a77-273">In un progetto MVC accade spesso di usare una factory del controller personalizzato in combinazione con un'inversione del contenitore del controllo (IoC) per automatizzare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="42a77-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="42a77-274">Questi argomenti sono esula dall'ambito di questo articolo, ma è possibile leggere più da seguendo i riferimenti alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="42a77-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="42a77-275">Un'unità fittizio di implementazione di lavoro che è possibile usare per il test potrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="42a77-276">Si noti che l'unità di lavoro fittizio espone una proprietà eseguito il commit.</span><span class="sxs-lookup"><span data-stu-id="42a77-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="42a77-277">È talvolta utile aggiungere funzionalità a una classe fittizio che facilitano il test.</span><span class="sxs-lookup"><span data-stu-id="42a77-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="42a77-278">In questo caso è facile concludere se codice esegue il commit di un'unità di lavoro selezionando il commit di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="42a77-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="42a77-279">Sono necessari anche un IObjectSet fittizio&lt;T&gt; di conservare gli oggetti dipendenti e dipendente in memoria.</span><span class="sxs-lookup"><span data-stu-id="42a77-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="42a77-280">Possiamo fornire un'implementazione singola usano i generics.</span><span class="sxs-lookup"><span data-stu-id="42a77-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="42a77-281">Questo test double delega la maggior parte del lavoro a un HashSet sottostante&lt;T&gt; oggetto.</span><span class="sxs-lookup"><span data-stu-id="42a77-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="42a77-282">Si noti che IObjectSet&lt;T&gt; richiede un vincolo generico applicando T come una classe (un tipo riferimento) e inoltre forzata l'implementazione di IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="42a77-283">È facile visualizzare una raccolta in memoria come un'interfaccia IQueryable&lt;T&gt; utilizzando l'operatore LINQ standard AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="42a77-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="42a77-284">I test</span><span class="sxs-lookup"><span data-stu-id="42a77-284">The Tests</span></span>

<span data-ttu-id="42a77-285">Unit test tradizionali userà una singola classe di test per contenere tutti i test per tutte le azioni in un singolo controller MVC.</span><span class="sxs-lookup"><span data-stu-id="42a77-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="42a77-286">È possibile scrivere questi test, o qualsiasi tipo di unit test, usando in memoria fakes è stata compilata.</span><span class="sxs-lookup"><span data-stu-id="42a77-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="42a77-287">Tuttavia, per questo articolo che si eviterà monolitico contrapposto approccio della classe di test e invece raggruppare i test per concentrarsi su una parte specifica della funzionalità.</span><span class="sxs-lookup"><span data-stu-id="42a77-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="42a77-288">Ad esempio, "Crea nuovo dipendente" potrebbe essere la funzionalità di cui che si vuole testare, quindi si userà una singola classe di test per verificare l'azione del controller unico responsabile per la creazione di un nuovo dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="42a77-289">È disponibile del codice il programma di installazione comuni che necessaria per tutte queste classi di test granulare.</span><span class="sxs-lookup"><span data-stu-id="42a77-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="42a77-290">Ad esempio, è sempre necessario creare i repository in memoria e fittizio unità di lavoro.</span><span class="sxs-lookup"><span data-stu-id="42a77-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="42a77-291">È necessario anche un'istanza del controller dipendente con l'unità di lavoro inserito fittizio.</span><span class="sxs-lookup"><span data-stu-id="42a77-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="42a77-292">Abbiamo potrai condividere questo codice di programma di installazione comuni tra le classi di test usando una classe di base.</span><span class="sxs-lookup"><span data-stu-id="42a77-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="42a77-293">Il "madre oggetto" viene usato nella classe di base è un modello comune per la creazione di dati di test.</span><span class="sxs-lookup"><span data-stu-id="42a77-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="42a77-294">Madre un oggetto contiene i metodi factory per creare un'istanza di entità di test da utilizzare in più strumenti di test.</span><span class="sxs-lookup"><span data-stu-id="42a77-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="42a77-295">È possibile usare il EmployeeControllerTestBase come classe base per un numero di Fixture di test (vedere la figura 3).</span><span class="sxs-lookup"><span data-stu-id="42a77-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="42a77-296">Ogni fixture di test verrà testata una specifica azione del controller.</span><span class="sxs-lookup"><span data-stu-id="42a77-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="42a77-297">Ad esempio, una fixture di test sarà incentrata sul test l'azione di creazione usata durante una richiesta HTTP GET (per visualizzare la visualizzazione per la creazione di un dipendente) e una fixture diversi sarà incentrata sull'azione di creazione usato in una richiesta HTTP POST (per richiedere informazioni trasmesse dal utente di creare un dipendente).</span><span class="sxs-lookup"><span data-stu-id="42a77-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="42a77-298">Ogni classe derivata solo è responsabile per la configurazione necessaria nel relativo contesto specifico e per fornire le asserzioni necessarie per verificare i risultati per il contesto di test specifico.</span><span class="sxs-lookup"><span data-stu-id="42a77-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![test_03 Entity Framework](~/ef6/media/eftest-03.png)

<span data-ttu-id="42a77-300">**Figura 3**</span><span class="sxs-lookup"><span data-stu-id="42a77-300">**Figure 3**</span></span>

<span data-ttu-id="42a77-301">Lo stile di test e convenzione di denominazione presentato in questo articolo non è necessario per codice non testabile: si tratta di un solo approccio.</span><span class="sxs-lookup"><span data-stu-id="42a77-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="42a77-302">Figura 4 mostra i test in esecuzione in Resharper il cervello Jet test runner plug-in per Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="42a77-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![test_04 Entity Framework](~/ef6/media/eftest-04.png)

<span data-ttu-id="42a77-304">**Figura 4**</span><span class="sxs-lookup"><span data-stu-id="42a77-304">**Figure 4**</span></span>

<span data-ttu-id="42a77-305">Con una classe base per gestire il codice di installazione condivisa, gli unit test per ogni azione del controller sono ridotte e facile da scrivere.</span><span class="sxs-lookup"><span data-stu-id="42a77-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="42a77-306">I test verranno eseguito rapidamente (Poiché stiamo eseguendo operazioni in memoria) e non devono avere esito negativo a causa non correlati dell'infrastruttura o preoccupazioni ambientali (perché è stato isolato dell'unità sottoposta a test).</span><span class="sxs-lookup"><span data-stu-id="42a77-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="42a77-307">In questi test, la classe base svolge la maggior parte delle operazioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="42a77-308">Tenere presente che il costruttore di classe di base viene creato il repository in memoria, un'unità di lavoro fittizio e un'istanza della classe EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="42a77-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="42a77-309">La classe di test deriva da questa classe di base e si concentra sulle specifiche del metodo di creazione di test.</span><span class="sxs-lookup"><span data-stu-id="42a77-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="42a77-310">In questo caso le specifiche riducono alla procedura "Disponi, agire e di asserzione" verrà visualizzato in qualsiasi unit test di procedure:</span><span class="sxs-lookup"><span data-stu-id="42a77-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="42a77-311">Creare un oggetto Nuovo_dipendente per simulare i dati in ingresso.</span><span class="sxs-lookup"><span data-stu-id="42a77-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="42a77-312">Richiamare l'azione di creazione del EmployeeController e passare il Nuovo_dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="42a77-313">Verificare che l'azione di creazione produce i risultati previsti (il dipendente viene visualizzato nel repository).</span><span class="sxs-lookup"><span data-stu-id="42a77-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="42a77-314">Che cosa abbiamo creato consente di testare qualsiasi azione EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="42a77-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="42a77-315">Ad esempio, quando si scrivono test per l'azione Index del controller dipendente è possibile ereditare dalla classe di base di test per stabilire la stessa configurazione di base per i test.</span><span class="sxs-lookup"><span data-stu-id="42a77-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="42a77-316">Anche in questo caso la classe di base verrà creato il repository in memoria, l'unità di lavoro fittizio e un'istanza di EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="42a77-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="42a77-317">I test per l'azione di indice necessario concentrarsi solo su come richiamare l'operazione sull'indice e restituisce la qualità del modello di azione di test.</span><span class="sxs-lookup"><span data-stu-id="42a77-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="42a77-318">I test che stiamo creando con fakes in memoria sono orientati ai test il *stato* del software.</span><span class="sxs-lookup"><span data-stu-id="42a77-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="42a77-319">Ad esempio, quando si desidera controllare lo stato del repository dopo che viene eseguita l'azione di creazione, l'azione di creazione di test repository contenere il nuovo dipendente?</span><span class="sxs-lookup"><span data-stu-id="42a77-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="42a77-320">In un secondo momento vedremo interazione basato su test.</span><span class="sxs-lookup"><span data-stu-id="42a77-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="42a77-321">Test di interazione basati su chiederà se il codice sottoposto a test richiamati i metodi appropriati dei nostri oggetti e passati i parametri corretti.</span><span class="sxs-lookup"><span data-stu-id="42a77-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="42a77-322">Per il momento si passerà il coperchio un altro modello di progettazione: il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="42a77-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="42a77-323">Il caricamento eager e il caricamento Lazy</span><span class="sxs-lookup"><span data-stu-id="42a77-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="42a77-324">In un determinato web ASP.NET MVC dell'applicazione che potrebbe essere piacerebbe per visualizzare le informazioni del dipendente e includere il dipendente associato schede.</span><span class="sxs-lookup"><span data-stu-id="42a77-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="42a77-325">Ad esempio, potremmo avere una visualizzazione di riepilogo da schede che mostra il nome del dipendente e il numero totale di schede nel sistema.</span><span class="sxs-lookup"><span data-stu-id="42a77-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="42a77-326">Esistono diversi approcci che è possibile eseguire per implementare questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="42a77-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="42a77-327">Proiezione</span><span class="sxs-lookup"><span data-stu-id="42a77-327">Projection</span></span>

<span data-ttu-id="42a77-328">Un approccio semplice per creare il riepilogo consiste nel creare un modello dedicato alle informazioni di cui che si vuole visualizzare nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="42a77-329">In questo scenario il modello potrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="42a77-330">Si noti che il EmployeeSummaryViewModel non è un'entità – in altre parole non che si vogliono rendere persistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="42a77-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="42a77-331">Si intende solo utilizzare questa classe in modo casuale i dati nella visualizzazione in modo fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="42a77-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="42a77-332">Il modello di visualizzazione è simile a un trasferimento dati (DTO) oggetto perché non contiene alcun comportamento (nessun metodi) – solo le proprietà.</span><span class="sxs-lookup"><span data-stu-id="42a77-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="42a77-333">Le proprietà saranno contenuti i dati, occorre spostare.</span><span class="sxs-lookup"><span data-stu-id="42a77-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="42a77-334">È facile creare un'istanza di questo modello di visualizzazione usando l'operatore di proiezione standard di LINQ: l'operatore Select.</span><span class="sxs-lookup"><span data-stu-id="42a77-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="42a77-335">Esistono due importanti funzionalità per il codice sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="42a77-335">There are two notable features to the above code.</span></span> <span data-ttu-id="42a77-336">Prima di tutto, il codice è facile eseguire il test perché è ancora facile osservare e isolare.</span><span class="sxs-lookup"><span data-stu-id="42a77-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="42a77-337">L'operatore Select funziona altrettanto bene con i nostri fakes in memoria a quanto accade con l'unità di lavoro reale.</span><span class="sxs-lookup"><span data-stu-id="42a77-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="42a77-338">La seconda funzionalità rilevanti è come il codice consente EF4 generare una query singola ed efficiente per assemblare dipendente e schede informazioni.</span><span class="sxs-lookup"><span data-stu-id="42a77-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="42a77-339">È stato caricato informazioni sui dipendenti e le informazioni sulla scheda in allo stesso oggetto senza usare alcuna API speciali.</span><span class="sxs-lookup"><span data-stu-id="42a77-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="42a77-340">Il codice espresso semplicemente le informazioni necessarie tramite operatori LINQ standard che funzionano con origini dati in memoria, nonché a origini dati remote.</span><span class="sxs-lookup"><span data-stu-id="42a77-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="42a77-341">È stato in grado di convertire gli alberi delle espressioni create dalle query LINQ e C EF4\# compilatore in una query T-SQL unica ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="42a77-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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

<span data-ttu-id="42a77-342">Sono disponibili in altri casi quando non si desidera lavorare con un modello di visualizzazione o di un oggetto DTO, ma con le entità reali.</span><span class="sxs-lookup"><span data-stu-id="42a77-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="42a77-343">Quando si conosce abbiamo bisogno di un dipendente *e* schede del dipendente, è possibile caricare rapidamente i dati correlati in modo efficiente e non intrusivo.</span><span class="sxs-lookup"><span data-stu-id="42a77-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="42a77-344">Caricamento Eager esplicito</span><span class="sxs-lookup"><span data-stu-id="42a77-344">Explicit Eager Loading</span></span>

<span data-ttu-id="42a77-345">Quando si vuole caricare rapidamente le informazioni sulle entità correlate occorre un meccanismo per la logica di business (o in questo scenario, logica di azione del controller) per esprimere il desiderio di repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="42a77-346">L'oggetto ObjectQuery EF4&lt;T&gt; classe definisce un metodo Include per specificare gli oggetti correlati da recuperare durante una query.</span><span class="sxs-lookup"><span data-stu-id="42a77-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="42a77-347">Ricordare l'ObjectContext EF4 espone entità tramite ObjectSet concreta&lt;T&gt; classe che eredita da ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="42a77-348">Se si stesse usando ObjectSet&lt;T&gt; riferimenti nel nostro azione del controller è stato possibile scrivere il codice seguente per specificare un caricamento eager delle informazioni sulle schede per ogni dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="42a77-349">Tuttavia, poiché Stiamo tentando di mantenere il nostro codice verificabile è sta esponendo non ObjectSet&lt;T&gt; all'esterno dell'unità reali della classe di lavoro.</span><span class="sxs-lookup"><span data-stu-id="42a77-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="42a77-350">Al contrario, ci si affida la IObjectSet&lt;T&gt; interfaccia che è più facile da simulare, ma IObjectSet&lt;T&gt; non definisce un metodo di inclusione.</span><span class="sxs-lookup"><span data-stu-id="42a77-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="42a77-351">La bellezza di LINQ è che possiamo creare un operatore di inclusione.</span><span class="sxs-lookup"><span data-stu-id="42a77-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="42a77-352">Si noti che questo operatore inclusione è definito come metodo di estensione per interfaccia IQueryable&lt;T&gt; anziché IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="42a77-353">Ciò offre la possibilità di usare il metodo con una vasta gamma di tipi possibili, tra cui IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;e ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="42a77-354">Nel caso in cui la sequenza sottostante non è un oggetto ObjectQuery EF4 autentico&lt;T&gt;, non esiste alcun modo e l'operatore di inclusione è no-op.</span><span class="sxs-lookup"><span data-stu-id="42a77-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="42a77-355">Se l'oggetto sottostante di sequenza *viene* un ObjectQuery&lt;T&gt; (o derivato da ObjectQuery&lt;T&gt;), quindi EF4 vedrà il requisito per i dati aggiuntivi e formulare SQL corretto query.</span><span class="sxs-lookup"><span data-stu-id="42a77-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="42a77-356">Con questo nuovo operatore posto è possibile richiedere in modo esplicito un caricamento eager delle informazioni sulle schede dal repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="42a77-357">Se viene eseguita in un ObjectContext reale, il codice genera la seguente query singola.</span><span class="sxs-lookup"><span data-stu-id="42a77-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="42a77-358">La query raccoglie le informazioni necessarie dal database in un unico round trip per materializzare gli oggetti dipendenti e completamente popolare le proprietà delle schede.</span><span class="sxs-lookup"><span data-stu-id="42a77-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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

<span data-ttu-id="42a77-359">La buona notizia è che resta testabile completamente, il codice all'interno del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="42a77-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="42a77-360">Non è necessario fornire le funzionalità aggiuntive per i nostri fakes supportare l'operatore di inclusione.</span><span class="sxs-lookup"><span data-stu-id="42a77-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="42a77-361">Quelle cattive sono che abbiamo dovuto utilizzare l'operatore di inclusione all'interno di codice che si desiderava mantenere che non riconoscono la persistenza.</span><span class="sxs-lookup"><span data-stu-id="42a77-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="42a77-362">Questo è un ottimo esempio del tipo di compromessi da valutare durante la compilazione di codice non testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="42a77-363">Esistono casi è necessario per consentire la persistenza delle perdite di problematiche di fuori l'astrazione di repository per soddisfare gli obiettivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="42a77-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="42a77-364">L'alternativa per il caricamento eager è il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="42a77-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="42a77-365">Indica di caricamento lazy facciamo *non* necessario il codice di business per annunciare in modo esplicito il requisito per i dati associati.</span><span class="sxs-lookup"><span data-stu-id="42a77-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="42a77-366">Invece, utilizziamo le nostre entità nell'applicazione e se i dati aggiuntivi sono necessari Entity Framework verranno caricati i dati su richiesta.</span><span class="sxs-lookup"><span data-stu-id="42a77-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="42a77-367">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="42a77-367">Lazy Loading</span></span>

<span data-ttu-id="42a77-368">È facile immaginare uno scenario in cui non si sa cosa saranno necessario dati in un componente della logica di business.</span><span class="sxs-lookup"><span data-stu-id="42a77-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="42a77-369">Potrebbe essere sappiamo che la logica richiede un oggetto dipendente, ma si può creare rami in diversi percorsi di esecuzione in cui alcuni di questi percorsi richiedono informazioni sulle schede del dipendente e alcune non lo sono.</span><span class="sxs-lookup"><span data-stu-id="42a77-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="42a77-370">Scenari simili sono perfetti per implicita il caricamento lazy poiché i dati vengono visualizzati magicamente in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="42a77-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="42a77-371">Il caricamento lazy, noto anche come posticipato il caricamento, posizionare alcuni requisiti dei nostri oggetti entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="42a77-372">Oggetti poco con mancato riconoscimento della persistenza true non dovrebbe affrontare tutti i requisiti dal livello di persistenza, ma è praticamente impossibile raggiungere mancato riconoscimento della persistenza true.</span><span class="sxs-lookup"><span data-stu-id="42a77-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="42a77-373">In alternativa viene misurata mancato riconoscimento della persistenza in gradi relativi.</span><span class="sxs-lookup"><span data-stu-id="42a77-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="42a77-374">Sarebbe sfortunato se fosse necessario ereditare una classe di base di persistenza orientata ai servizi o usare un insieme specifico per ottenere il caricamento lazy di oggetti poco.</span><span class="sxs-lookup"><span data-stu-id="42a77-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="42a77-375">Fortunatamente, EF4 è una soluzione meno intrusiva.</span><span class="sxs-lookup"><span data-stu-id="42a77-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="42a77-376">Rilevabili</span><span class="sxs-lookup"><span data-stu-id="42a77-376">Virtually Undetectable</span></span>

<span data-ttu-id="42a77-377">Quando si usano gli oggetti POCO, EF4 può generare in modo dinamico i proxy di runtime per le entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="42a77-378">Questi proxy in modo invisibile il wrapping di oggetti poco materializzati e forniscono servizi aggiuntivi mediante l'intercettazione di ogni proprietà ottenere e impostare operazione da eseguire operazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="42a77-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="42a77-379">Uno di questi servizi è la funzionalità di caricamento lazy che stiamo cercando.</span><span class="sxs-lookup"><span data-stu-id="42a77-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="42a77-380">Un altro servizio è una modifica efficiente meccanismo che consentono di registrare quando i valori delle proprietà di un'entità viene modificato il programma di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="42a77-381">L'elenco delle modifiche viene utilizzata da ObjectContext durante il metodo SaveChanges per rendere persistente qualsiasi entità modificate usando i comandi di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="42a77-382">Per questi proxy a funzionare, tuttavia, hanno bisogno di hook in routine property get in modo e set di operazioni su un'entità e i proxy raggiungere questo obiettivo eseguendo l'override di membri virtuali.</span><span class="sxs-lookup"><span data-stu-id="42a77-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="42a77-383">Di conseguenza, se si vuole che il caricamento lazy implicito e il rilevamento delle modifiche efficienti è necessario tornare indietro ai nostri le definizioni delle classi POCO e contrassegnare le proprietà come virtuale.</span><span class="sxs-lookup"><span data-stu-id="42a77-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="42a77-384">È comunque possibile affermare che l'entità dipendente è principalmente non riconoscono la a livello di persistenza.</span><span class="sxs-lookup"><span data-stu-id="42a77-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="42a77-385">L'unico requisito consiste nell'usare i membri virtuali e non influisce la testabilità del codice.</span><span class="sxs-lookup"><span data-stu-id="42a77-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="42a77-386">Non è necessario derivare da una speciale classe di base, o addirittura usare una raccolta speciale dedicata per il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="42a77-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="42a77-387">Come illustrato nel codice, qualsiasi classe che implementa ICollection&lt;T&gt; è disponibile per includere le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="42a77-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="42a77-388">È inoltre disponibile una modifica secondaria, che è necessario apportare all'interno di nostra unità di lavoro.</span><span class="sxs-lookup"><span data-stu-id="42a77-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="42a77-389">Il caricamento lazy costituisce *disattivata* per impostazione predefinita quando si lavora direttamente con un oggetto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="42a77-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="42a77-390">È disponibile una proprietà che è possibile impostare sulla proprietà ContextOptions per abilitare il caricamento posticipato ed è possibile impostare questa proprietà all'interno di nostra unità di lavoro reale Se si vuole abilitare ovunque il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="42a77-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="42a77-391">Con l'abilitazione del caricamento lazy implicito, il codice dell'applicazione può usare un dipendente e del dipendente associati schede tempo pur rimanendo inconsapevoli del lavoro richiesto per Entity Framework caricare i dati aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="42a77-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="42a77-392">Il caricamento lazy rende più semplice scrivere il codice dell'applicazione e con il magic proxy resta testabile completamente, il codice.</span><span class="sxs-lookup"><span data-stu-id="42a77-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="42a77-393">Fakes in memoria dell'unità di lavoro può essere precaricata semplicemente fittizio entità con dati associati secondo necessità durante un test.</span><span class="sxs-lookup"><span data-stu-id="42a77-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="42a77-394">A questo punto è possibile concentrare l'attenzione dalla creazione di repository con IObjectSet&lt;T&gt; ed esaminare le astrazioni per nascondere tutti i segni di framework di persistenza.</span><span class="sxs-lookup"><span data-stu-id="42a77-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="42a77-395">Repository personalizzati</span><span class="sxs-lookup"><span data-stu-id="42a77-395">Custom Repositories</span></span>

<span data-ttu-id="42a77-396">Quando l'unità di lavoro schema progettuale è prima di tutto presentati in questo articolo che viene fornito un codice di esempio per ciò che potrebbe essere simile all'unità di lavoro.</span><span class="sxs-lookup"><span data-stu-id="42a77-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="42a77-397">Verrà ora nuovamente presentare questa idea originale usando il dipendente e lo scenario da schede dipendente che questo abbiamo lavorato con.</span><span class="sxs-lookup"><span data-stu-id="42a77-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="42a77-398">La differenza principale tra questa unità di lavoro e l'unità di lavoro è stato creato nell'ultima sezione è come questa unità di lavoro non usa alcun astrazioni da framework EF4 (non vi è alcun IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="42a77-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="42a77-399">IObjectSet&lt;T&gt; funziona bene come un'interfaccia del repository, ma l'API espone può non allineare perfettamente alle esigenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="42a77-400">In questo approccio imminente rappresentiamo repository tramite un IRepository personalizzato&lt;T&gt; astrazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="42a77-401">Molti sviluppatori che seguono test-driven design, progettazione basata sul comportamento e la progettazione di metodologie basata su domini preferiscono la IRepository&lt;T&gt; approccio per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="42a77-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="42a77-402">Primo, il IRepository&lt;T&gt; interfaccia rappresenta un livello "anti-danneggiamento".</span><span class="sxs-lookup"><span data-stu-id="42a77-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="42a77-403">Come descritto da Eric Evans nel suo libro Domain Driven Design un livello antidanneggiamento mantiene il codice di dominio dall'infrastruttura API, ad esempio un'API di persistenza.</span><span class="sxs-lookup"><span data-stu-id="42a77-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="42a77-404">In secondo luogo, gli sviluppatori possono compilare i metodi nel repository che soddisfano le esigenze specifiche di un'applicazione (come individuato durante la scrittura di test).</span><span class="sxs-lookup"><span data-stu-id="42a77-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="42a77-405">Ad esempio, potrebbe essere necessario spesso a individuare una singola entità tramite un valore di ID, in modo che è possibile aggiungere un metodo FindById per l'interfaccia del repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="42a77-406">Nostro IRepository&lt;T&gt; definizione avrà un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="42a77-407">Si noti che verrà eliminato nuovamente all'uso di un'interfaccia IQueryable&lt;T&gt; interfaccia da esporre le raccolte di entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="42a77-408">Oggetto IQueryable&lt;T&gt; consente gli alberi delle espressioni LINQ al flusso in un provider EF4 e fornire una visione olistica del query il provider.</span><span class="sxs-lookup"><span data-stu-id="42a77-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="42a77-409">Una seconda opzione, è possibile restituire IEnumerable&lt;T&gt;, ovvero il provider LINQ EF4 visualizzerà solo le espressioni compilate all'interno del repository.</span><span class="sxs-lookup"><span data-stu-id="42a77-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="42a77-410">Qualsiasi raggruppamento, ordinamento e proiezione effettuata all'esterno del repository non comporranno il comando SQL inviato al database, che può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="42a77-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="42a77-411">D'altra parte, un repository restituisca solo IEnumerable&lt;T&gt; risultati non verranno mai strano, ma con un nuovo comando SQL.</span><span class="sxs-lookup"><span data-stu-id="42a77-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="42a77-412">Funzioneranno entrambi gli approcci e rimangono testabili entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="42a77-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="42a77-413">Si tratta di semplice fornire una singola implementazione del IRepository&lt;T&gt; interfaccia usando l'API di ObjectContext EF4 e generics.</span><span class="sxs-lookup"><span data-stu-id="42a77-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="42a77-414">Il IRepository&lt;T&gt; approccio offre un maggiore controllo sulle query poiché dispone di un client per richiamare un metodo per ottenere un'entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="42a77-415">All'interno del metodo potremmo offriamo controlli aggiuntivi e gli operatori LINQ per applicare i vincoli dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="42a77-416">Si noti che l'interfaccia dispone di due vincoli sul parametro di tipo generico.</span><span class="sxs-lookup"><span data-stu-id="42a77-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="42a77-417">Il primo vincolo è l'aspetto degli svantaggi classe richiesto da ObjectSet&lt;T&gt;, e il secondo vincolo forza le nostre entità implementare IEntity – un'astrazione creata per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="42a77-418">L'interfaccia IEntity impone le entità abbiano una proprietà Id leggibile e quindi è possibile usare questa proprietà nel metodo FindById.</span><span class="sxs-lookup"><span data-stu-id="42a77-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="42a77-419">IEntity viene definita con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="42a77-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="42a77-420">IEntity può essere considerato una violazione di piccole dimensioni del mancato riconoscimento della persistenza poiché le nostre entità necessarie per implementare questa interfaccia.</span><span class="sxs-lookup"><span data-stu-id="42a77-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="42a77-421">Tenere presente mancato riconoscimento della persistenza riguarda i compromessi e per molte funzionalità FindById ottenuti superino il vincolo imposto dall'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="42a77-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="42a77-422">L'interfaccia non ha alcun impatto su testabilità.</span><span class="sxs-lookup"><span data-stu-id="42a77-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="42a77-423">Creare un'istanza di un live IRepository&lt;T&gt; richiede un oggetto ObjectContext EF4, in modo che un'unità concreta dell'implementazione di lavoro deve gestire la creazione di istanze.</span><span class="sxs-lookup"><span data-stu-id="42a77-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="42a77-424">Usare il Repository personalizzato</span><span class="sxs-lookup"><span data-stu-id="42a77-424">Using the Custom Repository</span></span>

<span data-ttu-id="42a77-425">Usando il repository personalizzato non è significativamente diversa dall'uso del repository basato su IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="42a77-426">Anziché applicare operatori LINQ direttamente a una proprietà, è necessario innanzitutto uno richiamare i metodi del repository per recuperare un oggetto IQueryable&lt;T&gt; riferimento.</span><span class="sxs-lookup"><span data-stu-id="42a77-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="42a77-427">Si noti che l'operatore di inclusione personalizzato che viene implementati in precedenza funzionerà senza alcuna modifica.</span><span class="sxs-lookup"><span data-stu-id="42a77-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="42a77-428">Metodo FindById del repository rimuove i duplicati per la logica da azioni sta tentando di recuperare una singola entità.</span><span class="sxs-lookup"><span data-stu-id="42a77-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="42a77-429">Non c'è alcuna differenza significativo in fase di testabilità di aver esaminato i due approcci.</span><span class="sxs-lookup"><span data-stu-id="42a77-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="42a77-430">È stato possibile fornire implementazioni fittizie di IRepository&lt;T&gt; creando classi concrete supportate da HashSet&lt;dipendente&gt; : analogamente a ciò che abbiamo fatto nell'ultima sezione.</span><span class="sxs-lookup"><span data-stu-id="42a77-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="42a77-431">Tuttavia, alcuni sviluppatori preferiscono usare oggetti fittizi e simulare il framework di oggetti invece di compilare fakes.</span><span class="sxs-lookup"><span data-stu-id="42a77-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="42a77-432">Esamineremo utilizzando oggetti simulati per testare l'implementazione e illustra le differenze tra gli oggetti fittizi e fake nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="42a77-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="42a77-433">Simulazioni e test</span><span class="sxs-lookup"><span data-stu-id="42a77-433">Testing with Mocks</span></span>

<span data-ttu-id="42a77-434">Esistono diversi approcci per la creazione di un "test double" quali chiamate di Martin Fowler.</span><span class="sxs-lookup"><span data-stu-id="42a77-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="42a77-435">Un test double (ad esempio, un stunt film double) è un oggetto compilato per "in evidenza" per il real, gli oggetti di produzione durante i test.</span><span class="sxs-lookup"><span data-stu-id="42a77-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="42a77-436">I repository in memoria che è stata creata sono copie di test per i repository che comunicano con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a77-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="42a77-437">Abbiamo visto come utilizzare queste copie di test durante gli unit test per isolare il codice e mantenere i test che vengono eseguiti rapidamente.</span><span class="sxs-lookup"><span data-stu-id="42a77-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="42a77-438">Le copie di test che è stata compilata dispongono di implementazioni reali, l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="42a77-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="42a77-439">Dietro le quinte ciascuno di essi archivia una raccolta di oggetti concreta e verranno aggiungere e rimuovere gli oggetti da questa raccolta come manipolare il repository durante un test.</span><span class="sxs-lookup"><span data-stu-id="42a77-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="42a77-440">Alcuni sviluppatori preferiscono creare le copie di test in questo modo, con le implementazioni di lavoro e codice reale.</span><span class="sxs-lookup"><span data-stu-id="42a77-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="42a77-441">Queste copie di test sono quelle che chiamiamo *fakes*.</span><span class="sxs-lookup"><span data-stu-id="42a77-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="42a77-442">Dispongono di implementazioni di lavoro, ma non sono sufficientemente reali per la produzione.</span><span class="sxs-lookup"><span data-stu-id="42a77-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="42a77-443">Il repository fittizio non essendo possibile scrivere nel database.</span><span class="sxs-lookup"><span data-stu-id="42a77-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="42a77-444">Il server SMTP fittizio non effettivamente invia un messaggio di posta elettronica sulla rete.</span><span class="sxs-lookup"><span data-stu-id="42a77-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="42a77-445">Oggetti fittizi e Fakes</span><span class="sxs-lookup"><span data-stu-id="42a77-445">Mocks versus Fakes</span></span>

<span data-ttu-id="42a77-446">Esiste anche un altro tipo di test double noto come un *simulare*.</span><span class="sxs-lookup"><span data-stu-id="42a77-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="42a77-447">Mentre fakes dispongono di implementazioni di lavoro, gli oggetti fittizi sono dotate di alcuna implementazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="42a77-448">Con l'aiuto di un framework di oggetti fittizi è costruire questi oggetti fittizi in fase di esecuzione e usarli come copie di test.</span><span class="sxs-lookup"><span data-stu-id="42a77-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="42a77-449">In questa sezione verrà usato il comportamento fittizio Moq framework open source.</span><span class="sxs-lookup"><span data-stu-id="42a77-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="42a77-450">Ecco un semplice esempio dell'uso Moq per creare dinamicamente un test double per un repository dipendente.</span><span class="sxs-lookup"><span data-stu-id="42a77-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="42a77-451">Verranno chiesti Moq un IRepository&lt;dipendente&gt; implementazione e viene creato uno in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="42a77-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="42a77-452">È possibile ottenere per l'oggetto che implementa IRepository&lt;dipendente&gt; accedendo alle proprietà dell'oggetto della simulazione&lt;T&gt; oggetto.</span><span class="sxs-lookup"><span data-stu-id="42a77-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="42a77-453">Si tratta di questo oggetto interno che è possibile passare al nostro controller e non sapranno se si tratta di una copia di test o il repository reale.</span><span class="sxs-lookup"><span data-stu-id="42a77-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="42a77-454">È possibile richiamare i metodi sull'oggetto esattamente come si potrebbero richiamare metodi su un oggetto con un'implementazione reale.</span><span class="sxs-lookup"><span data-stu-id="42a77-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="42a77-455">È necessario chiedersi cosa il repository fittizio farà quando si richiama il metodo Add.</span><span class="sxs-lookup"><span data-stu-id="42a77-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="42a77-456">Poiché non esiste alcuna implementazione sottostante l'oggetto fittizio, Add non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="42a77-457">Non è Nessuna raccolta concreti dietro le quinte come avevamo la fakes che è stato scritto, in modo che il dipendente viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="42a77-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="42a77-458">Per quanto riguarda il valore restituito di FindById?</span><span class="sxs-lookup"><span data-stu-id="42a77-458">What about the return value of FindById?</span></span> <span data-ttu-id="42a77-459">In questo caso l'oggetto fittizio esegue l'unica cosa che può eseguire, viene restituita un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="42a77-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="42a77-460">Poiché si sta restituendo un tipo riferimento (un dipendente), il valore restituito è un valore null.</span><span class="sxs-lookup"><span data-stu-id="42a77-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="42a77-461">Gli oggetti fittizi potrebbero sembrare inutili; Tuttavia, esistono due altre funzionalità di simulazioni su che non abbiamo parlato.</span><span class="sxs-lookup"><span data-stu-id="42a77-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="42a77-462">In primo luogo, il framework Moq registra tutte le chiamate effettuate per l'oggetto fittizio.</span><span class="sxs-lookup"><span data-stu-id="42a77-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="42a77-463">In un secondo momento nel codice è possibile porre Moq se chiunque richiamato il metodo Add, o se tutti gli utenti ha richiamato il metodo FindById.</span><span class="sxs-lookup"><span data-stu-id="42a77-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="42a77-464">Si vedrà più avanti come è possibile usare questa funzionalità di registrazione "black box" nei test.</span><span class="sxs-lookup"><span data-stu-id="42a77-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="42a77-465">La seconda funzionalità ideale è che modo utilizzare Moq programmare un oggetto fittizio con *aspettative*.</span><span class="sxs-lookup"><span data-stu-id="42a77-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="42a77-466">Un'aspettativa indica l'oggetto fittizio come rispondere a qualsiasi interazione specificato.</span><span class="sxs-lookup"><span data-stu-id="42a77-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="42a77-467">Ad esempio, è possibile programmare un'aspettativa nel nostro simulazione e ordinargli di restituire un oggetto dipendente, quando un utente richiama FindById.</span><span class="sxs-lookup"><span data-stu-id="42a77-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="42a77-468">Il framework Moq utilizza un'API di installazione e le espressioni lambda per queste aspettative del programma.</span><span class="sxs-lookup"><span data-stu-id="42a77-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="42a77-469">In questo esempio chiediamo Moq creare dinamicamente un repository e abbiamo programmare il repository con una previsione.</span><span class="sxs-lookup"><span data-stu-id="42a77-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="42a77-470">L'aspettativa indica l'oggetto fittizio per restituire un nuovo oggetto dipendente con Id uguale a 5, quando un utente richiama il metodo FindById passando un valore pari a 5.</span><span class="sxs-lookup"><span data-stu-id="42a77-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="42a77-471">Questo test venga superato e non è stato necessario creare un'implementazione completa per IRepository fittizio&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="42a77-472">È possibile rivedere i test che è stata scritta in precedenza e modifiche in modo che usino le simulazioni anziché fakes.</span><span class="sxs-lookup"><span data-stu-id="42a77-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="42a77-473">Esattamente come prima, si userà una classe di base per configurare i componenti comuni dell'infrastruttura che necessaria per tutti i test del controller.</span><span class="sxs-lookup"><span data-stu-id="42a77-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="42a77-474">Il codice di installazione rimane prevalentemente invariato.</span><span class="sxs-lookup"><span data-stu-id="42a77-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="42a77-475">Invece di usare fakes, useremo Moq per costruire oggetti fittizi.</span><span class="sxs-lookup"><span data-stu-id="42a77-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="42a77-476">La classe di base predispone l'unità di lavoro fittizio restituire un repository fittizio quando codice richiama le proprietà dipendenti.</span><span class="sxs-lookup"><span data-stu-id="42a77-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="42a77-477">Il resto dell'installazione fittizio avrà luogo all'interno di Fixture di test dedicati per ogni scenario specifico.</span><span class="sxs-lookup"><span data-stu-id="42a77-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="42a77-478">Ad esempio, la fixture di test per l'azione Index configurerà il repository fittizio per restituire un elenco di dipendenti quando l'azione richiama il metodo FindAll del repository fittizio.</span><span class="sxs-lookup"><span data-stu-id="42a77-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="42a77-479">Tranne le aspettative, i nostri test hanno un aspetto simile ai test creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="42a77-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="42a77-480">Tuttavia, con la possibilità di registrazione di un framework di simulazione è possiamo approccio test da un angolo diverso.</span><span class="sxs-lookup"><span data-stu-id="42a77-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="42a77-481">Esamineremo questa nuova prospettiva nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="42a77-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="42a77-482">Stato e l'interazione di test</span><span class="sxs-lookup"><span data-stu-id="42a77-482">State versus Interaction Testing</span></span>

<span data-ttu-id="42a77-483">Esistono diverse tecniche utilizzabili per testare il software con oggetti fittizi.</span><span class="sxs-lookup"><span data-stu-id="42a77-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="42a77-484">Uno degli approcci è usare lo stato basato su test, che è quanto è stato fatto in questo articolo finora.</span><span class="sxs-lookup"><span data-stu-id="42a77-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="42a77-485">Stato basato su test rende asserzioni sullo stato del software.</span><span class="sxs-lookup"><span data-stu-id="42a77-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="42a77-486">Nell'ultima dei test è richiamato un metodo di azione nel controller ed effettuata un'asserzione relativi al modello che la compilazione dovrebbe.</span><span class="sxs-lookup"><span data-stu-id="42a77-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="42a77-487">Ecco alcuni altri esempi dello stato di test:</span><span class="sxs-lookup"><span data-stu-id="42a77-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="42a77-488">Verificare che il repository contiene il nuovo oggetto dipendente dopo l'esecuzione di Create.</span><span class="sxs-lookup"><span data-stu-id="42a77-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="42a77-489">Verificare che il modello contiene un elenco di tutti i dipendenti dopo l'esecuzione di indice.</span><span class="sxs-lookup"><span data-stu-id="42a77-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="42a77-490">Verificare che il repository non contenga un dipendente specificato dopo l'esecuzione di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="42a77-491">Verrà visualizzato con oggetti fittizi un altro approccio consiste nel verificare *interazioni*.</span><span class="sxs-lookup"><span data-stu-id="42a77-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="42a77-492">Mentre lo stato basato su test rende asserzioni sullo stato degli oggetti, l'interazione basato su test rende asserzioni, sull'interagiscano tra oggetti.</span><span class="sxs-lookup"><span data-stu-id="42a77-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="42a77-493">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42a77-493">For example:</span></span>

-   <span data-ttu-id="42a77-494">Verificare che il controller Richiama metodo Add del repository quando si esegue Create.</span><span class="sxs-lookup"><span data-stu-id="42a77-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="42a77-495">Verificare che il controller Richiama metodo FindAll del repository durante l'esecuzione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="42a77-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="42a77-496">Verificare che il controller richiama l'unità di metodo di Commit del lavoro per salvare le modifiche apportate durante l'esecuzione di modifica.</span><span class="sxs-lookup"><span data-stu-id="42a77-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="42a77-497">Interazione test spesso richiede meno dati di test, perché è non siano in grado di all'interno di raccolte e verificare i conteggi.</span><span class="sxs-lookup"><span data-stu-id="42a77-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="42a77-498">Ad esempio, se si conosce che l'azione Details Richiama metodo FindById del repository con il valore corretto, quindi l'azione è probabilmente il corretto comportamento.</span><span class="sxs-lookup"><span data-stu-id="42a77-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="42a77-499">È possibile verificare questo comportamento senza dover configurare tutti i dati di test da restituire dalla FindById.</span><span class="sxs-lookup"><span data-stu-id="42a77-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="42a77-500">Il programma di installazione solo richiesto nella fixture di test precedente è il programma di installazione fornito dalla classe di base.</span><span class="sxs-lookup"><span data-stu-id="42a77-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="42a77-501">Quando si richiama l'azione del controller, Moq registrerà le interazioni con il repository fittizio.</span><span class="sxs-lookup"><span data-stu-id="42a77-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="42a77-502">Usando l'API di Moq verificare, possiamo porre Moq se il controller di richiamata FindById con il valore dell'ID corretto.</span><span class="sxs-lookup"><span data-stu-id="42a77-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="42a77-503">Se il controller non ha richiamato il metodo o ha richiamato il metodo con un valore di parametro imprevisto, il metodo di verifica genera un'eccezione e il test avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="42a77-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="42a77-504">Ecco un altro esempio per verificare che l'azione di creazione richiama Commit in unità di lavoro corrente.</span><span class="sxs-lookup"><span data-stu-id="42a77-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="42a77-505">Un aspetto con il testing di interazione è la tendenza per specificare le interazioni tramite.</span><span class="sxs-lookup"><span data-stu-id="42a77-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="42a77-506">La capacità dell'oggetto fittizio per registrare e verificare che ogni interazione con l'oggetto simulato non significa che il test è consigliabile provare a verificare ogni interazione.</span><span class="sxs-lookup"><span data-stu-id="42a77-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="42a77-507">Alcune interazioni sono dettagli di implementazione ed è consigliabile verificare solo le interazioni *obbligatorio* per soddisfare il test corrente.</span><span class="sxs-lookup"><span data-stu-id="42a77-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="42a77-508">La scelta tra oggetti fittizi o simulazioni dipende in gran parte del sistema si esegue il test e il personale (o team) delle preferenze.</span><span class="sxs-lookup"><span data-stu-id="42a77-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="42a77-509">Gli oggetti fittizi possono ridurre drasticamente la quantità di codice che è necessario implementare le copie di test, ma non tutti gli utenti è comodo le aspettative di programmazione e la verifica delle interazioni.</span><span class="sxs-lookup"><span data-stu-id="42a77-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="42a77-510">Conclusioni</span><span class="sxs-lookup"><span data-stu-id="42a77-510">Conclusions</span></span>

<span data-ttu-id="42a77-511">In questo white paper sono stati illustrati diversi approcci per la creazione di codice non testabile quando si usa ADO.NET Entity Framework per la persistenza dei dati.</span><span class="sxs-lookup"><span data-stu-id="42a77-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="42a77-512">È possibile sfruttare astrazioni incorporate, ad esempio IObjectSet&lt;T&gt;, oppure creare proprie astrazioni come IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="42a77-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="42a77-513">In entrambi i casi, il supporto POCO in ADO.NET Entity Framework 4.0 consente ai consumer di queste astrazioni di rimanere permanente che non riconoscono la e altamente testabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="42a77-514">Funzionalità EF4 aggiuntive, ad esempio il caricamento lazy implicito consente al servizio business e dell'applicazione del codice senza preoccuparsi dei dettagli di un archivio dati relazionale.</span><span class="sxs-lookup"><span data-stu-id="42a77-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="42a77-515">Infine, le astrazioni che creiamo sono facili da fittizio o fittizio all'interno di unit test e possiamo usare queste copie di test per ottenere tempi di esecuzione veloce, elevata isolata e i test.</span><span class="sxs-lookup"><span data-stu-id="42a77-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="42a77-516">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42a77-516">Additional Resources</span></span>

-   <span data-ttu-id="42a77-517">Robert C. Martin, " [il principio di singola responsabilità](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="42a77-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="42a77-518">Martin Fowler [catalogo dei modelli](http://www.martinfowler.com/eaaCatalog/index.html) da *modelli di architettura dell'applicazione Enterprise*</span><span class="sxs-lookup"><span data-stu-id="42a77-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="42a77-519">Caprio Griffin, " [inserimento delle dipendenze](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="42a77-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="42a77-520">Blog di programmabilità dei dati, " [procedura dettagliata: sviluppo con Entity Framework 4.0 basato su Test](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="42a77-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="42a77-521">Blog di programmabilità dei dati, " [modelli con Repository e unità di lavoro con Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="42a77-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="42a77-522">Dave Astels, " [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="42a77-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="42a77-523">Aaron Jensen, " [Introduzione a macchine specifiche](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="42a77-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="42a77-524">Eric Lee, " [BDD con MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="42a77-524">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="42a77-525">Eric Evans, " [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="42a77-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="42a77-526">Martin Fowler, " [fittizi e stub](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="42a77-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="42a77-527">Martin Fowler, " [Test Double](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="42a77-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="42a77-528">Jeremy Miller, " [lo stato rispetto a test l'interazione](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="42a77-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="42a77-529">Moq</span><span class="sxs-lookup"><span data-stu-id="42a77-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="42a77-530">Biografia</span><span class="sxs-lookup"><span data-stu-id="42a77-530">Biography</span></span>

<span data-ttu-id="42a77-531">Scott Allen è un membro del personale tecnico in Pluralsight e fondatore di OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="42a77-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="42a77-532">In 15 anni di sviluppo del software commerciale, Scott ha lavorato su soluzioni per tutti i dati da dispositivi con embedded 8 bit per le applicazioni web ASP.NET altamente scalabile.</span><span class="sxs-lookup"><span data-stu-id="42a77-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="42a77-533">È possibile contattare Scott sul suo blog all'indirizzo OdeToCode o su Twitter all'indirizzo [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="42a77-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
