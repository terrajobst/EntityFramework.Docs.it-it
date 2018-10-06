---
title: EF Core riferimenti per gli strumenti (Console di gestione pacchetti) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 9a57b58f8569ee1241e40c3809b03487d1d88e02
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834760"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Riferimenti - Console di gestione pacchetti in Visual Studio agli strumenti di Entity Framework Core

Gli strumenti di Package Manager Console (console di gestione pacchetti) per Entity Framework Core eseguono attività di sviluppo in fase di progettazione. Ad esempio, creano [migrazioni](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), applicare le migrazioni e generare il codice per un modello basato su un database esistente. I comandi vengono eseguiti all'interno di Visual Studio usando il [Console di gestione pacchetti](/nuget/tools/package-manager-console). Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.

Se non si usa Visual Studio, è consigliabile la [degli strumenti della riga di comando di EF Core](dotnet.md) invece. Gli strumenti dell'interfaccia della riga sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.

## <a name="installing-the-tools"></a>Installazione degli strumenti

Le procedure per l'installazione e gli strumenti di aggiornamento differiscono tra ASP.NET Core 2.1 e versioni precedenti o altri tipi di progetto.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core 2.1 e versioni successive

Gli strumenti sono inclusi automaticamente in un progetto ASP.NET Core 2.1 + in quanto il `Microsoft.EntityFrameworkCore.Tools` incluso nel pacchetto la [Microsoft.AspNetCore.App metapacchetto](/aspnet/core/fundamentals/metapackage-app).

Pertanto, non devi eseguire alcuna operazione per installare gli strumenti, ma è necessario:
* Ripristinare i pacchetti prima di usare gli strumenti in un nuovo progetto.
* Installare un pacchetto per aggiornare gli strumenti a una versione più recente.

Per essere certi di ottenere la versione più recente degli strumenti, è consigliabile eseguire anche il passaggio seguente:

