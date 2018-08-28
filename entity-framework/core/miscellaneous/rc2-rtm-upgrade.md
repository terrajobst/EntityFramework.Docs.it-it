---
title: L'aggiornamento da EF Core 1.0 RC2 a RTM - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998319"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>L'aggiornamento da EF Core 1.0 RC2 a RTM

Questo articolo fornisce indicazioni per lo spostamento di un'applicazione compilata con i pacchetti RC2 1.0.0 RTM.

## <a name="package-versions"></a>Versioni dei pacchetti

I nomi dei pacchetti di livello superiore che viene in genere installato in un'applicazione non sono cambiati tra RC2 e RTM.

**È necessario aggiornare i pacchetti installati con le versioni RTM:**

* Pacchetti di runtime (ad esempio, `Microsoft.EntityFrameworkCore.SqlServer`) modificata `1.0.0-rc2-final` a `1.0.0`.

* Il `Microsoft.EntityFrameworkCore.Tools` pacchetto modificata `1.0.0-preview1-final` a `1.0.0-preview2-final`. Si noti che gli strumenti sono ancora versioni non definitive.

## <a name="existing-migrations-may-need-maxlength-added"></a>Le migrazioni esistenti potrebbe essere necessario maxLength aggiunto

In RC2, era simile alla definizione di colonna in una migrazione `table.Column<string>(nullable: true)` e la lunghezza della colonna è stata cercata in alcuni metadati vengono archiviati nel code-behind della migrazione. Nella versione RTM, la lunghezza è ora inclusa nel codice con scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

Le migrazioni esistenti che sono stati sottoposto a scaffolding prima di utilizzare RTM non avranno il `maxLength` argomento specificato. Ciò significa che la lunghezza massima supportata dal database verrà utilizzata (`nvarchar(max)` su SQL Server). Ciò potrebbe essere appropriato per alcune colonne, ma le colonne che fanno parte di una chiave, chiave esterna, o devono essere aggiornati per includere una lunghezza massima dell'indice. Per convenzione, 450 è la lunghezza massima usata per le chiavi, chiavi esterne e colonne indicizzate. Se è stato configurato in modo esplicito una lunghezza nel modello, quindi è necessario utilizzare tale lunghezza.

**ASP.NET Identity**

Questa modifica interessa i progetti che utilizzano ASP.NET Identity e sono stati creati da una pre-RTM modello di progetto. Il modello di progetto include una migrazione considerata di creare il database. Questa migrazione deve essere modificata per specificare una lunghezza massima di `256` per le colonne seguenti.

*  **AspNetRoles**

    * nome

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Errore per apportare questa modifica comporterà l'eccezione seguente durante la migrazione iniziale viene applicata a un database.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Rimuovere "imports" nel file Project. JSON

Se si era destinati a .NET Core con RC2, è necessario aggiungere `imports` a Project. JSON come soluzione alternativa temporanea per alcune delle dipendenze di EF Core non supporta .NET Standard. Questi possono a questo punto verranno rimossi.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> A partire dalla versione 1.0 RTM, la [.NET Core SDK](https://www.microsoft.com/net/download/core) non supporta più `project.json` o lo sviluppo di applicazioni .NET Core usando Visual Studio 2015. È consigliabile [eseguire la migrazione da project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Se si usa Visual Studio, è consigliabile eseguire l'aggiornamento a [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>Piattaforma UWP: Aggiungere reindirizzamenti di associazione

È stato effettuato un tentativo di eseguire comandi EF sui risultati dei progetti di Universal Windows Platform (UWP) l'errore seguente:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

È necessario aggiungere manualmente i reindirizzamenti di associazione per il progetto UWP. Creare un file denominato `App.config` nel progetto di cartella radice e aggiungere reindirizzamenti per le versioni degli assembly corretto.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
