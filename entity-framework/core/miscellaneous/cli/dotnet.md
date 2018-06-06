---
title: .NET core - CLI EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754496"
---
<a name="ef-core-net-command-line-tools"></a>Strumenti da riga di comando di EF Core .NET
===============================
Gli strumenti da riga di comando .NET di Entity Framework Core sono un'estensione della piattaforma incrociata **dotnet** comando, che fa parte di [.NET Core SDK][2].

> [!TIP]
> Se si utilizza Visual Studio, è consigliabile [gli strumenti PMC] [ 1] poiché forniscono invece un'esperienza più integrata.

<a name="installing-the-tools"></a>Installazione degli strumenti
--------------------
> [!NOTE]
> .NET Core SDK versione 2.1.300 e include più recente **dotnet ef** comandi che sono compatibili con Entity Framework Core 2.0 e versioni successive. Pertanto se si utilizza le versioni recenti di .NET Core SDK e il runtime di Entity Framework Core, è richiesta alcuna installazione ed è possibile ignorare il resto di questa sezione.
>
> D'altro canto, la **dotnet ef** strumento autonomo nella versione di .NET Core SDK 2.1.300 e più recente non è compatibile con la versione dei componenti di base di Entity Framework 1.0 e 1.1. Prima di poter utilizzare un progetto che Usa queste versioni precedenti di Entity Framework Core in un computer che dispone di .NET Core SDK 2.1.300 o installata più recente, è necessario installare anche versione 2.1.200 o precedente del SDK e configurare l'applicazione di usare tale versione precedente modificando il relativo  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file. Questo file è normalmente incluso nella directory della soluzione (uno di sopra del progetto). È quindi possibile procedere con l'istruzione di installazione riportata di seguito.

Per le versioni precedenti di .NET Core SDK, è possibile installare gli strumenti della riga di comando di EF Core .NET utilizzando i seguenti passaggi:

1. Modificare il file di progetto e aggiungere Microsoft.EntityFrameworkCore.Tools.DotNet come elemento DotNetCliToolReference (vedere sotto)
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
> Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che risulta particolarmente utile per i pacchetti vengono in genere utilizzati solo durante lo sviluppo.

Se si ha tutti gli elementi a destra, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Utilizzo degli strumenti
---------------
Ogni volta che si richiama un comando, vengono utilizzate due progetti:

Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file. Il progetto di destinazione del progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il <nobr> **-progetto** </nobr> opzione.

Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto. Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il **-progetto di avvio** opzione.

> [!NOTE]
> Ad esempio, l'aggiornamento del database dell'applicazione web che include Core EF installato in un progetto diverso avrà un aspetto simile al seguente: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)

Opzioni comuni:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Mostra l'output JSON.           |
| -c | -contesto \<DBCONTEXT >           | L'elemento DbContext da utilizzare.       |
| -p | -progetto \<progetto >             | Il progetto da utilizzare.         |
| -s | -progetto di avvio \<progetto >     | Il progetto di avvio da utilizzare. |
|    | -framework \<FRAMEWORK >         | Il framework di destinazione.       |
|    | -configurazione \<configurazione > | Configurazione da utilizzare.   |
|    | -runtime \<identificatore >          | Il runtime da utilizzare.         |
| -h | -Guida in linea                           | Mostra le informazioni della Guida.      |
| -v | -verbose                        | Mostra l'output dettagliato.        |
|    | -Nessun colore                       | Non usa l'output.      |
|    | --prefix-output                  | Prefisso con livello di output.   |


> [!TIP]
> Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.

<a name="commands"></a>Comandi:
--------

### <a name="dotnet-ef-database-drop"></a>eliminazione del database ef dotnet

Elimina il database.

Opzioni:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | -force   | Non verificare.                                           |
|    | -esecuzione | Mostra il database verrà rimossa, ma non l'eliminazione. |

### <a name="dotnet-ef-database-update"></a>aggiornamento del database ef dotnet

Aggiorna il database in una migrazione specificata.

Argomenti:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRAZIONE &GT; | La migrazione di destinazione. Se è 0, verranno annullate tutte le migrazioni. Per impostazione predefinita all'ultima migrazione. |

### <a name="dotnet-ef-dbcontext-info"></a>informazioni di dbcontext ef dotnet

Ottiene informazioni sul tipo DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>elenco di dbcontext ef dotnet

Elenca i tipi DbContext disponibili.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding di dbcontext ef dotnet

Strutture una tipi DbContext ed entità per un database.

Argomenti:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<CONNESSIONE &GT; | La stringa di connessione al database.                                      |
| \<PROVIDER &GT;   | Il provider da utilizzare. (ad esempio, Microsoft.EntityFrameworkCore.SqlServer) |

Opzioni:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --le annotazioni dei dati                      | Utilizzare gli attributi per configurare il modello (dove possibile). Se omesso, viene utilizzato solo l'API fluent. |
| -c              | -contesto \<nome >                       | Il nome di DbContext.                                                                       |
|                 | -dir contesto \<percorso >                   | Della directory in cui inserire file DbContext in. I percorsi sono relativi alla directory del progetto.             |
| -f              | -force                                 | Sovrascrivi file esistenti.                                                                        |
| -o              | -output-dir \<percorso >                    | Della directory in cui inserire i file in. I percorsi sono relativi alla directory del progetto.                      |
|                 | <nobr>--schema \<SCHEMA_NAME>...</nobr> | Gli schemi delle tabelle per generare i tipi di entità per.                                              |
| -t              | -tabella \<TABLE_NAME >...                | Generare tipi di entità per le tabelle.                                                         |
|                 | -Utilizzare nomi di database                    | Utilizzare nomi di tabella e colonna direttamente dal database.                                           |

### <a name="dotnet-ef-migrations-add"></a>aggiungere le migrazioni di ef dotnet

Aggiunge una nuova migrazione.

Argomenti:

|         |                            |
|:--------|:---------------------------|
| \<NOME &GT; | Il nome della migrazione. |

Opzioni:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>-output-dir \<percorso ></nobr> | La directory e spazio dei nomi secondario da utilizzare. I percorsi sono relativi alla directory del progetto. Valore predefinito è "Migrazione". |

### <a name="dotnet-ef-migrations-list"></a>elenco di migrazioni ef dotnet

Elenca le migrazioni disponibili.

### <a name="dotnet-ef-migrations-remove"></a>rimuovere le migrazioni di ef dotnet

Rimuove l'ultima migrazione.

Opzioni:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | -force | Ripristinare la migrazione, se è stato applicato al database. |

### <a name="dotnet-ef-migrations-script"></a>script di migrazioni ef dotnet

Genera uno script SQL dalla migrazione.

Argomenti:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<DA &GT; | La migrazione inizia. Il valore predefinito 0 (il database iniziale). |
| \<A &GT;   | La migrazione finale. Per impostazione predefinita all'ultima migrazione.         |

Opzioni:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | -output \<FILE > | File in cui scrivere il risultato.                                   |
| -i | idempotente.     | Generare uno script che può essere usato in un database in ogni operazione di migrazione. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
