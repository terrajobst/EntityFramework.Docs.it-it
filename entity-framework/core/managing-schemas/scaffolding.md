---
title: Reverse Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688680"
---
# <a name="reverse-engineering"></a>Reverse Engineering

Decodifica è il processo di scaffolding classi del tipo di entità e una classe DbContext basati su uno schema di database. Può essere eseguita usando il `Scaffold-DbContext` comando degli strumenti di Entity Framework Core Package Manager Console (console di gestione pacchetti) o `dotnet ef dbcontext scaffold` comando degli strumenti dell'interfaccia della riga di comando (CLI di .NET).

## <a name="installing"></a>Installazione di

Prima la decompilazione, è necessario eseguire l'installazione di [strumenti console di gestione pacchetti](xref:core/miscellaneous/cli/powershell) (solo Visual Studio) o il [gli strumenti CLI](xref:core/miscellaneous/cli/dotnet). Vedere i collegamenti per informazioni dettagliate.

È necessario anche installare appropriata [provider di database](xref:core/providers/index) per lo schema del database che si desidera decodificare.

## <a name="connection-string"></a>Stringa di connessione

Il primo argomento per il comando è una stringa di connessione al database. Gli strumenti userà questa stringa di connessione per leggere lo schema del database.

Come utilizzare le virgolette ed escape alla stringa di connessione dipende dal quale shell in uso per eseguire il comando. Fare riferimento alla documentazione della shell per le specifiche. Ad esempio, PowerShell richiede di eseguire l'escape di `$` carattere, ma non `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configurazione e segreti utente

Se si dispone di un progetto ASP.NET Core, è possibile usare il `Name=<connection-string>` sintassi per leggere la stringa di connessione dalla configurazione.

Questo funziona bene con la [strumento Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) separare la password del database dalla codebase.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nome del provider

Il secondo argomento è il nome del provider. Il nome del provider è in genere lo stesso nome di pacchetto NuGet del provider.

## <a name="specifying-tables"></a>Specifica le tabelle

Tutte le tabelle nello schema del database vengono decodificati in tipi di entità per impostazione predefinita. È possibile limitare le tabelle che vengono decodificati specificando gli schemi e tabelle.

Il `-Schemas` parametro nella console di gestione pacchetti e `--schema` opzione della riga di comando può essere usata per includere tutte le tabelle all'interno di uno schema.

`-Tables` (PMC) e `--table` (CLI) può essere utilizzato per includere tabelle specifiche.

Per includere più tabelle nella console di gestione pacchetti, usare una matrice.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Per includere più tabelle della riga di comando, specificare l'opzione più volte.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Nomi di mantenimento

I nomi di tabella e colonna sono fisse per adattarlo meglio le convenzioni di denominazione .NET per i tipi e le proprietà per impostazione predefinita. Specificando il `-UseDatabaseNames` console di gestione pacchetti di attivazione o la `--use-database-names` opzione della riga di comando disabiliterà il problema mantenendo quanto più possibile i nomi di database originali. Verranno risolti ancora identificatori .NET non è validi e nomi sintetizzati come proprietà di navigazione saranno ancora conforme alle convenzioni di denominazione .NET.

## <a name="fluent-api-or-data-annotations"></a>API Fluent o annotazioni dei dati

Per impostazione predefinita, i tipi di entità sono configurati con l'API Fluent. Specificare `-DataAnnotations` (console di gestione pacchetti) o `--data-annotations` (CLI) per usare invece le annotazioni dei dati quando possibile.

Ad esempio, tramite l'API Fluent eseguirà lo scaffolding di questo.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Durante l'utilizzo di annotazioni dei dati eseguirà lo scaffolding questo.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nome oggetto DbContext

Il nome della classe DbContext sottoposto a scaffolding sarà il nome del database con suffisso *contesto* per impostazione predefinita. Per specificare una diversa, usare `-Context` nella console di gestione pacchetti e `--context` della riga di comando.

## <a name="directories-and-namespaces"></a>Le directory e gli spazi dei nomi

Le classi di entità e una classe DbContext è sottoposto a scaffolding nella directory radice del progetto e usare lo spazio dei nomi predefinito del progetto. È possibile specificare la directory in cui vengano eseguito lo scaffolding di classi usando `-OutputDir` (console di gestione pacchetti) o `--output-dir` (CLI). Lo spazio dei nomi sarà lo spazio dei nomi radice e i nomi di tutte le sottodirectory nella directory radice del progetto.

