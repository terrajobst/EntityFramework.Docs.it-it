---
title: Pianificare Entity Framework Core 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136228"
---
# <a name="plan-for-entity-framework-core-50"></a>Pianificare Entity Framework Core 5.0

Come descritto nel processo di [pianificazione](../release-planning.md), abbiamo raccolto input dalle parti interessate in un piano provvisorio per la versione di EF Core 5.0.

> [!IMPORTANT] 
> Questo piano è ancora in corso. Niente qui è un impegno. Questo piano è un punto di partenza che si evolverà man mano che impariamo di più. Alcune cose attualmente non previste per 5.0 possono essere tirate dentro. Alcune cose attualmente pianificate per 5.0 potrebbero essere spuntate.

### <a name="version-number-and-release-date"></a>Numero di versione e data di rilascio.

EF Core 5.0 è attualmente pianificato per il rilascio [contemporaneamente a .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). La versione "5.0" è stata scelta per l'allineamento con .NET 5.0.

### <a name="supported-platforms"></a>Piattaforme supportate

EF Core 5.0 è pianificato per l'esecuzione su qualsiasi piattaforma .NET 5.0 basata sulla [convergenza di queste piattaforme in .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Ciò significa in termini di .NET Standard e il TFM effettivo utilizzato è ancora TBD.

EF Core 5.0 non verrà eseguito in .NET Framework.

### <a name="breaking-changes"></a>Modifiche di rilievo

EF Core 5.0 conterrà alcune modifiche di rilievo, ma questi saranno molto meno gravi rispetto a EF Core 3.0. Il nostro obiettivo è consentire alla maggior parte delle applicazioni di aggiornare senza rompersi.

Si prevede che ci saranno alcune modifiche di rilievo per i fornitori di database, soprattutto intorno al supporto TPT. Tuttavia, prevediamo che il lavoro per aggiornare un provider per 5.0 sarà inferiore a quello richiesto per l'aggiornamento per 3.0.

## <a name="themes"></a>Themes

Abbiamo estratto alcune aree o temi importanti che costituiranno la base per i grandi investimenti in EF Core 5.0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Proprietà di navigazione molti-a-molti (ovvero "ignora navigazione")

Sviluppatori principali: @smitpatel e@AndriySvyryd

