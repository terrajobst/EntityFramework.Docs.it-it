---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152414"
---
<a name="ef-core-net-command-line-tools"></a>Strumenti da riga di comando di EF Core .NET
===============================
Gli strumenti della riga di comando .NET di Entity Framework Core sono un'estensione per il multipiattaforma **dotnet** comando, che fa parte delle [.NET Core SDK][2].

> [!TIP]
> Se si usa Visual Studio, è consigliabile [gli strumenti della console di gestione pacchetti] [ 1] ma poiché forniscono un'esperienza più integrata.

<a name="installing-the-tools"></a>Installazione degli strumenti
--------------------
> [!NOTE]
> .NET Core SDK 2.1.300 versione e include più recente **dotnet ef** comandi che sono compatibili con EF Core 2.0 e versioni successive. Se si usa le versioni recenti di .NET Core SDK e runtime di Entity Framework Core, pertanto è richiesta alcuna installazione ed è possibile ignorare il resto di questa sezione.
>
> D'altra parte, il **dotnet ef** strumento contenuto nella versione di .NET Core SDK 2.1.300 e versioni successiva non è compatibile con la versione di EF Core 1.0 e 1.1. Prima di poter utilizzare un progetto che Usa queste versioni precedenti di EF Core in un computer che dispone di .NET Core SDK 2.1.300 installati o versioni successive, è necessario installare anche versione 2.1.200 o precedente del SDK e configurare l'applicazione per usare tale versione precedente, modificando il  [Global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file. In genere, questo file è incluso nella directory della soluzione (quello precedente del progetto). È quindi possibile procedere con le istruzioni di installazione riportato di seguito.

Per le versioni precedenti di .NET Core SDK, è possibile installare gli strumenti della riga di comando di EF Core .NET attenendosi alla procedura seguente:

1. Modificare il file di progetto e aggiungere l'entityframeworkcore come elemento DotNetCliToolReference (vedere sotto)
2. Eseguire i comandi seguenti:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Il progetto risultante dovrebbe essere simile al seguente:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che è particolarmente utile per i pacchetti che in genere vengono usati solo durante lo sviluppo.

Se si ha tutto per bene, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Usando gli strumenti
---------------
Ogni volta che si richiama un comando, sono disponibili due progetti:

Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file. Il progetto di destinazione per impostazione predefinita per il progetto nella directory corrente, ma può essere modificato utilizzando la <nobr> **`--project`** </nobr> opzione.

Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto. Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando la **`--startup-project`** opzione.

> [!NOTE]
> L'aggiornamento del database dell'applicazione web con EF Core installato in un progetto diverso, ad esempio, si presenta come segue: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)

Opzioni comuni:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | Mostra l'output JSON.           |
| -c | `--context <DBCONTEXT>`           | DbContext da utilizzare.       |
| -p | `--project <PROJECT>`             | Il progetto da utilizzare.         |
| -s | `--startup-project <PROJECT>`     | Il progetto di avvio da usare. |
|    | `--framework <FRAMEWORK>`         | Il framework di destinazione.       |
|    | `--configuration <CONFIGURATION>` | Configurazione da utilizzare.   |
|    | `--runtime <IDENTIFIER>`          | Usare il runtime.         |
| -h | `--help`                           | Mostra le informazioni della Guida.      |
| -v | `--verbose`                        | Visualizzare output dettagliato.        |
|    | `--no-color`                       | Non leggano l'output.      |
|    | `--prefix-output`                  | Prefisso con il livello di output.   |


> [!TIP]
> Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.

<a name="commands"></a>Comandi:
--------

### <a name="dotnet-ef-database-drop"></a>eliminazione del database ef dotnet

Elimina il database.

Opzioni:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | Non verificare.                                           |
|    | `--dry-run` | Mostra in quale database verrà rimossa, ma non eliminarlo. |

### <a name="dotnet-ef-database-update"></a>aggiornamento del database ef dotnet

Aggiorna il database per una migrazione specificata.

Argomenti:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | La migrazione di destinazione. Se è 0, verranno ripristinate tutte le migrazioni. Per impostazione predefinita all'ultima migrazione. |

### <a name="dotnet-ef-dbcontext-info"></a>informazioni sull'oggetto dbcontext di Entity Framework di dotnet

Ottiene informazioni su un tipo DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>elenco di dbcontext di Entity Framework dotnet

Elenca i tipi DbContext disponibili.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding dbcontext di Entity Framework di dotnet

Esegue lo scaffolding di un tipi DbContext ed entità per un database.

Argomenti:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | La stringa di connessione al database.                                      |
| `<PROVIDER>`   | Il provider da utilizzare. (ad esempio, l'entityframeworkcore) |

Opzioni:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | Usare gli attributi per configurare il modello (se possibile). Se omesso, viene utilizzato solo l'API fluent. |
| -c              | `--context <NAME>`                       | Il nome di DbContext.                                                                       |
|                 | `--context-dir <PATH>`                   | La directory in cui inserire file DbContext in. I percorsi sono relativi alla directory del progetto.             |
| -f              | `--force`                                 | Sovrascrivi file esistenti.                                                                        |
| -o              | `--output-dir <PATH>`                    | La directory in cui inserire file nella. I percorsi sono relativi alla directory del progetto.                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Gli schemi delle tabelle per generare i tipi di entità per.                                              |
| -t              | `--table <TABLE_NAME>`...                | Generare tipi di entità per le tabelle.                                                         |
|                 | `--use-database-names`                    | Usare nomi di tabella e colonna direttamente dal database.                                           |

### <a name="dotnet-ef-migrations-add"></a>aggiungere dotnet migrazioni di Entity Framework

Aggiunge una nuova migrazione.

Argomenti:

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | Il nome della migrazione. |

Opzioni:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr> `--output-dir <PATH>` </nobr> | La directory e sub-spazio dei nomi da usare. I percorsi sono relativi alla directory del progetto. Il valore predefinito è "Migrazione". |

### <a name="dotnet-ef-migrations-list"></a>elenco di dotnet ef migrations

Elenca le migrazioni disponibili.

### <a name="dotnet-ef-migrations-remove"></a>rimuovere le migrazioni di Entity Framework di dotnet

Rimuove l'ultima migrazione.

Opzioni:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | Se è stato applicato al database, ripristinare la migrazione. |

### <a name="dotnet-ef-migrations-script"></a>script di dotnet ef migrations

Genera uno script SQL dalle migrazioni.

Argomenti:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | La migrazione inizia. Il valore predefinito è 0 (il database iniziale). |
| `<TO>`   | La migrazione finale. Per impostazione predefinita all'ultima migrazione.         |

Opzioni:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | File in cui scrivere il risultato.                                   |
| -i | `--idempotent`     | Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