* Modificare il *csproj* del file e aggiungere una riga che specifica la versione più recente del [entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pacchetto. Ad esempio, il *csproj* può includere file un `ItemGroup` simile alla seguente:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aggiornare gli strumenti quando viene visualizzato un messaggio simile al seguente:

> La versione degli strumenti di Entity Framework Core '2.1.1-rtm-30846' è precedente a quella del runtime di '2.1.3-rtm-32065'. Aggiornare gli strumenti per le ultime funzionalità e correzioni di bug.

Per aggiornare gli strumenti:
* Installare la versione più recente di .NET Core SDK.
* Aggiornare Visual Studio alla versione più recente.
* Modificare il *file con estensione csproj* file in modo che includa un riferimento al pacchetto per il pacchetto di strumenti più recente, come illustrato in precedenza.

### <a name="other-versions-and-project-types"></a>Le altre versioni e i tipi di progetto

Installare gli strumenti della Console di gestione pacchetti eseguendo il comando seguente nella **Console di gestione pacchetti**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aggiornare gli strumenti eseguendo il comando seguente nella **Console di gestione pacchetti**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Verificare l'installazione

Verificare che gli strumenti vengono installati eseguendo questo comando:

``` powershell
Get-Help about_EntityFrameworkCore
```

L'output è simile al seguente (non consente di capire quale versione degli strumenti in uso):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Usando gli strumenti

Prima di usare gli strumenti:
* Comprendere la differenza tra progetti di avvio e di destinazione.
* Informazioni su come usare gli strumenti con le librerie di classi .NET Standard.
* Per i progetti ASP.NET Core, impostare l'ambiente.

### <a name="target-and-startup-project"></a>Progetto di avvio e di destinazione

I comandi di fare riferimento a un *project* e una *progetto di avvio*.

* Il *project* è noto anche come il *progetto di destinazione* perché è in cui i comandi aggiungono o rimuovono file. Per impostazione predefinita, il **progetto predefinito** selezionato in **Console di gestione pacchetti** è il progetto di destinazione. È possibile specificare un altro progetto come progetto di destinazione usando il <nobr> `--project` </nobr> opzione.

* Il *progetto di avvio* è quello che gli strumenti di compilare ed eseguire. Gli strumenti richiede l'esecuzione di codice dell'applicazione in fase di progettazione per ottenere informazioni sul progetto, ad esempio la stringa di connessione di database e la configurazione del modello. Per impostazione predefinita, il **progetto di avvio** nelle **Esplora soluzioni** è il progetto di avvio. È possibile specificare un altro progetto come progetto di avvio usando il <nobr> `--startup-project` </nobr> opzione.

Il progetto di avvio e di un progetto di destinazione sono spesso dello stesso progetto. Uno scenario tipico in cui sono progetti separati è quando:

* Le classi di contesto ed entità di Entity Framework Core sono in una libreria di classi .NET Core.
* Un'app console .NET Core o un'app web fa riferimento alla libreria di classi.

È anche possibile [inserire il codice di migrazioni in una libreria di classi separata dal contesto di Entity Framework Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Altri framework di destinazione

Gli strumenti della Console di gestione pacchetti funzionano con i progetti .NET Core o .NET Framework. Le app con il modello di EF Core in una libreria di classi .NET Standard potrebbero non avere un progetto di .NET Framework o .NET Core. Ad esempio, questo vale delle App Xamarin e (Universal Windows Platform). In questi casi, è possibile creare un progetto di app console .NET Core o .NET Framework il cui unico scopo è agire come progetto di avvio per gli strumenti. Il progetto può essere un progetto fittizio senza vero codice &mdash; , è necessario soltanto per fornire una destinazione per gli strumenti.

Perché è un progetto fittizio necessario? Come accennato in precedenza, gli strumenti di dovranno eseguire il codice dell'applicazione in fase di progettazione. A tale scopo, è necessario usare il runtime di .NET Core o .NET Framework. Quando il modello di EF Core è in un progetto destinato a .NET Core o .NET Framework, gli strumenti di Entity Framework Core presi in prestito il runtime dal progetto. Che non possono eseguire se il modello di EF Core è in una libreria di classi .NET Standard. .NET Standard non è un'implementazione .NET effettiva; si tratta di una specifica di un set di API che le implementazioni di .NET devono supportare. .NET Standard non è pertanto sufficiente per gli strumenti di Entity Framework Core eseguire il codice dell'applicazione. Il progetto fittizio creato per essere utilizzato come progetto di avvio fornisce una piattaforma di destinazione concreta in cui gli strumenti possono caricare la libreria di classi .NET Standard.

### <a name="aspnet-core-environment"></a>Ambiente ASP.NET Core

Per specificare l'ambiente per i progetti ASP.NET Core, impostare **env:ASPNETCORE_ENVIRONMENT** prima di eseguire comandi.

## <a name="common-parameters"></a>Parametri comuni

Nella tabella seguente vengono illustrati i parametri che sono comuni a tutti i comandi di EF Core:

| Parametro                 | Descrizione                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Contesto \<stringa >        | Il `DbContext` classe da utilizzare. Nome della classe solo o nome completo con gli spazi dei nomi.  Se questo parametro viene omesso, EF Core consente di trovare la classe del contesto. Se sono presenti più classi di contesto, questo parametro è obbligatorio. |
| -Progetto \<stringa >        | Il progetto di destinazione. Se questo parametro viene omesso, il **progetto predefinito** per **Console di gestione pacchetti** viene usato come progetto di destinazione.                                                                             |
| -StartupProject \<stringa > | Il progetto di avvio. Se questo parametro viene omesso, il **progetto di avvio** nelle **proprietà della soluzione** viene usato come progetto di destinazione.                                                                                 |
| -Verbose                  | Visualizzare output dettagliato.                                                                                                                                                                                                 |

Per visualizzare la Guida informazioni su un comando, usare PowerShell `Get-Help` comando.

> [!TIP]
> I parametri di contesto, progetto e oggetto StartupProject supportano l'espansione tramite tab.

## <a name="add-migration"></a>Add-Migration

Aggiunge una nuova migrazione.

Parametri:

| Parametro                         | Descrizione                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Nome \<stringa ><nobr>       | Il nome della migrazione. Si tratta di un parametro posizionale ed è obbligatorio.                                              |
| <nobr>-OutputDir \<stringa ></nobr> | La directory e sub-spazio dei nomi da usare. I percorsi sono relativi alla directory del progetto di destinazione. Il valore predefinito è "Migrazione". |

## <a name="drop-database"></a>Eliminazione di Database

Elimina il database.

Parametri:

| Parametro | Descrizione                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Mostra in quale database verrà rimossa, ma non eliminarlo. |

## <a name="get-dbcontext"></a>Get-DbContext

Elenchi disponibili `DbContext` tipi.

## <a name="remove-migration"></a>Remove-Migration

Rimuove l'ultima migrazione (rollback le modifiche al codice che sono state eseguite per la migrazione).

Parametri:

| Parametro | Descrizione                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Ripristinare la migrazione (rollback delle modifiche che sono state applicate al database). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Genera il codice per un `DbContext` e tipi di entità per un database. Affinché `Scaffold-DbContext` per generare un tipo di entità, la tabella di database deve avere una chiave primaria.

Parametri:

| Parametro                          | Descrizione                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \<stringa ></nobr> | La stringa di connessione al database. Per progetti ASP.NET Core 2.x, il valore può essere *nome =\<nome della stringa di connessione >*. In tal caso il nome deriva dalle origini configurazione che sono configurati per il progetto. Si tratta di un parametro posizionale ed è obbligatorio. |
| <nobr>-Provider \<stringa ></nobr>   | Il provider da utilizzare. In genere si tratta del nome del pacchetto NuGet, ad esempio: `Microsoft.EntityFrameworkCore.SqlServer`. Si tratta di un parametro posizionale ed è obbligatorio.                                                                                           |
| -OutputDir \<stringa >               | La directory in cui inserire file nella. I percorsi sono relativi alla directory del progetto.                                                                                                                                                                                             |
| -ContextDir \<stringa >              | La directory di inserire il `DbContext` del file in. I percorsi sono relativi alla directory del progetto.                                                                                                                                                                              |
| -Contesto \<stringa >                 | Il nome del `DbContext` classe da generare.                                                                                                                                                                                                                          |
| -Schemi \<String [] >               | Gli schemi delle tabelle per generare i tipi di entità per. Se questo parametro viene omesso, vengono inclusi tutti gli schemi.                                                                                                                                                             |
| -Tabelle \<String [] >                | Generare tipi di entità per le tabelle. Se questo parametro viene omesso, vengono incluse tutte le tabelle.                                                                                                                                                                         |
| -DataAnnotations                   | Usare gli attributi per configurare il modello (se possibile). Se questo parametro viene omesso, viene utilizzato solo l'API fluent.                                                                                                                                                      |
| -UseDatabaseNames                  | Usare nomi di tabella e colonna esattamente come appaiono nel database. Se questo parametro viene omesso, vengono modificati i nomi di database per più da vicino è conforme alle convenzioni di stile nome c#.                                                                                       |
| -Force                             | Sovrascrivi file esistenti.                                                                                                                                                                                                                                               |

Esempio:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Esempio che esegue lo scaffolding delle solo le tabelle selezionate e crea il contesto in una cartella separata con il nome specificato:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Migrazione degli script

Genera uno script SQL che tutte le modifiche da una migrazione selezionata si applica a un'altra migrazione selezionata.

Parametri:

| Parametro                | Descrizione                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-From* \<stringa >        | La migrazione inizia. Le migrazioni possono essere identificate in base al nome o ID. Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima*. Il valore predefinito è 0.                                                              |
| *-A* \<stringa >          | La migrazione finale. Per impostazione predefinita all'ultima migrazione.                                                                                                                                                                      |
| <nobr>-Idempotenti</nobr> | Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.                                                                                                                                                         |
| -Output \<stringa >        | File in cui scrivere il risultato. Se questo parametro viene omesso, il file viene creato con un nome generato nella stessa cartella quando vengono creati i file di runtime dell'app, ad esempio: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> To, From, e i parametri di Output supportano espansione tramite tab.

Nell'esempio seguente crea uno script per la migrazione InitialCreate, usando il nome della migrazione.

```powershell
Script-Migration -To InitialCreate
```

L'esempio seguente crea uno script per tutte le migrazioni dopo la migrazione InitialCreate, usando l'ID di migrazione.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Update-Database

Aggiorna il database per l'ultima migrazione o per una migrazione specificata.

| Parametro                           | Descrizione                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*-Migrazione* \<stringa ></nobr> | La migrazione di destinazione. Le migrazioni possono essere identificate in base al nome o ID. Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima* e fa sì che tutte le migrazioni da ripristinare. Se non viene specificata alcuna migrazione, il comando predefinito all'ultima migrazione. |

> [!TIP]
> Il parametro di migrazione supporta l'espansione tramite tab.

Nell'esempio seguente ripristina tutte le migrazioni.

```powershell
Update-Database -Migration 0
```

Negli esempi seguenti aggiornano il database per una migrazione specificata. Il primo Usa il nome della migrazione e il secondo Usa l'ID di migrazione:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```
