---
title: .NET core - CLI EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a>Strumenti da riga di comando di EF Core .NET
===============================
Gli strumenti da riga di comando .NET di Entity Framework Core sono un'estensione della piattaforma incrociata **dotnet** comando, che fa parte di [.NET Core SDK][2].

> [!TIP]
> Se si utilizza Visual Studio, è consigliabile [gli strumenti PMC] [ 1] poiché forniscono invece un'esperienza più integrata.

<a name="installing-the-tools"></a>Installazione degli strumenti
--------------------
Installare gli strumenti da riga di comando di EF Core .NET attenendosi alla procedura seguente:

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

Il progetto di destinazione è in cui vengono aggiunti tutti i file (o in alcuni casi rimossi). Il progetto di destinazione del progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il <nobr> **-progetto** </nobr> opzione.

Il progetto di avvio viene emulato dagli strumenti quando si esegue il codice del progetto. Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il **-progetto di avvio** opzione.

Opzioni comuni:

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | json:                           | Mostra l'output JSON.           |
| -c | -contesto \<DBCONTEXT >           | L'elemento DbContext da utilizzare.       |
| -P | -progetto \<progetto >             | Il progetto da utilizzare.         |
| -s | -progetto di avvio \<progetto >     | Il progetto di avvio da utilizzare. |
|    | -framework \<FRAMEWORK >         | Il framework di destinazione.       |
|    | -configurazione \<configurazione > | Configurazione da utilizzare.   |
|    | -runtime \<identificatore >          | Il runtime da utilizzare.         |
| -h | -Guida in linea                           | Mostra le informazioni della Guida.      |
| -v | -verbose                        | Mostra l'output dettagliato.        |
|    | -Nessun colore                       | Non usa l'output.      |
|    | -output-prefisso                  | Prefisso con livello di output.   |


> [!TIP]
> Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.

<a name="commands"></a>Comandi
--------

### <a name="dotnet-ef-database-drop"></a>eliminazione del database ef dotnet

Elimina il database.

Opzioni:

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| -f | -force   | Non verificare.                                           |
|    | -esecuzione | Mostra il database verrà rimossa, ma non l'eliminazione. |

### <a name="dotnet-ef-database-update"></a>aggiornamento del database ef dotnet

Aggiorna il database in una migrazione specificata.

Argomenti:

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| \<MIGRAZIONE > | La migrazione di destinazione. Se è 0, verranno annullate tutte le migrazioni. Per impostazione predefinita all'ultima migrazione. |

### <a name="dotnet-ef-dbcontext-info"></a>informazioni di dbcontext ef dotnet

Ottiene informazioni sul tipo DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>elenco di dbcontext ef dotnet

Elenca i tipi DbContext disponibili.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding di dbcontext ef dotnet

Strutture una tipi DbContext ed entità per un database.

Argomenti:

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| \<CONNESSIONE > | La stringa di connessione al database.                              |
| \<PROVIDER >   | Il provider da utilizzare. Ad esempio, Microsoft.EntityFrameworkCore.SqlServer) |

Opzioni:

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <nobr>-d</nobr> |       --le annotazioni dei dati                | Utilizzare gli attributi per configurare il modello (dove possibile). Se omesso, viene utilizzato solo l'API fluent. |
|       -c        |       -contesto \<nome >                 | Il nome di DbContext.                               |
|       -f        |       -force                           | Sovrascrivi file esistenti.                                |
|       -o        |       -output-dir \<percorso >              | Della directory in cui inserire i file in. I percorsi sono relativi alla directory del progetto. |
|                 | <nobr>-schema \<SCHEMA_NAME >...</nobr> | Gli schemi delle tabelle per generare i tipi di entità per.      |
|       -t        |       -tabella \<TABLE_NAME >...          | Generare tipi di entità per le tabelle.                 |
|                 |       -Utilizzare nomi di database              | Utilizzare nomi di tabella e colonna direttamente dal database.   |

### <a name="dotnet-ef-migrations-add"></a>aggiungere le migrazioni di ef dotnet

Aggiunge una nuova migrazione.

Argomenti:

|         |                            |
| ------- | -------------------------- |
| \<NOME > | Il nome della migrazione. |

Opzioni:

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <nobr>-o</nobr> | <nobr>-output-dir \<percorso ></nobr> | La directory e spazio dei nomi secondario da utilizzare. I percorsi sono relativi alla directory del progetto. Valore predefinito è "Migrazione". |

### <a name="dotnet-ef-migrations-list"></a>elenco di migrazioni ef dotnet

Elenca le migrazioni disponibili.

### <a name="dotnet-ef-migrations-remove"></a>rimuovere le migrazioni di ef dotnet

Rimuove l'ultima migrazione.

Opzioni:

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| -f | -force | Non verificare se la migrazione è stata applicata al database. |

### <a name="dotnet-ef-migrations-script"></a>script di migrazioni ef dotnet

Genera uno script SQL dalla migrazione.

Argomenti:

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| \<DA > | La migrazione inizia. Il valore predefinito 0 (il database iniziale). |
| \<A >   | La migrazione finale. Per impostazione predefinita all'ultima migrazione.         |

Opzioni:

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| -o | -output \<FILE > | File in cui scrivere il risultato.                                   |
| -i | idempotente.     | Generare uno script che può essere usato in un database in ogni operazione di migrazione. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
