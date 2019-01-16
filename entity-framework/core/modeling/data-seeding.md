---
title: Seeding dei dati - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 1c450b142573368d043430f55a3144b6696a8691
ms.sourcegitcommit: b4a5ed177b86bf7f81602106dab6b4acc18dfc18
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2019
ms.locfileid: "54316634"
---
# <a name="data-seeding"></a>Seeding dei dati

Seeding dei dati è il processo di popolamento di un database con un set di dati iniziale.

Esistono diversi modi a questo scopo in EF Core:
* Dati del valore di inizializzazione modello
* Personalizzazione della migrazione manuale
* Per la logica l'inizializzazione personalizzata

## <a name="model-seed-data"></a>Dati del valore di inizializzazione modello

> [!NOTE]
> Questa funzionalità è stata introdotta in EF Core 2.1.

Diversamente da ef6, in EF Core, il seeding dei dati può essere associato a un tipo di entità come parte della configurazione del modello. Quindi Entity Framework Core [migrazioni](xref:core/managing-schemas/migrations/index) può calcolare automaticamente quali inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.

> [!NOTE]
> Le migrazioni considera solo le modifiche al modello quando si determina quale operazione deve essere eseguita per ottenere i dati di seeding, lo stato desiderato. In questo modo tutte le modifiche ai dati eseguite di fuori di migrazioni potrebbero andare persi o provocare un errore.

Ad esempio, si configurerà i dati di inizializzazione per un `Blog` in `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Per aggiungere entità contenenti una relazione di valori di chiave esterna devono essere specificate:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Se il tipo di entità dispone di qualsiasi proprietà in una classe anonima può essere utilizzata per fornire i valori dello stato di ombreggiatura:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Proprietà dell'entità di tipi possono essere inizializzati in modo analogo:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) per altre informazioni sul contesto.

Dopo aver aggiunto i dati al modello [migrazioni](xref:core/managing-schemas/migrations/index) deve essere usato per applicare le modifiche.

> [!TIP]
> Se è necessario applicare le migrazioni come parte di una distribuzione automatizzata è possibile [creare uno script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) che può essere visualizzato in anteprima prima dell'esecuzione.

In alternativa, è possibile usare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di seeding, ad esempio per un database di test o quando si utilizza il provider in memoria o in qualsiasi database di non-relazione. Si noti che se il database esiste già, `EnsureCreated()` aggiornerà né i dati dello schema o valore di inizializzazione nel database. Per i database relazionali non devono chiamare `EnsureCreated()` se si prevede di usare le migrazioni.

### <a name="limitations-of-model-seed-data"></a>Limitazioni dei dati del valore di inizializzazione modello

Questo tipo di dati di seeding viene gestito mediante le migrazioni e lo script per aggiornare i dati che si trova già nel database deve essere generato senza stabilire la connessione al database. Ciò impone alcune limitazioni:
* Il valore della chiave primaria deve essere specificata anche se in genere generato dal database. Da utilizzare per rilevare le modifiche dei dati tra le migrazioni.
* In precedenza verranno rimosso dei dati di seeding se la chiave primaria viene modificata in alcun modo.

Pertanto questa funzionalità è particolarmente utile per i dati statici che non sono prevista la modifica di fuori di migrazioni e non dipendono da altre operazioni nel database, ad esempio i codici postali ZIP.

Se lo scenario include i seguenti, è consigliabile usare la logica di inizializzazione personalizzato descritta nell'ultima sezione:
* Dati temporanei per il test
* Dati che dipende dallo stato del database
* Dati che richiede valori di chiave generato dal database, incluse le entità che usano le chiavi alternative come identità
* I dati che richiede la trasformazione personalizzati (che non è gestita da [valore conversioni](xref:core/modeling/value-conversions)), ad esempio alcuni hashing della password
* Dati che richiedono le chiamate all'API esterna, ad esempio la creazione di utenti e ruoli di ASP.NET Core Identity

## <a name="manual-migration-customization"></a>Personalizzazione della migrazione manuale

Quando viene aggiunta le modifiche ai dati specificati con una migrazione `HasData` vengono trasformate in chiamate a `InsertData()`, `UpdateData()`, e `DeleteData()`. Un modo per aggirare alcune delle limitazioni della `HasData` consiste nell'aggiungere manualmente queste chiamate oppure [operazioni personalizzate](xref:core/managing-schemas/migrations/operations) per la migrazione invece.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Per la logica l'inizializzazione personalizzata

Un modo semplice e potente per eseguire il seeding dei dati consiste nell'utilizzare [ `DbContext.SaveChanges()` ](xref:core/saving/index) prima che l'applicazione principale per la logica inizia l'esecuzione.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Il codice di seeding non deve far parte dell'esecuzione normale app perché ciò può causare problemi di concorrenza quando sono in esecuzione e l'app con l'autorizzazione per modificare lo schema del database può richiedere anche più istanze.

In base ai vincoli della distribuzione è possibile eseguire il codice di inizializzazione in modi diversi:
* Esecuzione locale dell'app di inizializzazione
* Distribuzione dell'app di inizializzazione con l'app principale, chiamata di routine di inizializzazione e la disabilitazione o la rimozione dell'app di inizializzazione.

Ciò in genere può essere automatizzata mediante [profili di pubblicazione](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
