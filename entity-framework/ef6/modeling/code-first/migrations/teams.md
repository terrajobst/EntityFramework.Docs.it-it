---
title: Migrazioni Code First in Entity Framework 6 - ambienti di Team
author: divega
ms.date: 10/23/2016
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 53460b6cdd454099ccf93b4e2133e4ea21278a64
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857468"
---
# <a name="code-first-migrations-in-team-environments"></a>Migrazioni Code First in ambienti di Team
> [!NOTE]
> Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base. Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Scarica un caffè, è necessario leggere questo articolo completo

I problemi negli ambienti di team sono prevalentemente riguardo l'unione, le migrazioni quando due sviluppatori hanno generato le migrazioni di base di codice locale. Anche se i passaggi per risolvere questi sono piuttosto semplici, è necessario avere una conoscenza approfondita del funzionamento di migrazioni richiedono. Non semplicemente passare direttamente alla fine, il tempo necessario per leggere l'intero articolo per verificare che l'operazione ha esito positivo.

## <a name="some-general-guidelines"></a>Linee guida generali

Prima di approfondire come gestire le migrazioni unione generate da più sviluppatori, ecco alcune linee guida generali per raggiungere risultati di successo.

### <a name="each-team-member-should-have-a-local-development-database"></a>Ogni membro del team deve avere un database di sviluppo locale

Le migrazioni viene utilizzata la  **\_ \_MigrationsHistory** tabella per archiviare le migrazioni sono state applicate al database. Se si devono di più sviluppatori generazione diverse migrazioni durante il tentativo di nello stesso database di destinazione (e pertanto condividono un  **\_ \_MigrationsHistory** tabella) migrazioni dovrà verificare alcuni problemi.

Naturalmente, se si dispone di membri del team che non verranno generate le migrazioni, è sufficiente che condividono un database centrale di sviluppo.

### <a name="avoid-automatic-migrations"></a>Evitare di processi di migrazione automatiche

La conclusione è che le migrazioni automatiche inizialmente visualizzate correttamente in ambienti di team, ma in realtà semplicemente non funzionano. Se si desidera conoscere la causa, continuare a leggere: in caso contrario, è possibile ignorare la sezione successiva.

Processi di migrazione automatiche consente di disporre dello schema del database aggiornato in modo da corrispondere al modello corrente senza la necessità di generare file di codice (le migrazioni basate su codice). Processi di migrazione automatiche funzionano molto bene in un ambiente di team mai più usati e mai generato le migrazioni basate su codice. Il problema è che le migrazioni automatiche sono limitate e non gestiscono un numero di operazioni: Rinomina proprietà/colonna, lo spostamento dei dati a un'altra tabella, e così via. Per gestire questi scenari si finisce generando le migrazioni basate su codice (e la modifica del codice sottoposto a scaffolding) che vengono combinati tra le modifiche che sono gestite dai processi di migrazione automatiche. In questo modo è quasi nella comunicazione Impossibile eseguire il merge delle modifiche quando due gli sviluppatori archiviano le migrazioni.

## <a name="screencasts"></a>Screencast

Se si sarebbe piuttosto guardare uno screencast rispetto a leggere questo articolo, i due video seguenti riguardano lo stesso contenuto in questo articolo.

### <a name="video-one-migrations---under-the-hood"></a>Un video: "Migrazioni - dietro le quinte"

[In questo screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) riguarda la modalità di traccia delle migrazioni e utilizza le informazioni sul modello per rilevare le modifiche al modello.

### <a name="video-two-migrations---team-environments"></a>Due video: "Migrazioni - ambienti di Team"

