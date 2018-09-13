---
title: Migrazioni Code First con un database esistente - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: 77370ec7d922b8324b924a0b4aca3e58f5ec6066
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490571"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrazioni Code First con un database esistente
> [!NOTE]
> **EF4.3 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Questo articolo illustra l'uso di migrazioni Code First con un database esistente, che non è stato creato da Entity Framework.

> [!NOTE]
> Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base. Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.

## <a name="screencasts"></a>Screencast

Se si sarebbe piuttosto guardare uno screencast rispetto a leggere questo articolo, i due video seguenti riguardano lo stesso contenuto in questo articolo.

### <a name="video-one-migrations---under-the-hood"></a>Un video: "Migrazioni - dietro le quinte"

[In questo screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) riguarda la modalità di traccia delle migrazioni e utilizza le informazioni sul modello per rilevare le modifiche al modello.

### <a name="video-two-migrations---existing-databases"></a>Due video: "Migrazioni - database esistente"

I concetti del video precedente, di compilazione [questo screencast](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) illustra come abilitare e usare le migrazioni con un database esistente.

## <a name="step-1-create-a-model"></a>Passaggio 1: Creare un modello

Il primo passaggio sarà quello di creare un modello Code First destinato a un database esistente. Il [Code First per un Database esistente](~/ef6/modeling/code-first/workflows/existing-database.md) argomento fornisce indicazioni dettagliate su come eseguire questa operazione.

>[!NOTE]
> È importante seguire il resto dei passaggi in questo argomento prima di apportare modifiche al modello che richiede modifiche allo schema del database. I passaggi seguenti richiedono il modello sia sincronizzato con lo schema del database.

## <a name="step-2-enable-migrations"></a>Passaggio 2: Abilitare le migrazioni

Il passaggio successivo è abilitare le migrazioni. È possibile farlo eseguendo il **Enable-Migrations** comando nella Console di gestione pacchetti.

Questo comando creare una cartella nella soluzione denominata migrazioni e inserire una singola classe in essa definita configurazione. La classe di configurazione è possibile configurare le migrazioni per l'applicazione, è possibile trovare altre informazioni, vedere la [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) argomento.

## <a name="step-3-add-an-initial-migration"></a>Passaggio 3: Aggiungere una migrazione iniziale

Dopo che le migrazioni sono state create e applicate a database locale che è anche possibile applicarli diventa altri database. Ad esempio, il database locale può essere un database di test e si potrebbe in definitiva desidera inoltre applicare le modifiche a un database di produzione e/o altri sviluppatori di database di test. Sono disponibili due opzioni per questo passaggio e quello che è consigliabile scegliere dipende se lo schema di altri database è vuoto o attualmente corrispondenti allo schema del database locale.

-   **Opzione uno: Utilizzare schema esistente come punto di partenza.** È necessario utilizzare questo approccio quando altri database che in futuro verranno applicate le migrazioni a avrà lo stesso schema come ha attualmente il database locale. Ad esempio, è possibile utilizzare questo se i database di test locale corrisponde attualmente al v1 del database di produzione e si applicherà in seguito queste migrazioni per aggiornare il database di produzione a v2.
-   **Opzione 2: Usare database vuoto come punto di partenza.** È consigliabile usare questo approccio quando altri database che in futuro verranno applicate le migrazioni a sono vuoti (o non esistono ancora). Ad esempio, è possibile utilizzare questo se subito a sviluppare l'applicazione usando un database di test ma senza usare le migrazioni e si sarà in un secondo momento creare un database di produzione da zero.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opzione uno: Usare lo schema esistente come punto di partenza

Migrazioni Code First utilizza uno snapshot del modello archiviato nel processo di migrazione più recente per rilevare le modifiche apportate al modello (è possibile trovare informazioni dettagliate in proposito [migrazioni Code First in ambienti di Team](~/ef6/modeling/code-first/migrations/teams.md)). Dal momento che si prevede di presumere che i database già lo schema del modello corrente, verranno generati una migrazione (no-op) vuota che contiene il modello corrente come snapshot.

1.  Eseguire la **InitialCreate Add-Migration – IgnoreChanges** comando nella Console di gestione pacchetti. Verrà creata una migrazione vuota con il modello corrente come snapshot.
2.  Eseguire la **Update-Database** comando nella Console di gestione pacchetti. In questo applicherà la migrazione InitialCreate al database. Poiché la migrazione effettiva non contiene tutte le modifiche, semplicemente aggiungere una riga per il \_ \_MigrationsHistory tabella che indica che questa migrazione è già stata applicata.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opzione 2: Usare database vuoto come punto di partenza

In questo scenario è necessario migrazioni sia in grado di creare l'intero database da zero:, comprese le tabelle che sono già presenti nel database locale. Verranno per generare una migrazione InitialCreate che include la logica per creare lo schema esistente. Si renderà quindi il database esistente sembra questa migrazione è già stata applicata.

