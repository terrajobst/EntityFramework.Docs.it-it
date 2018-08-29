---
title: 'Conversione da EF6 a EF Core: convalidare i requisiti'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993440"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Prima della conversione da EF6 a EF Core: convalidare i requisiti dell'applicazione

Prima di iniziare il processo di porting è importante convalidare che EF Core soddisfi i requisiti di accesso di dati per l'applicazione.

## <a name="missing-features"></a>Funzionalità mancanti

Assicurarsi che EF Core include tutte le funzionalità da usare nell'applicazione. Visualizzare [confronto tra le funzionalità](../features.md) per un confronto dettagliato delle modalità con cui confrontare di funzionalità disponibili in EF Core per Entity Framework 6. Se tutte le funzionalità necessarie sono mancante, verificare che è possibile compensare la mancanza di queste funzionalità prima della conversione a EF Core.

## <a name="behavior-changes"></a>Modifiche del comportamento

Si tratta di un elenco non esaustivo di alcuni cambiamenti nel comportamento tra EF6 ed EF Core. È importante tenere presente quanto segue come la porta dell'applicazione perché queste potrebbero cambiare il modo l'applicazione si comporta, ma non verrà visualizzata come errori di compilazione dopo lo scambio in EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento DbSet.Add/Attach e un grafo

In EF6, chiamata `DbSet.Add()` ai risultati entità in una ricerca ricorsiva per tutte le entità a cui fa riferimento la proprietà di navigazione. Qualsiasi entità che vengono individuati e non sono già rilevato dal contesto, sono anche essere contrassegnato come aggiunto. `DbSet.Attach()` si comporta, ad eccezione del fatto tutte le entità sono contrassegnate come non modificato.

**EF Core esegue una ricerca ricorsiva simile, ma con alcune regole leggermente diverse.**

*  L'entità radice è sempre nello stato richiesto (aggiunte per `DbSet.Add` e invariato per `DbSet.Attach`).

*  **Per le entità che si verificano durante la ricerca ricorsiva delle proprietà di navigazione:**

    *  **Se la chiave primaria dell'entità archivio generato**

        * Se la chiave primaria non è impostata su un valore, lo stato viene impostato a aggiunto. Il valore della chiave primaria viene considerato "non impostato" Se viene assegnato il valore predefinito CLR per il tipo di proprietà (ad esempio, `0` per `int`, `null` per `string`e così via.).

        * Se la chiave primaria è impostata su un valore, lo stato è impostato su non modificato.

    *  Se la chiave primaria non è generato dal database, l'entità viene inserita nello stesso stato come radice.

### <a name="code-first-database-initialization"></a>Inizializzazione del database prima del codice

**EF6 è una quantità significativa di magic che esegue tutto selezionando la connessione al database e l'inizializzazione del database. Alcune di queste regole includono:**

* Se non viene eseguita alcuna configurazione, Entity Framework 6 selezionerà un database in SQL Express o LocalDb.

* Se è una stringa di connessione con lo stesso nome di contesto nelle applicazioni `App/Web.config` file, verrà utilizzata questa connessione.

* Se il database non esiste, viene creato.

* Se nessuna delle tabelle dal modello esiste nel database, lo schema per il modello corrente viene aggiunto al database. Se sono abilitate le migrazioni, quindi vengono utilizzati per creare il database.

* Se il database esista ed EF6 è stata creata in precedenza lo schema, lo schema viene verificato la compatibilità con il modello corrente. Il modello è stato modificato dopo lo schema è stato creato, viene generata un'eccezione.

**EF Core non esegue alcun questo speciale.**

* La connessione al database deve essere configurata in modo esplicito nel codice.

* Viene eseguita alcuna inizializzazione. È necessario utilizzare `DbContext.Database.Migrate()` per applicare le migrazioni (o `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` per creare o eliminare il database senza usare le migrazioni).

### <a name="code-first-table-naming-convention"></a>Prima tabella codice convenzione di denominazione

EF6 viene eseguito il nome di classe di entità tramite un servizio di pluralizzazione per la quale calcolare il nome di tabella predefinito che l'entità è mappata a.

EF Core Usa il nome del `DbSet` proprietà dell'entità esposta nel contesto derivato. Se l'entità non dispone di un `DbSet` proprietà, quindi il nome della classe viene utilizzata.
