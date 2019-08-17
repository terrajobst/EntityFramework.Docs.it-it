---
title: Porting da EF6 a requisiti di convalida EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565354"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Prima di eseguire il porting da EF6 a EF Core: Convalidare i requisiti dell'applicazione

Prima di iniziare il processo di porting, è importante verificare che EF Core soddisfi i requisiti di accesso ai dati per l'applicazione.

## <a name="missing-features"></a>Funzionalità mancanti

Assicurarsi che EF Core disponga di tutte le funzionalità che è necessario usare nell'applicazione. Per un confronto dettagliato del modo in cui il set di funzionalità in EF Core viene confrontato con EF6, vedere [confronto delle funzionalità](../features.md) . Se mancano alcune funzionalità necessarie, assicurarsi che sia possibile compensare la mancanza di queste funzionalità prima di eseguire il porting a EF Core.

## <a name="behavior-changes"></a>Modifiche del comportamento

Si tratta di un elenco non esaustivo di alcune modifiche del comportamento tra EF6 e EF Core. È importante tenere presenti queste considerazioni come la porta dell'applicazione, in quanto possono cambiare il comportamento dell'applicazione, ma non verranno visualizzati come errori di compilazione dopo lo scambio in EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento di DbSet. Add/Connetti e Graph

In EF6 la chiamata `DbSet.Add()` a su un'entità comporta una ricerca ricorsiva di tutte le entità a cui viene fatto riferimento nelle proprietà di navigazione. Anche le entità trovate e non ancora rilevate dal contesto vengono contrassegnate come aggiunte. `DbSet.Attach()`si comporta allo stesso modo, ad eccezione del fatto che tutte le entità sono contrassegnate come invariate.

**EF Core esegue una ricerca ricorsiva simile, ma con alcune regole leggermente diverse.**

*  L'entità radice è sempre nello stato richiesto (aggiunta per `DbSet.Add` e senza modifiche per `DbSet.Attach`).

*  **Per le entità rilevate durante la ricerca ricorsiva delle proprietà di navigazione:**

    *  **Se la chiave primaria dell'entità è l'archivio generato**

        * Se la chiave primaria non è impostata su un valore, lo stato viene impostato su added. Il valore della chiave primaria viene considerato "non impostato" Se viene assegnato il valore predefinito CLR per il tipo di proprietà, ad esempio `0` per `int`, `null` per `string`e così via.

        * Se la chiave primaria è impostata su un valore, lo stato viene impostato su invariato.

    *  Se la chiave primaria non è generata dal database, l'entità viene inserita nello stesso stato della radice.

### <a name="code-first-database-initialization"></a>Inizializzazione del database Code First

**EF6 ha una notevole quantità di magie che esegue per selezionare la connessione al database e inizializzare il database. Di seguito sono riportate alcune di queste regole:**

* Se non viene eseguita alcuna configurazione, in EF6 verrà selezionato un database in SQL Express o nel database locale.

* Se una stringa di connessione con lo stesso nome del contesto si trova nel file `App/Web.config` di applicazioni, verrà utilizzata questa connessione.

* Se il database non esiste, viene creato.

* Se nessuna tabella del modello esiste nel database, lo schema per il modello corrente viene aggiunto al database. Se le migrazioni sono abilitate, verranno utilizzate per creare il database.

* Se il database esiste e EF6 lo schema è stato creato in precedenza, viene verificata la compatibilità dello schema con il modello corrente. Se il modello è stato modificato dopo la creazione dello schema, viene generata un'eccezione.

**EF Core non esegue questa operazione magica.**

* La connessione al database deve essere configurata in modo esplicito nel codice.

* Non viene eseguita alcuna inizializzazione. Per applicare le `DbContext.Database.Migrate()` migrazioni `DbContext.Database.EnsureCreated()` `EnsureDeleted()` o per creare o eliminare il database senza utilizzare le migrazioni, è necessario utilizzare.

### <a name="code-first-table-naming-convention"></a>Convenzione di denominazione della tabella Code First

EF6 esegue il nome della classe di entità tramite un servizio di pluralismo per calcolare il nome della tabella predefinito a cui è stato eseguito il mapping dell'entità.

EF Core usa il nome della `DbSet` proprietà in cui l'entità è esposta nel contesto derivato. Se l'entità non dispone di una `DbSet` proprietà, viene utilizzato il nome della classe.