I concetti del video precedente, di compilazione [questo screencast](http://channel9.msdn.com/blogs/ef/migrations-team-environments) vengono illustrati i problemi che si verificano in un ambiente di team e come risolverli.

## <a name="understanding-how-migrations-works"></a>Comprendere il funzionamento delle migrazioni

La chiave per l'utilizzo corretto delle migrazioni in un ambiente di team è una semplice comprendere come tiene traccia delle migrazioni e Usa le informazioni sul modello per rilevare le modifiche al modello.

### <a name="the-first-migration"></a>La prima migrazione

Quando si aggiunge la prima migrazione al progetto, si esegue simile **Add-Migration prima** nella Console di gestione pacchetti. I passaggi di alto livello che esegue questo comando sono illustrati in precedenza.

![Prima della migrazione](~/ef6/media/firstmigration.png)

Il modello corrente viene calcolato dal codice (1). Gli oggetti di database necessari vengono quindi calcolati le differenze tra modelli (2): poiché si tratta della prima migrazione il modello sono diversi usi soli un modello vuoto per il confronto. Le modifiche necessarie vengono passate al generatore di codice per compilare il codice di migrazione necessarie (3) che viene quindi aggiunto alla soluzione Visual Studio (4).

Oltre al codice di migrazione effettivo che viene archiviato nel file di codice principale, le migrazioni genera inoltre un file code-behind aggiuntivi. Questi file sono metadati che viene usato per le migrazioni e non sono un elemento che è consigliabile modificare. Uno di questi file è un file di risorse (resx) che contiene uno snapshot del modello al momento che della migrazione è stata generata. Si noterà come viene usato nel passaggio successivo.

A questo punto è probabilmente verrebbe eseguito **Update-Database** per rendere effettive le modifiche al database e quindi passare sull'implementazione di altre aree dell'applicazione.

### <a name="subsequent-migrations"></a>Migrazioni successive

In un secondo momento si tornare indietro e apportare alcune modifiche al modello: in questo esempio si aggiungerà un **Url** proprietà **Blog**. È quindi necessario eseguire un comando, ad esempio **Add-Migration AddUrl** per eseguire lo scaffolding di una migrazione per il database corrispondente di applicare le modifiche. I passaggi di alto livello che esegue questo comando sono illustrati in precedenza.

![Seconda migrazione](~/ef6/media/secondmigration.png)

Come in precedenza, il modello corrente viene calcolato dal codice (1). Tuttavia, questa volta sono presenti le migrazioni esistenti in modo che il modello precedente viene recuperato dalla migrazione più recente (2). Questi due modelli sono addedgroups per trovare le modifiche del database necessarie (3) e quindi il completamento del processo come indicato in precedenza.

Questo stesso processo viene usato per eventuali altre migrazioni aggiunti al progetto.

### <a name="why-bother-with-the-model-snapshot"></a>Perché è necessario eseguire lo snapshot del modello?

Si potrebbe chiedere perché EF anche con lo snapshot del modello: il motivo per cui non solo esaminare il database. In questo caso, continuare a leggere. Se non si è interessati è possibile ignorare questa sezione.

Esistono diversi motivi, a che Entity Framework mantiene lo snapshot modello intorno:

-   Consente il database dello sfasamento dal modello di Entity Framework. È possibile apportare queste modifiche direttamente nel database oppure è possibile modificare il codice con scaffolding nelle migrazioni per apportare le modifiche. Ecco alcuni esempi di ciò in pratica:
    -   Si desidera aggiungere un inserite e aggiornate alla colonna a una o più delle tabelle, ma non si vuole includere queste colonne nel modello di Entity Framework. Se le migrazioni preso in esame il database che verrebbe continuamente tenta di eliminare queste colonne ogni volta che è stato eseguito lo scaffolding di una migrazione. Usa lo snapshot del modello, Entity Framework rileverà unicamente legittime modifiche al modello.
    -   Si desidera modificare il corpo di una stored procedure usata per gli aggiornamenti da includere alcune registrazioni. Se le migrazioni esaminato questa stored procedure dal database tenterebbe continuamente di ripristinare la definizione che si aspetta di Entity Framework. Usando lo snapshot del modello, Entity Framework sempre eseguirà lo scaffolding di codice per modificare la stored procedure quando si modifica la forma della procedura nel modello di Entity Framework.
    -   Questi stessi principi valgono per l'aggiunta di indici aggiuntivi, incluse le tabelle aggiuntive nel database, il mapping di Entity Framework per una vista di database che si trova su una tabella e così via.
-   Il modello di Entity Framework contiene solo la forma del database. L'intero modello consente migrazioni esaminare le informazioni sulle proprietà e le classi nel modello e sul relativo mapping alle tabelle e colonne. Queste informazioni consentono di essere più intelligente nel codice che esegue lo scaffolding delle migrazioni. Ad esempio, se si modifica il nome della colonna che esegue il mapping di una proprietà per le migrazioni possa rilevare la ridenominazione dalla che è la stessa proprietà – qualcosa che non può essere eseguita se si ha solo lo schema del database. 

## <a name="what-causes-issues-in-team-environments"></a>Qual è la causa problemi negli ambienti di team

Il flusso di lavoro illustrati nel funzionamento della sezione precedente ideale quando si è un singolo sviluppatore che lavora in un'applicazione. Funziona anche in un ambiente con team se si è l'unica persona ad apportare modifiche al modello. In questo scenario è possibile apportare modifiche al modello, generare le migrazioni e inviarli al controllo del codice sorgente. Altri sviluppatori possono sincronizzare le modifiche ed eseguire **Update-Database** affinché le modifiche dello schema applicate.

I problemi danno avvio a cui si verificano quando si devono di più sviluppatori apportano modifiche al modello di Entity Framework e l'invio di controllo del codice sorgente nello stesso momento. Ciò che non dispone di Entity Framework è un modo eccellente per unire le migrazioni locali con le migrazioni che un altro sviluppatore è inviata al controllo del codice sorgente perché ultima sincronizzazione.

## <a name="an-example-of-a-merge-conflict"></a>Un esempio di un conflitto di merge

Primo verrà ora esaminato un esempio concreto di un conflitto di merge. Microsoft continuerà in con l'esempio che è stato esaminato in precedenza. Come un avvio di un punto si presuppongono le modifiche nella sezione precedente sono state archiviate dallo sviluppatore dell'originale. Si terrà traccia due sviluppatori quando si apportano modifiche al codice base.

Si terrà traccia il modello di EF e le migrazioni attraverso una serie di modifiche. Per un punto di partenza, entrambi gli sviluppatori sono sincronizzati con il repository del controllo codice sorgente, come illustrato nella figura seguente.

![Punto di partenza](~/ef6/media/startingpoint.png)

Per gli sviluppatori \#1 e per gli sviluppatori \#2 ora apporta alcune modifiche al modello di Entity Framework nel proprio codice locale base. Per gli sviluppatori \#aggiunge 1 una **Rating** proprietà **Blog** – e genera un **AddRating** migrazione per applicare le modifiche al database. Per gli sviluppatori \#2 aggiunge una **lettori** proprietà **Blog** – e genera il corrispondente **AddReaders** migrazione. Entrambi gli sviluppatori di eseguire **Update-Database**, per applicare le modifiche ai database locali e quindi continuare a sviluppare l'applicazione.

> [!NOTE]
> Le migrazioni sono precedute da un timestamp, in modo che il nostro grafico rappresenta che la migrazione AddReaders dello sviluppatore \#2 dopo la migrazione AddRating proviene Developer \#1. Se per gli sviluppatori \#1 o \#2 non generato la stabilita prima migrazione alcuna differenza per i problemi dell'utilizzo di un team o il processo per eseguire il merge che vedremo nella prossima sezione.

![Modifiche locali](~/ef6/media/localchanges.png)

È un giorno fortunato per sviluppatore \#1 in tempo reale per inviare le modifiche prima di tutto. Poiché non ve ne ha archiviato poiché vengono sincronizzati con i repository, può solo inviare le modifiche senza eseguire alcuna operazione di unione.

![Invia](~/ef6/media/submit.png)

È ora per sviluppatore \#2 da inviare. Non sono pertanto fortunati. Perché un altro utente ha inviato le modifiche perché sono sincronizzati, dovranno ottenga le modifiche e di tipo merge. Il controllo del codice sorgente probabilmente sarà in grado di unire automaticamente le modifiche a livello di codice perché sono molto semplici. Lo stato di sviluppatore \#2 locale del repository dopo la sincronizzazione è illustrata nella figura seguente. 

![Eseguire il pull](~/ef6/media/pull.png)

In questa fase per sviluppatori \#2 possono eseguire **Update-Database** cui rileverà il nuovo **AddRating** migrazione (che non sia stata applicata allo sviluppatore \#del 2 database) e applicarlo. A questo punto il **Rating** viene aggiunta la **blog** tabella e il database è sincronizzato con il modello.

Esistono tuttavia alcuni problemi:

1.  Sebbene **Update-Database** applicherà le **AddRating** migrazione, verrà anche generato un avviso: *non è possibile aggiornare database in modo che corrisponda al modello corrente perché sono presenti modifiche in sospeso e la migrazione automatica è disabilitata...*
    Il problema è che lo snapshot del modello archiviato nell'ultima migrazione (**AddReader**) non è presente il **classificazione** proprietà sul **Blog** (poiché non è stato incluso nel modello di quando il la migrazione è stata generata). Il codice rileva prima che il modello nell'ultima migrazione non corrisponda al modello corrente e genera l'avviso.
2.  Esegue l'applicazione genera un'eccezione InvalidOperationException che indica che "*il modello supporta il contesto 'BloggingContext' è stato modificato dal momento della creazione del database. È consigliabile usare migrazioni Code First per aggiornare il database..."*
    Il problema è anche in questo caso, lo snapshot del modello archiviato nell'ultima migrazione non corrisponda al modello corrente.
3.  Infine, è prevedibile in esecuzione **Add-Migration** ora genera una migrazione vuota (perché non sono presenti modifiche da applicare al database). Tuttavia, poiché le migrazioni confronta il modello corrente a quello dall'ultima migrazione (che non è presente il **Rating** proprietà) verrà effettivamente lo scaffolding di un'altra **AddColumn** chiamata per aggiungere il **Rating** colonna. Naturalmente, questa migrazione avrà esito negativo durante **Update-Database** perché la **Rating** colonna esiste già.

## <a name="resolving-the-merge-conflict"></a>Risolvere il conflitto di merge

La buona notizia è che non è troppo difficile da affrontare il merge manuale, purché tu abbia una conoscenza del funzionamento delle migrazioni. Pertanto, se è stata ignorata direttamente a questa sezione... è necessario tornare indietro e leggere prima di tutto il resto dell'articolo.

Sono disponibili due opzioni, la più semplice consiste nel generare una migrazione vuota con il modello corretto corrente come snapshot. La seconda opzione consiste nell'aggiornare lo snapshot nell'ultima migrazione per avere i valori corretti snapshot del modello. La seconda opzione è un po' più difficile e non può essere utilizzata in ogni scenario, ma è anche più chiaro perché, non prevede l'aggiunta di una migrazione aggiuntiva.

### <a name="option-1-add-a-blank-merge-migration"></a>Opzione 1: Aggiungere una migrazione vuota 'merge'

In questo caso è possibile generare una migrazione vuota esclusivamente allo scopo di garantire la migrazione di più recente è lo snapshot corretto del modello in essa archiviati.

Questa opzione è utilizzabile indipendentemente che ha generato l'ultima migrazione. Nell'esempio è stato seguito Developer \#2 si sta occupando di merge e si sono verificati per generare l'ultima migrazione. Ma questi stessi passaggi possono essere utilizzati se per gli sviluppatori \#generato 1 l'ultima migrazione. I passaggi si applicano anche se sono coinvolti più migrazioni, è stato appena stato esaminando due per mantenerla semplice.

Per questo approccio, a partire dal momento in cui che si può notare, che è necessario effettuare modifiche che devono essere sincronizzati dal controllo del codice sorgente può essere usata la seguente procedura.

1.  Verificare che eventuali modifiche apportate al modello in sospeso nella base di codice locale sono stati scritti di una migrazione. In questo modo, che non perdere eventuali modifiche legittime quando giunge il momento per generare la migrazione vuota.
2.  Sincronizzazione con controllo del codice sorgente.
3.  Eseguire **Update-Database** per applicare eventuali migrazioni nuove che sono archiviati altri sviluppatori.
    **_Nota:_**  *se non si ottengono tutti gli avvisi dal comando Update-Database, quindi si sono verificati alcun nuovo migrazioni da altri sviluppatori e non è necessario eseguire alcuna ulteriore operazione di unione.*
4.  Eseguire **Add-Migration &lt;seleziona\_una\_nome&gt; – IgnoreChanges** (ad esempio, **Merge Add-Migration – IgnoreChanges**). Questo genera una migrazione con tutti i metadati (incluso uno snapshot del modello corrente), ma ignorerà le modifiche viene rilevato durante il confronto dei modelli di corrente in base allo snapshot nelle migrazioni ultimo (vale a dire si ottiene un valore vuoto **backup** e **Verso il basso** (metodo)).
5.  Continuare a sviluppare o inviare al controllo del codice sorgente (dopo che eseguono gli unit test naturalmente).

Ecco lo stato di sviluppatore \#2 locale del codebase dopo aver usato questo approccio.

![Migrazione di tipo merge](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Opzione 2: Aggiornare lo snapshot del modello nell'ultima migrazione

Questa opzione è molto simile all'opzione 1, ma rimuove la migrazione: vuota aggiuntive perché parliamoci chiaro, che richiede i file di codice aggiuntivo nella soluzione offerta.

**Questo approccio è realizzabile solo se l'ultima migrazione esiste solo nella base di codice locale e non è ancora stato inviato al controllo del codice sorgente (ad esempio, se l'ultima migrazione è stato generato dall'utente che esegue il merge)**. Modificare i metadati delle migrazioni che altri sviluppatori possono sono già applicati per i database di sviluppo, o addirittura peggio ancora applicato a un database di produzione, può comportare effetti collaterali imprevisti. Durante il processo di dobbiamo annullare l'ultima migrazione nel nostro database locale e applicarlo nuovamente con i metadati aggiornati.

Mentre l'ultima migrazione deve semplicemente essere nella base non sono previste restrizioni al numero o ordine di migrazioni di procedere, il codice locale. Possono esserci più migrazioni da più sviluppatori diverse e si applicano gli stessi passaggi, si abbiamo appena stata esaminando due per mantenerla semplice.

Per questo approccio, a partire dal momento in cui che si può notare, che è necessario effettuare modifiche che devono essere sincronizzati dal controllo del codice sorgente può essere usata la seguente procedura.

1.  Verificare che eventuali modifiche apportate al modello in sospeso nella base di codice locale sono stati scritti di una migrazione. In questo modo, che non perdere eventuali modifiche legittime quando giunge il momento per generare la migrazione vuota.
2.  Sincronizzazione con il controllo del codice sorgente.
3.  Eseguire **Update-Database** per applicare eventuali migrazioni nuove che sono archiviati altri sviluppatori.
    **_Nota:_**  *se non si ottengono tutti gli avvisi dal comando Update-Database, quindi si sono verificati alcun nuovo migrazioni da altri sviluppatori e non è necessario eseguire alcuna ulteriore operazione di unione.*
4.  Eseguire **Update-Database-TargetMigration &lt;secondo\_ultima\_migrazione&gt;**  (nell'esempio è stato seguito questo sarebbe **Update-Database: TargetMigration AddRating**). Questo ruoli di cui eseguire il backup il database allo stato del secondo ultima migrazione, in modo efficace 'senza applica' l'ultima migrazione dal database.
    **_Nota:_**  *questo passaggio è obbligatorio per renderlo più sicuro modificare i metadati della migrazione perché i metadati vengono archiviati anche nel \_ \_MigrationsHistoryTable del database. È per questo motivo è consigliabile usare questa opzione solo se l'ultima migrazione è solo nella base di codici locale. Se altri database era l'ultima migrazione applicata è anche necessario il rollback e riapplicare l'ultima migrazione per aggiornare i metadati.* 
5.  Eseguire **Add-Migration &lt;completo\_name\_inclusi\_timestamp\_del\_ultima\_migrazione** &gt; (nell'esempio è stato seguito il risultato sarà simile **Add-Migration 201311062215252\_AddReaders**).
    **_Nota:_**  *è necessario includere il timestamp in modo che le migrazioni sappia che si desidera modificare la migrazione esistente anziché lo scaffolding di una nuova.*
    Verranno aggiornati i metadati per l'ultima migrazione in modo che corrisponda al modello corrente. Si otterrà il seguente messaggio di avviso quando il comando viene completato, ma questo è esattamente ciò che si desidera. "*Solo il codice di progettazione per la migrazione ' 201311062215252\_AddReaders' è stato nuovamente sottoposto a scaffolding. Per lo scaffolding nuovamente l'intera migrazione, usare il parametro - Force. "*
6.  Eseguire **Update-Database** di riapplicare l'ultima migrazione con i metadati aggiornati.
7.  Continuare a sviluppare o inviare al controllo del codice sorgente (dopo che eseguono gli unit test naturalmente).

Ecco lo stato di sviluppatore \#2 locale del codebase dopo aver usato questo approccio.

![Aggiornamento dei metadati](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Riepilogo

Esistono alcuni problemi quando si usa migrazioni Code First in un ambiente di team. Tuttavia, una conoscenza di base del funzionamento delle migrazioni e alcuni approcci semplici per la risoluzione dei conflitti di merge rendono semplice ovviare a questi problemi.

Il problema fondamentale è non corretto dei metadati archiviati nel processo di migrazione più recente. In questo modo Code First per rilevare erroneamente che il modello corrente e lo schema del database non corrispondono e per eseguire lo scaffolding di codice non corretto nel processo di migrazione successivo. Questa situazione possa essere risolti, la generazione di una migrazione vuota con il modello corretto, o aggiornando i metadati nel processo di migrazione più recente.
