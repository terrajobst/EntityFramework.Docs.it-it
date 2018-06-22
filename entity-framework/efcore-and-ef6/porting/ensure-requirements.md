---
title: Porting da EF6 a EF Core - convalida i requisiti
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052861"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Prima di porting da EF6 a EF Core: convalidare i requisiti dell'applicazione

Prima di iniziare il processo di porting è importante convalidare che EF Core soddisfi i requisiti di accesso ai dati per l'applicazione.

## <a name="missing-features"></a>Funzionalità mancanti

Assicurarsi che EF Core offre tutte le funzionalità che si desidera utilizzare nell'applicazione. Vedere [confronto delle funzionalità](../features.md) per un confronto dettagliato di come il set di caratteristiche nella EF Core confronta con EF6. Se tutte le funzionalità necessarie sono mancante, verificare che per compensare la mancanza di queste funzionalità prima di porting a EF Core.

## <a name="behavior-changes"></a>Modifiche del comportamento

Questo è un elenco non esaustivo di alcune modifiche di comportamento tra EF6 e Core di Entity Framework. È importante tenere presente quanto segue come la porta dell'applicazione perché queste potrebbero cambiare la modalità di applicazione si comporta, ma non verrà visualizzata come errori di compilazione dopo lo scambio di EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento DbSet.Add/Attach e di grafico

In EF6, la chiamata `DbSet.Add()` su una ricerca ricorsiva di tutte le entità a cui fa riferimento nelle proprietà di navigazione, viene generata un'entità. Le entità che vengono individuate e non sono già rilevate dal contesto, sono anche essere contrassegnato come aggiunto. `DbSet.Attach()`si comporta allo stesso, ad eccezione del fatto tutte le entità sono contrassegnate come non modificato.

**Core EF esegue una ricerca ricorsiva simili, ma con alcune regole leggermente diverse.**

*  L'entità principale è sempre lo stato richiesto (aggiunto per `DbSet.Add` e invariato per `DbSet.Attach`).

*  **Per le entità che si verificano durante la ricerca ricorsiva di proprietà di navigazione:**

    *  **Se la chiave primaria dell'entità archivio generato**

        * Se la chiave primaria non è impostata su un valore, lo stato è impostato a aggiunto. Il valore della chiave primario viene considerato "non impostata" se è stato assegnato il valore predefinito CLR per il tipo di proprietà (ad esempio `0` per `int`, `null` per `string`, ecc.).

        * Se la chiave primaria è impostata su un valore, lo stato è impostato su non modificato.

    *  Se la chiave primaria non è generato dal database, l'entità viene inserito nello stesso stato come radice.

### <a name="code-first-database-initialization"></a>Inizializzazione del database prima del codice

**EF6 dispone di una quantità significativa di magic che esegue intorno selezionando la connessione al database e l'inizializzazione del database. Alcune di queste regole includono:**

* Se non viene eseguita alcuna configurazione, EF6 selezionerà un database in SQL Express o LocalDb.

* Se è una stringa di connessione con lo stesso nome di contesto nelle applicazioni `App/Web.config` file, verrà utilizzata questa connessione.

* Se il database non esiste, viene creato.

* Se nessuna delle tabelle dal modello esiste nel database, lo schema per il modello corrente viene aggiunto al database. Se sono abilitate le migrazioni, quindi vengono utilizzati per creare il database.

* Se il database esiste ed EF6 in precedenza era stato creato lo schema, lo schema viene verificato la compatibilità con il modello corrente. Il modello è stato modificato dopo che è stato creato lo schema, viene generata un'eccezione.

**Core EF non esegue questa attività.**

* La connessione al database deve essere configurata in modo esplicito nel codice.

* Viene eseguita alcuna inizializzazione. È necessario utilizzare `DbContext.Database.Migrate()` per applicare le migrazioni (o `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` per creare o eliminare il database senza utilizzare le migrazioni).

### <a name="code-first-table-naming-convention"></a>Prima tabella codice convenzione di denominazione

EF6, il nome di classe di entità viene eseguito tramite un servizio di pluralizzazione per calcolare il nome di tabella predefinito che viene eseguito il mapping di entità a.

EF Core viene utilizzato il nome del `DbSet` proprietà che viene esposta l'entità al contesto derivata. Se l'entità non dispone di un `DbSet` proprietà, quindi il nome della classe viene utilizzata.
