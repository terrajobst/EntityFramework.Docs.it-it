---
title: Console di gestione pacchetti (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490850"
---
<a name="ef-core-package-manager-console-tools"></a>Strumenti della Console di gestione pacchetti di EF Core
=====================================
Gli strumenti di Entity Framework Core Package Manager Console (console di gestione pacchetti) vengono eseguiti all'interno di Visual Studio usando NuGet [Console di gestione pacchetti][2].
Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.

> [!TIP]
> Non si utilizza Visual Studio? Il [degli strumenti della riga di comando di Entity Framework Core] [ 1] sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.

<a name="installing-the-tools"></a>Installazione degli strumenti
--------------------
Installare gli strumenti di Entity Framework Core Package Manager Console installando il pacchetto NuGet entityframeworkcore.
È possibile installarlo eseguendo il comando seguente all'interno [Console di gestione pacchetti][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Se tutto funziona correttamente, è necessario essere in grado di eseguire questo comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Se il progetto di avvio è destinata a .NET Standard [cross-destinazione un framework supportato] [ 3] prima di usare gli strumenti.

> [!IMPORTANT]
> Se si usa **Windows Universal** oppure **Xamarin**, spostare il codice di Entity Framework in una libreria di classi .NET Standard e [destinazione trasversale un framework supportato] [ 3] prima di usare gli strumenti. Specificare la libreria di classi come progetto di avvio.

<a name="using-the-tools"></a>Usando gli strumenti
---------------
Ogni volta che si richiama un comando, sono disponibili due progetti:

Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file. Per impostazione predefinita il progetto di destinazione il **progetto predefinito** selezionato nella Console di gestione pacchetti, ma può anche essere specificata utilizzando-parametro del progetto.

Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto. Per impostazione predefinita uno **imposta come progetto di avvio** in Esplora soluzioni. Può anche essere specificato usando il parametro - StartupProject.

Parametri comuni:

|                           |                             |
|:--------------------------|:----------------------------|
| -Contesto \<stringa >        | DbContext da utilizzare.       |
| -Progetto \<stringa >        | Il progetto da utilizzare.         |
| -StartupProject \<stringa > | Il progetto di avvio da usare. |
| -Verbose                  | Visualizzare output dettagliato.        |

Per visualizzare la Guida informazioni su un comando, usare PowerShell `Get-Help` comando.

> [!TIP]
> I parametri di contesto, progetto e oggetto StartupProject supportano l'espansione tramite tab.

> [!TIP]
> Impostare **env:ASPNETCORE_ENVIRONMENT** prima per specificare l'ambiente di ASP.NET Core in esecuzione.

<a name="commands"></a>Comandi:
--------

### <a name="add-migration"></a>Add-Migration

Aggiunge una nuova migrazione.

Parametri:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Name*** \<stringa >             | Il nome della migrazione.                                                                                       |
| <nobr>-OutputDir \<stringa ></nobr> | La directory e sub-spazio dei nomi da usare. I percorsi sono relativi alla directory del progetto. Il valore predefinito è "Migrazione". |

> [!NOTE]
> I parametri in **bold** sono obbligatori e quelle nella *corsivo* sono posizionali.

### <a name="drop-database"></a>Eliminazione di Database

Elimina il database.

Parametri:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Mostra in quale database verrà rimossa, ma non eliminarlo. |

### <a name="get-dbcontext"></a>Get-DbContext

Ottiene informazioni su un tipo DbContext.

### <a name="remove-migration"></a>Remove-Migration

Rimuove l'ultima migrazione.

Parametri:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Se è stato applicato al database, ripristinare la migrazione. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Esegue lo scaffolding di un tipi DbContext ed entità per un database.

Parametri:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connection*** \<stringa ></nobr> | La stringa di connessione al database.                                                           |
| ***-Provider*** \<stringa >                | Il provider da utilizzare. (ad esempio, l'entityframeworkcore)                      |
| -OutputDir \<stringa >                     | La directory in cui inserire file nella. I percorsi sono relativi alla directory del progetto.                      |
| -ContextDir \<stringa >                    | La directory in cui inserire file DbContext in. I percorsi sono relativi alla directory del progetto.             |
| -Contesto \<stringa >                       | Il nome di DbContext da generare.                                                           |
| -Schemi \<String [] >                     | Gli schemi delle tabelle per generare i tipi di entità per.                                              |
| -Tabelle \<String [] >                      | Generare tipi di entità per le tabelle.                                                         |
| -DataAnnotations                         | Usare gli attributi per configurare il modello (se possibile). Se omesso, viene utilizzato solo l'API fluent. |
| -UseDatabaseNames                        | Usare nomi di tabella e colonna direttamente dal database.                                           |
| -Force                                   | Sovrascrivi file esistenti.                                                                        |

### <a name="script-migration"></a>Migrazione degli script

Genera uno script SQL dalle migrazioni.

Parametri:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<stringa > | La migrazione inizia. Il valore predefinito è 0 (il database iniziale).      |
| *-A* \<stringa >   | La migrazione finale. Per impostazione predefinita all'ultima migrazione.              |
| -Idempotenti       | Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione. |
| -Output \<stringa > | File in cui scrivere il risultato.                                   |

> [!TIP]
> To, From, e i parametri di Output supportano espansione tramite tab.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migrazione* \<stringa ></nobr> | La migrazione di destinazione. Se è '0', verranno ripristinate tutte le migrazioni. Per impostazione predefinita all'ultima migrazione. |

> [!TIP]
> Il parametro di migrazione supporta l'espansione tramite tab.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