È anche possibile usare `-ContextDir` (console di gestione pacchetti) e `--context-dir` (CLI) per eseguire lo scaffolding della classe DbContext in una directory diversa dalle classi di tipo di entità.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Come funziona

Verrà avviata la decompilazione leggendo lo schema del database. Legge le informazioni sulle tabelle, colonne, vincoli e indici.

Successivamente, Usa le informazioni sullo schema per creare un modello di EF Core. Le tabelle vengono utilizzate per creare tipi di entità. le colonne vengono utilizzate per creare le proprietà; e le chiavi esterne vengono utilizzate per creare relazioni.

Infine, il modello viene utilizzato per generare il codice. Il corrispondente entità dati classi e API Fluent annotazioni del tipo sono sottoposto a scaffolding per ricreare lo stesso modello dalla propria app.

## <a name="what-doesnt-work"></a>Cosa non funziona

Non tutti gli aspetti relativi a un modello possono essere rappresentati usando uno schema di database. Ad esempio, le informazioni sulle **gerarchie di ereditarietà**, **tipi di proprietà**, e **tabella suddivisione** non sono presenti nello schema del database. Per questo motivo, questi costrutti non verrà mai essere decodificata.

È inoltre **alcuni tipi di colonna** potrebbero non essere supportate dal provider di EF Core. Queste colonne non verranno incluse nel modello.

EF Core richiede ogni tipo di entità dispone di una chiave. Le tabelle, tuttavia, non sono necessari per specificare una chiave primaria. **Le tabelle senza una chiave primaria** attualmente non vengono decodificati.

È possibile definire **token di concorrenza** in un modello di EF Core per evitare che due utenti di aggiornare la stessa entità contemporaneamente. Alcuni database dispongono di un tipo speciale per rappresentare questo tipo di colonna (ad esempio, rowversion in SQL Server) in questo caso è possibile invertire progettare queste informazioni. Tuttavia, altri token di concorrenza saranno non essere decodificato.

## <a name="customizing-the-model"></a>Personalizzazione del modello

Il codice generato da EF Core è il codice. È possibile modificarlo. Verrà rigenerato solo se si decodifica lo stesso modello anche in questo caso. Rappresenta il codice con scaffolding *uno* modello che può essere utilizzato per accedere al database, ma sicuramente non è la *solo* modello che può essere utilizzato.

Personalizzare le classi del tipo di entità e classe DbContext in base alle esigenze. Ad esempio, è possibile scegliere di rinominare i tipi e le proprietà, introdurre le gerarchie di ereditarietà, o dividere una tabella in più entità. È anche possibile rimuovere gli indici non univoci, sequenze inutilizzate e le proprietà di navigazione, facoltativo delle proprietà scalari e nomi di vincoli dal modello.

È anche possibile aggiungere altri costruttori, metodi, proprietà, e così via. utilizzo di un'altra classe parziale in un file separato. Questo approccio funziona anche quando si intende decompilare il modello di nuovo.

## <a name="updating-the-model"></a>L'aggiornamento del modello

Dopo aver apportato modifiche al database, si potrebbe essere necessario aggiornare il modello di EF Core per riflettere tali modifiche. Se le modifiche del database sono semplici, potrebbe essere più semplice apportare manualmente le modifiche al modello di EF Core. Ad esempio, la ridenominazione di una tabella o colonna, rimuovere una colonna o si aggiorna tipo di una colonna è semplici modifiche da apportare nel codice.

Altre modifiche significative, tuttavia, non sono come facile apportare manualmente. Un flusso di lavoro comune è quello inverso progettare il modello dal database di utilizzando nuovamente `-Force` (console di gestione pacchetti) o `--force` (CLI) per sovrascrivere il modello esistente con una aggiornata.

Un'altra funzionalità comunemente richiesta è la possibilità di aggiornare il modello dal database mantenendo la personalizzazione, ad esempio le operazioni di ridenominazione, le gerarchie dei tipi e così via. Usare problema [831 #](https://github.com/aspnet/EntityFrameworkCore/issues/831) per tenere traccia dell'avanzamento di questa funzionalità.

> [!WARNING]
> Se il modello dal database la decodificazione anche in questo caso, eventuali modifiche apportate ai file andranno perse.