1.  Eseguire la **Add-Migration InitialCreate** comando nella Console di gestione pacchetti. In questo modo viene creata una migrazione per creare lo schema esistente.
2.  Impostare come commento tutto il codice nel metodo verticale della migrazione appena creato. Ciò permetterà di 'apply' la migrazione al database locale senza provare a ricreare tutte le tabelle e così via che esistano già.
3.  Eseguire la **Update-Database** comando nella Console di gestione pacchetti. In questo applicherà la migrazione InitialCreate al database. Dato che non contiene l'effettiva migrazione delle eventuali modiche (perché è commentato temporaneamente tali moduli), è sufficiente aggiungere una riga per il \_ \_MigrationsHistory tabella che indica che questa migrazione è già stata applicata.
4.  Rimuovere il commento il codice nel metodo di backup. Ciò significa che quando viene applicata questa migrazione nei database futuri, lo schema già presenti nel database locale verrà creato mediante le migrazioni.

## <a name="things-to-be-aware-of"></a>Aspetti da tenere presenti

Esistono alcuni aspetti da tenere presenti quando si usano le migrazioni da un database esistente.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>I nomi predefiniti/calcolato potrebbero non corrispondere schema esistente

Quando esegue scaffolding delle migrazioni di una, le migrazioni specifica in modo esplicito i nomi delle colonne e tabelle. Tuttavia, esistono altri oggetti di database che le migrazioni calcola un nome predefinito quando si applicano le migrazioni. Sono inclusi gli indici e vincoli di chiave esterna. Quando la destinazione è uno schema esistente, questi nomi calcolati potrebbero non corrispondere effettivamente ciò che esiste nel database.

Di seguito sono riportati alcuni esempi di quando è necessario essere a conoscenza di questo oggetto:

**Se è stato usato ' un'opzione: usare lo schema esistente come punto di partenza ' dal passaggio 3:**

-   Se le modifiche future nel modello richiedono la modifica o eliminazione di uno degli oggetti di database che ha un nome diverso, è necessario modificare la migrazione con scaffolding per specificare il nome corretto. Le API di migrazioni hanno un parametro Name facoltativo che consente di eseguire questa operazione.
    Ad esempio, lo schema esistente sia disponibile una tabella di Post con una colonna chiave esterna BlogId dotato di un indice denominato IndexFk\_BlogId. Tuttavia, per impostazione predefinita le migrazioni aspetta l'indice sia denominato IX\_BlogId. Se si apporta una modifica al modello che causa l'eliminazione dell'indice, è necessario modificare la chiamata DropIndex sottoposto a scaffolding per specificare il IndexFk\_BlogId nome.

**Se è stato usato ' opzione due: usare il database vuoto come punto di partenza ' dal passaggio 3:**

-   Tentativo di eseguire il metodo verso il basso della migrazione iniziale (che è, il ripristino a un database vuoto) nel database locale potrebbe non riuscire perché Migrations proverà a eliminare gli indici e vincoli di chiave esterna usando i nomi non corretti. Questo influirà solo il database locale poiché altri database verranno creati da zero usando il metodo di accesso della migrazione iniziale.
    Se si vuole effettuare il downgrade del database esistente locale a uno stato vuoto è più semplice eseguire questa operazione manualmente, mediante l'eliminazione del database o l'eliminazione di tutte le tabelle. Dopo il downgrade iniziale di che tutti gli oggetti di database verranno ricreati con i nomi predefiniti, quindi questo problema non presenterà stesso nuovamente.
-   Se le modifiche future nel modello richiedono la modifica o eliminazione di uno degli oggetti di database che ha un nome diverso, questo non funzionerà con il database locale esistente, poiché i nomi non corrispondano ai valori predefiniti. Tuttavia, funzionerà con i database creati 'da zero' poiché sono verranno usati i nomi predefiniti scelti per le migrazioni.
    È possibile apportare manualmente queste modifiche sul database esistente locale oppure è consigliabile che le migrazioni di ricreare il database da zero: quando si verifica in altri computer.
-   I database creati mediante il metodo di accesso del processo di migrazione iniziale possono differire leggermente dal database locale poiché i nomi predefiniti calcolati per gli indici e vincoli di chiave esterna verranno utilizzati. Potrebbero anche avere indici aggiuntivi come migrazioni creerà gli indici su colonne chiave esterna per impostazione predefinita, questo potrebbe non essere stata nel caso di database locale originale.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Non tutti gli oggetti di database sono rappresentati nel modello

Gli oggetti di database che non fanno parte del modello non verranno gestiti mediante le migrazioni. Può trattarsi di viste, stored procedure, le autorizzazioni, le tabelle che non fanno parte del modello, gli indici aggiuntivi, e così via.

Di seguito sono riportati alcuni esempi di quando è necessario essere a conoscenza di questo oggetto:

-   Indipendentemente dall'opzione scelto nel "Passaggio 3", se le modifiche future nel modello richiedono la modifica o eliminazione di questi oggetti aggiuntivi che non riconoscerà le migrazioni per apportare queste modifiche. Ad esempio, se si elimina una colonna che dispone di un indice aggiuntivo su di esso, le migrazioni non sa di eliminare l'indice. È necessario aggiungere manualmente questo schema per la migrazione con scaffolding.
-   Se è stato usato ' opzione due: usare il database vuoto come punto di partenza ', questi oggetti aggiuntivi non verrà creati dal metodo di accesso del processo di migrazione iniziale.
    È possibile modificare l'alto e verso il basso i metodi che si occupi di questi oggetti aggiuntivi se si desidera. Per gli oggetti che non sono supportati in modo nativo nell'API di migrazioni – ad esempio viste, è possibile usare la [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) metodo per l'esecuzione SQL non elaborate per creare/eliminare li.
