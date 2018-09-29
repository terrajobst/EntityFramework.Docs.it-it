---
title: EF Core riferimenti per gli strumenti (.NET CLI) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: a280aad0344a89c41c30be27a249df3c28c44c70
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447170"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Riferimenti - CLI di .NET agli strumenti di Entity Framework Core

Gli strumenti dell'interfaccia della riga di comando (CLI) per Entity Framework Core eseguono attività di sviluppo in fase di progettazione. Ad esempio, creano [migrazioni](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), applicare le migrazioni e generare il codice per un modello basato su un database esistente. I comandi sono un'estensione per il multipiattaforma [dotnet](/dotnet/core/tools) comando, che fa parte delle [.NET Core SDK](https://www.microsoft.com/net/core). Questi strumenti funzionano con i progetti .NET Core.

Se si usa Visual Studio, è consigliabile la [strumenti della Console di gestione pacchetti](powershell.md) invece:
* Funzionano automaticamente con il progetto corrente selezionato nella **Console di gestione pacchetti** senza la necessità di passare manualmente le directory.
* Vengono aperti automaticamente i file generati da un comando dopo il completamento del comando.

## <a name="installing-the-tools"></a>Installazione degli strumenti

La procedura di installazione dipende dalla versione e il tipo di progetto:

* ASP.NET Core 2.1 e versioni successive
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Installare l'oggetto corrente [.NET Core SDK](https://www.microsoft.com/net/download/core). Il SDK deve essere installato anche se è presente la versione più recente di Visual Studio 2017.

  Questo è tutto ciò che serve per ASP.NET Core 2.1 + in quanto il `Microsoft.EntityFrameworkCore.Design` pacchetto è incluso nel [Microsoft.AspNetCore.App metapacchetto](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (non su ASP.NET Core)

Il `dotnet ef` comandi sono inclusi in .NET Core SDK, ma per abilitare i comandi è necessario installare il `Microsoft.EntityFrameworkCore.Design` pacchetto.

* Installare l'oggetto corrente [.NET Core SDK](https://www.microsoft.com/net/download/core). Il SDK deve essere installato anche se è presente la versione più recente di Visual Studio 2017.

* Installare l'ultima versione stabile `Microsoft.EntityFrameworkCore.Design` pacchetto. 

  ``` Console   
  dotnet add package Microsoft.EntityFrameworkCore.Design   
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Installare .NET Core SDK versione 2.1.200. Nelle versioni successive non sono compatibili con gli strumenti CLI di EF Core 1.0 e 1.1.

* Configurare l'applicazione alla versione del SDK Usa il 2.1.200 modificando relativi [Global. JSON](/dotnet/core/tools/global-json) file. In genere, questo file è incluso nella directory della soluzione (quello precedente del progetto). 

* Modificare il file di progetto e aggiungere `Microsoft.EntityFrameworkCore.Tools.DotNet` come un `DotNetCliToolReference` elemento. Specificare l'ultima versione 1.x, ad esempio: 1.1.6. Vedere l'esempio di file di progetto alla fine di questa sezione.

* Installare l'ultima versione 1.x del `Microsoft.EntityFrameworkCore.Design` creare un pacchetto, ad esempio:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Con entrambi i riferimenti del pacchetto aggiunti, il file di progetto simile al seguente:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Un riferimento al pacchetto con `PrivateAssets="All"` non viene esposta ai progetti che fanno riferimento a questo progetto. Questa restrizione è particolarmente utile per i pacchetti che in genere vengono usati solo durante lo sviluppo.

### <a name="verify-installation"></a>Verificare l'installazione

Eseguire i comandi seguenti per verificare che siano installati correttamente gli strumenti CLI di EF Core:

  ``` Console
  dotnet restore
  dotnet ef
  ```

L'output del comando identifica la versione degli strumenti in uso:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Usando gli strumenti

Prima di usare gli strumenti, potrebbe essere necessario creare un progetto di avvio o impostare l'ambiente.

### <a name="target-project-and-startup-project"></a>Progetto di destinazione e progetto di avvio

I comandi di fare riferimento a un *project* e una *progetto di avvio*.

* Il *project* è noto anche come il *progetto di destinazione* perché è in cui i comandi aggiungono o rimuovono file. Per impostazione predefinita, il progetto nella directory corrente è il progetto di destinazione. È possibile specificare un altro progetto come progetto di destinazione usando il <nobr> `--project` </nobr> opzione.

* Il *progetto di avvio* è quello che gli strumenti di compilare ed eseguire. Gli strumenti richiede l'esecuzione di codice dell'applicazione in fase di progettazione per ottenere informazioni sul progetto, ad esempio la stringa di connessione di database e la configurazione del modello. Per impostazione predefinita, il progetto nella directory corrente è il progetto di avvio. È possibile specificare un altro progetto come progetto di avvio usando il <nobr> `--startup-project` </nobr> opzione.

Il progetto di avvio e di un progetto di destinazione sono spesso dello stesso progetto. Uno scenario tipico in cui sono progetti separati è quando:

* Le classi di contesto ed entità di Entity Framework Core sono in una libreria di classi .NET Core.
* Un'app console .NET Core o un'app web fa riferimento alla libreria di classi.

È anche possibile [inserire il codice di migrazioni in una libreria di classi separata dal contesto di Entity Framework Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Altri framework di destinazione

Gli strumenti dell'interfaccia della riga di lavoro con i progetti .NET Core e .NET Framework. Le app con il modello di EF Core in una libreria di classi .NET Standard potrebbero non avere un progetto di .NET Framework o .NET Core. Ad esempio, questo vale delle App Xamarin e (Universal Windows Platform). In questi casi, è possibile creare un progetto di app console .NET Core, il cui unico scopo è agire come progetto di avvio per gli strumenti. Il progetto può essere un progetto fittizio senza vero codice &mdash; , è necessario soltanto per fornire una destinazione per gli strumenti.

Perché è un progetto fittizio necessario? Come accennato in precedenza, gli strumenti di dovranno eseguire il codice dell'applicazione in fase di progettazione. A tale scopo, è necessario usare il runtime di .NET Core. Quando il modello di EF Core è in un progetto destinato a .NET Core o .NET Framework, gli strumenti di Entity Framework Core presi in prestito il runtime dal progetto. Che non possono eseguire se il modello di EF Core è in una libreria di classi .NET Standard. .NET Standard non è un'implementazione .NET effettiva; si tratta di una specifica di un set di API che le implementazioni di .NET devono supportare. .NET Standard non è pertanto sufficiente per gli strumenti di Entity Framework Core eseguire il codice dell'applicazione. Il progetto fittizio creato per essere utilizzato come progetto di avvio fornisce una piattaforma di destinazione concreta in cui gli strumenti possono caricare la libreria di classi .NET Standard. 

### <a name="aspnet-core-environment"></a>Ambiente ASP.NET Core

Per specificare l'ambiente per i progetti ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima di eseguire comandi.

## <a name="common-options"></a>Opzioni comuni

|                   | Opzione                             | Descrizione                                                                                                                                                                                                                                                   |
|-------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                           | Mostra l'output JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`            | Il `DbContext` classe da utilizzare. Nome della classe solo o nome completo con gli spazi dei nomi.  Se questa opzione viene omessa, EF Core troverà la classe del contesto. Se sono presenti più classi di contesto, questa opzione è obbligatoria.                                            |
| `-p`              | `--project <PROJECT>`              | Percorso relativo alla cartella del progetto del progetto di destinazione.  Valore predefinito è la cartella corrente.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`      | Percorso relativo alla cartella del progetto del progetto di avvio. Valore predefinito è la cartella corrente.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`          | Il [Moniker Framework di destinazione](/dotnet/standard/frameworks#supported-target-framework-versions) per il [framework di destinazione](/dotnet/standard/frameworks).  Usare quando il file di progetto specifica più Framework di destinazione e si desidera selezionare uno di essi. |
|                   | `--configuration <CONFIGURATION>`  | La configurazione della build, ad esempio: `Debug` o `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`           | L'identificatore di ripristinare i pacchetti per il runtime di destinazione. Per un elenco degli identificatori di runtime (RID, Runtime Identifier), vedere il [catalogo RID](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                           | Mostra le informazioni della Guida.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                        | Visualizzare output dettagliato.                                                                                                                                                                                                                                          |
|                   | `--no-color`                       | Non leggano l'output.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                  | Prefisso con il livello di output.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>eliminazione del database ef dotnet

Elimina il database.

Opzioni:

|                   | Opzione                   | Descrizione                                                |
|-------------------|--------------------------|------------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Non verificare.                                             |
|                   | <nobr>`--dry-run`</nobr> | Mostra in quale database verrà rimossa, ma non eliminarlo.   |

## <a name="dotnet-ef-database-update"></a>aggiornamento del database ef dotnet

Aggiorna il database per l'ultima migrazione o per una migrazione specificata.

Argomenti:

| Argomento       | Descrizione                                                                                                                                                                                                                                                     |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>`  | La migrazione di destinazione. Le migrazioni possono essere identificate in base al nome o ID. Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima* e fa sì che tutte le migrazioni da ripristinare. Se non viene specificata alcuna migrazione, il comando predefinito all'ultima migrazione. |

Negli esempi seguenti aggiornano il database per una migrazione specificata. Il primo Usa il nome della migrazione e il secondo Usa l'ID di migrazione:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informazioni sull'oggetto dbcontext di Entity Framework di dotnet

Ottiene informazioni su un `DbContext` tipo.

## <a name="dotnet-ef-dbcontext-list"></a>elenco di dbcontext di Entity Framework dotnet

Elenchi disponibili `DbContext` tipi.

## <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding dbcontext di Entity Framework di dotnet

Genera il codice per un `DbContext` e tipi di entità per un database.

Argomenti:

| Argomento        | Descrizione                                                                                                                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>`  | La stringa di connessione al database. Per progetti ASP.NET Core 2.x, il valore può essere *nome =\<nome della stringa di connessione >*. In tal caso il nome deriva dalle origini configurazione che sono configurati per il progetto. |
| `<PROVIDER>`    | Il provider da utilizzare. In genere si tratta del nome del pacchetto NuGet, ad esempio: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Opzioni:

|                   | Opzione                                    | Descrizione                                                                                                                                                                    |
|-------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr>   | `--data-annotations`                      | Usare gli attributi per configurare il modello (se possibile). Se questa opzione viene omessa, viene utilizzato solo l'API fluent.                                                                |
| `-c`              | `--context <NAME>`                        | Il nome del `DbContext` classe da generare.                                                                                                                                 |
|                   | `--context-dir <PATH>`                    | La directory di inserire il `DbContext` file di classe. I percorsi sono relativi alla directory del progetto. Gli spazi dei nomi sono derivati dai nomi di cartella.                                 |
| `-f`              | `--force`                                 | Sovrascrivi file esistenti.                                                                                                                                                      |
| `-o`              | `--output-dir <PATH>`                     | La directory per inserire i file di classe di entità in. I percorsi sono relativi alla directory del progetto.                                                                                       |
|                   | <nobr>`--schema <SCHEMA_NAME>...`</nobr>  | Gli schemi delle tabelle per generare i tipi di entità per. Per specificare più schemi, ripetere `--schema` per ognuno di essi. Se questa opzione viene omessa, vengono inclusi tutti gli schemi.          |
| `-t`              | `--table <TABLE_NAME>`...                 | Generare tipi di entità per le tabelle. Per specificare più tabelle, ripetere `-t` o `--table` per ognuno di essi. Se si omette questa opzione, tutte le tabelle vengono incluse.                |
|                   | `--use-database-names`                    | Usare nomi di tabella e colonna esattamente come appaiono nel database. Se questa opzione viene omessa, vengono modificati i nomi di database per più da vicino è conforme alle convenzioni di stile nome c#. |

Nell'esempio seguente esegue scaffolding delle tutti gli schemi e tabelle e inserisce i nuovi file nei *modelli* cartella.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Nell'esempio seguente esegue scaffolding delle sole tabelle selezionate e crea il contesto in una cartella separata con il nome specificato:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>aggiungere dotnet migrazioni di Entity Framework

Aggiunge una nuova migrazione.

Argomenti:

| Argomento  | Descrizione                  |
|-----------|------------------------------|
| `<NAME>`  | Il nome della migrazione.   |

Opzioni:

|                   | Opzione                              | Descrizione                                                                                                        |
|-------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr>  | La directory e sub-spazio dei nomi da usare. I percorsi sono relativi alla directory del progetto. Il valore predefinito è "Migrazione".   |

## <a name="dotnet-ef-migrations-list"></a>elenco di dotnet ef migrations

Elenca le migrazioni disponibili.

## <a name="dotnet-ef-migrations-remove"></a>rimuovere le migrazioni di Entity Framework di dotnet

Rimuove l'ultima migrazione (rollback le modifiche al codice che sono state eseguite per la migrazione). 

Opzioni:

|                   | Opzione    | Descrizione                                                                        |
|-------------------|-----------|------------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Ripristinare la migrazione (rollback delle modifiche che sono state applicate al database).    |

## <a name="dotnet-ef-migrations-script"></a>script di dotnet ef migrations

Genera uno script SQL dalle migrazioni.

Argomenti:

| Argomento  | Descrizione                                                                                                                                                   |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>`  | La migrazione inizia. Le migrazioni possono essere identificate in base al nome o ID. Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima*. Il valore predefinito è 0. |
| `<TO>`    | La migrazione finale. Per impostazione predefinita all'ultima migrazione.                                                                                                         |

Opzioni:

|                   | Opzione             | Descrizione                                                          |
|-------------------|--------------------|----------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>`  | File in cui scrivere lo script.                                     |
| `-i`              | `--idempotent`     | Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.   |

L'esempio seguente crea uno script per la migrazione InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

Nell'esempio seguente crea uno script per tutte le migrazioni dopo la migrazione InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```