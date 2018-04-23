---
title: Console di gestione pacchetti (Visual Studio) - Core a Entity Framework
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-package-manager-console-tools"></a>Strumenti di Entity Framework Core Package Manager Console
=====================================
Gli strumenti di Entity Framework Core Package Manager Console (PMC) eseguito all'interno di Visual Studio tramite NuGet [Package Manager Console][2].
Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.

> [!TIP]
> Non si usa Visual Studio? Il [strumenti da riga di comando di base EF] [ 1] sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.

<a name="installing-the-tools"></a>Installazione degli strumenti
--------------------
Installare gli strumenti di Entity Framework Core Package Manager Console installando il pacchetto Microsoft.EntityFrameworkCore.Tools NuGet.
È possibile installarlo eseguendo il comando seguente all'interno di [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Se tutto funziona correttamente, deve essere in grado di eseguire questo comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Se il progetto di avvio è destinato a .NET Standard [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti.

> [!IMPORTANT]
> Se si utilizza **Windows universale** o **Xamarin**, spostare il codice di Entity Framework in una libreria di classi .NET Standard e [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti. Specificare la libreria di classi come progetto di avvio.

<a name="using-the-tools"></a>Utilizzo degli strumenti
---------------
Ogni volta che si richiama un comando, vengono utilizzate due progetti:

Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file. Per impostazione predefinita il progetto di destinazione di **progetto predefinito** selezionato nella Console di gestione pacchetti, ma può anche essere specificato utilizzando il progetto parametro-.

Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto. Per impostazione predefinita uno **imposta come progetto di avvio** in Esplora soluzioni. Può anche essere specificato utilizzando il parametro - proprietà.

Parametri comuni:

|                           |                             |
|:--------------------------|:----------------------------|
| -Contesto \<stringa >        | L'elemento DbContext da utilizzare.       |
| -Progetto \<stringa >        | Il progetto da utilizzare.         |
| Proprietà - \<stringa > | Il progetto di avvio da utilizzare. |
| -Verbose                  | Mostra l'output dettagliato.        |

Per visualizzare informazioni della Guida su un comando, utilizzare PowerShell `Get-Help` comando.

> [!TIP]
> I parametri di contesto, del progetto e proprietà supportano l'espansione tramite tab.

> [!TIP]
> Impostare **env:ASPNETCORE_ENVIRONMENT** prima di eseguire per specificare l'ambiente di ASP.NET Core.

<a name="commands"></a>Comandi:
--------

### <a name="add-migration"></a>Aggiungere la migrazione

Aggiunge una nuova migrazione.

Parametri:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Nome*** \<stringa >             | Il nome della migrazione.                                                                                       |
| <nobr>-OutputDir \<stringa ></nobr> | La directory e spazio dei nomi secondario da utilizzare. I percorsi sono relativi alla directory del progetto. Valore predefinito è "Migrazione". |

> [!NOTE]
> I parametri in **grassetto** sono necessari e quelle in *corsivo* sono posizionali.

### <a name="drop-database"></a>Eliminazione di Database

Elimina il database.

Parametri:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Mostra il database verrà rimossa, ma non l'eliminazione. |

### <a name="get-dbcontext"></a>Get-DbContext

Ottiene informazioni sul tipo DbContext.

### <a name="remove-migration"></a>Remove-migrazione

Rimuove l'ultima migrazione.

Parametri:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Ripristinare la migrazione, se è stato applicato al database. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Strutture una tipi DbContext ed entità per un database.

Parametri:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connessione*** \<stringa ></nobr> | La stringa di connessione al database.                                                           |
| ***-Provider*** \<stringa >                | Il provider da utilizzare. Ad esempio, Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir \<stringa >                     | Della directory in cui inserire i file in. I percorsi sono relativi alla directory del progetto.                      |
| -ContextDir \<stringa >                    | Della directory in cui inserire file DbContext in. I percorsi sono relativi alla directory del progetto.             |
| -Contesto \<stringa >                       | Il nome di DbContext per generare.                                                           |
| -Gli schemi \<String [] >                     | Gli schemi delle tabelle per generare i tipi di entità per.                                              |
| -Tabelle \<String [] >                      | Generare tipi di entità per le tabelle.                                                         |
| -DataAnnotations                         | Utilizzare gli attributi per configurare il modello (dove possibile). Se omesso, viene utilizzato solo l'API fluent. |
| -UseDatabaseNames                        | Utilizzare nomi di tabella e colonna direttamente dal database.                                           |
| -Force                                   | Sovrascrivi file esistenti.                                                                        |

### <a name="script-migration"></a>Migrazione di script

Genera uno script SQL dalla migrazione.

Parametri:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-Da* \<stringa > | La migrazione inizia. Il valore predefinito 0 (il database iniziale).      |
| *-A* \<stringa >   | La migrazione finale. Per impostazione predefinita all'ultima migrazione.              |
| -Idempotente       | Generare uno script che può essere usato in un database in ogni operazione di migrazione. |
| -Output \<stringa > | File in cui scrivere il risultato.                                   |

> [!TIP]
> To, From, e i parametri di Output supportano l'espansione tramite tab.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migrazione* \<stringa ></nobr> | La migrazione di destinazione. Se è '0', verranno annullate tutte le migrazioni. Per impostazione predefinita all'ultima migrazione. |

> [!TIP]
> Il parametro di migrazione supporta l'espansione tramite tab.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