Tracciato da [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Taglia t: L

Stato: In corso

Molti-a-molti è la [funzionalità più richiesta](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (407 voti) nel backlog di GitHub.

Il supporto per le relazioni molti-a-molti nella loro interezza viene tracciato come [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508). Questo può essere suddiviso in tre aree principali:

* Ignorare le proprietà di navigazione. Questi consentono di utilizzare il modello per le query e così via senza riferimento all'entità della tabella di join sottostante. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Tipi di entità property-bag. Questi consentono di utilizzare un tipo `Dictionary`CLR standard (ad esempio ) per le istanze di entità in modo che un tipo CLR esplicito non è necessario per ogni tipo di entità. (Allunga per la 5.0: [#9914.)](https://github.com/aspnet/EntityFrameworkCore/issues/9914)
* Lo zucchero per una facile configurazione delle relazioni molti-a-molti. (Allunga per 5.0.)

Riteniamo che il blocco più significativo per coloro che desiderano un supporto molti-a-molti sia non essere in grado di utilizzare le relazioni "naturali", senza fare riferimento alla tabella di join, nella logica di business, ad esempio le query. Il tipo di entità della tabella di join può esistere ancora, ma non deve ostacolarlo nella logica di business. Questo è il motivo per cui abbiamo scelto di affrontare saltare le proprietà di navigazione per 5.0.

In questo momento le altre parti di molti-a-molti vengono perseguite come obiettivo di estensione per EF Core 5.0. Ciò significa che non sono attualmente nel piano per 5.0, ma se le cose vanno bene speriamo di tirarli dentro.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapping di ereditarietà tabella per tipo (TPT)

Sviluppatore capo:@AndriySvyryd

Tracciato da [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Taglia t: XL

Stato: In corso

Stiamo facendo TPT perché è sia una funzionalità molto richiesta (254 voti; 3rd complessivo) e perché richiede alcune modifiche di basso livello che riteniamo appropriate per la natura fondamentale del piano .NET 5 complessivo. Prevediamo che questo si traduca in modifiche di rilievo per i provider di database, anche se queste dovrebbero essere molto meno gravi rispetto alle modifiche necessarie per 3.0.

## <a name="filtered-include"></a>Includi filtrato

Sviluppatore capo:@maumar

Tracciato da [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

T-shirt taglia: M

Stato: In corso

L'inclusione filtrata è una funzionalità molto richiesta (317 voti, 2a nel complesso) che non è una grande quantità di lavoro e che riteniamo che sbloccherà o renderà più semplici molti scenari che attualmente richiedono filtri a livello di modello o query più complesse.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Razionalizzare ToTable, ToQuery, ToView, FromSql e così via.

Sviluppatori principali: @maumar e@smitpatel

Tracciato da [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Taglia t: L

Stato: In corso

Nelle versioni precedenti sono stati compiuti progressi verso il supporto di SQL non elaborato, tipi senza chiave e aree correlate. Tuttavia, ci sono sia lacune che incoerenze nel modo in cui tutto funziona insieme nel suo complesso. L'obiettivo per la 5.0 è risolvere questi problemi e creare una buona esperienza per la definizione, la migrazione e l'utilizzo di diversi tipi di entità e delle query associate e degli elementi del database. Ciò può comportare anche aggiornamenti all'API di query compilata.

Si noti che questo elemento può comportare alcune modifiche di interruzione a livello di applicazione poiché alcune delle funzionalità attualmente disponibili sono troppo permissive in modo che possa portare rapidamente le persone in pozzi di errore. Probabilmente finiremo per bloccare alcune di queste funzionalità insieme a indicazioni su cosa fare invece.

## <a name="general-query-enhancements"></a>Miglioramenti generali delle query

Sviluppatori principali: @smitpatel e@maumar

Tracciato da [problemi `area-query` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Taglia t: XL

Stato: In corso

Il codice di conversione della query è stato ampiamente riscritto per EF Core 3.0.The query translation code was extensively rewritten for EF Core 3.0. Il codice di query è in genere in uno stato molto più affidabile a causa di questo. Per la 5.0 non è in programma di apportare modifiche importanti alle query, al di fuori di quelle necessarie per supportare TPT e ignorare le proprietà di navigazione. Tuttavia, è ancora necessario lavorare in modo significativo per fissare alcuni debiti tecnici rimasti dalla revisione del 3.0. Si prevede inoltre di correggere molti bug e implementare piccoli miglioramenti per migliorare ulteriormente l'esperienza complessiva delle query.

## <a name="migrations-and-deployment-experience"></a>Migrazioni ed esperienza di distribuzione

Sviluppatori principali:@bricelam

Tracciato da [#19587](https://github.com/dotnet/efcore/issues/19587)

Taglia t: L

Stato: In corso

Attualmente, molti sviluppatori eseguire la migrazione dei database in fase di avvio dell'applicazione. Questo è facile, ma non è raccomandato perché:

* Più thread/processi/server possono tentare di eseguire contemporaneamente la migrazione del database
* Le applicazioni possono tentare di accedere a uno stato incoerente mentre questo accade
* In genere le autorizzazioni del database per modificare lo schema non devono essere concesse per l'esecuzione dell'applicazione
* E 'difficile tornare a uno stato pulito se qualcosa va storto

Vogliamo offrire un'esperienza migliore in questo caso che consenta un modo semplice per eseguire la migrazione del database in fase di distribuzione. Questo dovrebbe:

* Lavorare su Linux, Mac e Windows
* Essere una buona esperienza nella riga di comando
* Scenari di supporto con i contenitoriSupport scenarios with containers
* Utilizzare flussi/flussi di distribuzione reali di uso comune
* Integrazione in almeno Visual Studio

Il risultato è probabilmente molti piccoli miglioramenti in EF Core (ad esempio, migliori migrazioni su SQLite), insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre solo EF.

## <a name="ef-core-platforms-experience"></a>Esperienza nelle piattaforme EF Core 

Sviluppatori principali: @roji e@bricelam

Tracciato da [#19588](https://github.com/dotnet/efcore/issues/19588)

Taglia t: L

Stato: Non avviato

Sono disponibili indicazioni utili per l'utilizzo di EF Core nelle applicazioni Web tradizionali di tipo MVC. Linee guida per altre piattaforme e modelli di applicazione mancanti o non aggiornate. Per EF Core 5.0, si prevede di analizzare, migliorare e documentare l'esperienza di utilizzo di EF Core con:

* Blazor
* Xamarin, incluso l'utilizzo della storia AOT/linker
* WinForms/WPF/WinUI ed eventualmente altri U.I. frameworks

Questo è probabile che si tratti di molti piccoli miglioramenti in EF Core, insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre il solo EF.

Le aree specifiche che prevediamo di esaminare sono:

* Distribuzione, inclusa l'esperienza per l'utilizzo di strumenti di EF, ad esempio per le migrazioni
* Modelli applicativi, tra cui Xamarin e Blazor, e probabilmente altri
* Esperienze SQLite, tra cui l'esperienza spaziale e le ricostruzioni delle tabelle
* AOT e esperienze di collegamento
* Integrazione diagnostica, inclusi i contatori perf

## <a name="performance"></a>Prestazioni

Sviluppatore capo:@roji

Tracciato da [problemi `area-perf` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Taglia t: L

Stato: In corso

Per EF Core, si prevede di migliorare la nostra suite di benchmark delle prestazioni e apportare miglioramenti diretti delle prestazioni al runtime. Inoltre, prevediamo di completare la nuova API di batch ADO.NET con prototipo durante il ciclo di rilascio 3.0. Anche a livello di ADO.NET, si pianificano ulteriori miglioramenti delle prestazioni per il provider Npgsql.

Come parte di questo lavoro si prevede anche di aggiungere i contatori delle prestazioni di ADO.NET/EF Core e altre funzionalità di diagnostica in base alle esigenze.

## <a name="architecturalcontributor-documentation"></a>Documentazione architettonica/collaboratore

Documentatore principale:@ajcvickers

Tracciato da [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Taglia t: L

Stato: In corso

L'idea qui è quello di rendere più facile capire cosa sta succedendo negli elementi interni di EF Core. Questo può essere utile a chiunque utilizzi EF Core, ma la motivazione principale è quello di rendere più semplice per gli utenti esterni:This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:

* Contribuire al codice di base di EFContribute to the EF Core code
* Creare provider di databaseCreate database providers
* Creare altre estensioni

## <a name="microsoftdatasqlite-documentation"></a>Documentazione di Microsoft.Data.Sqlite

Documentatore principale:@bricelam

Tracciato da [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

T-shirt taglia: M

Stato: Completato. La nuova documentazione è [disponibile nel sito Microsoft Docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

Il team di Entity Framework è inoltre proprietario del provider di ADO.NET Microsoft.Data.Sqlite. Prevediamo di documentare completamente questo provider come parte della versione 5.0.

## <a name="general-documentation"></a>Documentazione generale

Documentatore principale:@ajcvickers

Tracciata dai [problemi nel repository dei documenti nell'attività cardine 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Taglia t: L

Stato: In corso

È già in corso l'aggiornamento della documentazione per le versioni 3.0 e 3.1. Stiamo anche lavorando su:
  * Una revisione della documentazione introduttiva per renderli più accessibile/più facili da seguire
  * Riorganizzazione dei documenti per semplificare la ricerca e aggiungere riferimenti incrociati
  * Aggiunta di ulteriori dettagli e chiarimenti ai documenti esistenti
  * Aggiornamento degli esempi e aggiunta di altri esempi

## <a name="fixing-bugs"></a>Correzione di bug

Tracciato da [problemi `type-bug` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Sviluppatori: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , , ,@ajcvickers

Taglia t: L

Stato: In corso

Al momento della scrittura, abbiamo 135 bug triati per essere corretti nella versione 5.0 (con 62 già risolti), ma c'è una sovrapposizione significativa con la sezione Miglioramenti delle _query generali_ sopra.

La tariffa in entrata (problemi che finiscono come lavoro in una pietra miliare) è stata di circa 23 numeri al mese nel corso della versione 3.0. Non tutti questi dovranno essere risolti in 5.0. Come stima approssimativa abbiamo intenzione di risolvere altri 150 problemi nell'intervallo di tempo 5.0.

## <a name="small-enhancements"></a>Piccoli miglioramenti

Tracciato da [problemi `type-enhancement` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Sviluppatori: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , , ,@ajcvickers

Taglia t: L

Stato: In corso

Oltre alle caratteristiche più grandi descritte in precedenza, abbiamo anche molti piccoli miglioramenti programmati per la 5.0 per risolvere i "tagli di carta". Si noti che molti di questi miglioramenti sono trattati anche dai temi più generali descritti in precedenza.

## <a name="below-the-line"></a>Sotto la linea

Tracciata da [problemi `consider-for-next-release` etichettati con](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Si tratta di correzioni di bug e miglioramenti che **non** sono attualmente pianificati per la versione 5.0, ma esamineremo come obiettivi di stretching a seconda dei progressi compiuti sul lavoro di cui sopra.

Inoltre, consideriamo sempre le [questioni più votate](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante la pianificazione. Tagliare uno di questi problemi da un rilascio è sempre doloroso, ma abbiamo bisogno di un piano realistico per le risorse che abbiamo.

## <a name="feedback"></a>Commenti e suggerimenti

I commenti e i suggerimenti dei clienti sulla pianificazione sono importanti. Il modo migliore per indicare l'importanza di un problema consiste nel votare (Pollice in su) per tale problema in GitHub. Questi dati verranno quindi inseriti nel processo di [pianificazione](../release-planning.md) per la versione successiva.
