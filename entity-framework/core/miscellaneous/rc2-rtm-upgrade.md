---
title: L'aggiornamento da Entity Framework Core 1.0 RC2 alla versione RTM - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151040"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>L'aggiornamento da Entity Framework Core 1.0 RC2 per RTM

In questo articolo vengono fornite indicazioni per lo spostamento di un'applicazione compilata con i pacchetti 1.0.0 RC2 RTM.

## <a name="package-versions"></a>Versioni del pacchetto

I nomi dei pacchetti di livello superiore che in genere installato in un'applicazione non è stata modificata da RC2 e RTM.

**È necessario aggiornare i pacchetti installati con le versioni RTM:**

* Fase di esecuzione pacchetti (ad esempio `Microsoft.EntityFrameworkCore.SqlServer`) modificato da `1.0.0-rc2-final` a `1.0.0`.

* Il `Microsoft.EntityFrameworkCore.Tools` pacchetto modificato da `1.0.0-preview1-final` a `1.0.0-preview2-final`. Si noti che gli strumenti ancora pre-release.

## <a name="existing-migrations-may-need-maxlength-added"></a>Le migrazioni esistenti potrebbe essere necessario maxLength aggiunto

In RC2, la definizione di colonna in una migrazione aveva `table.Column<string>(nullable: true)` e la lunghezza della colonna è stata ricercata in alcuni metadati vengono archiviati nel code-behind della migrazione. Nella versione RTM, la lunghezza è ora inclusa nel codice scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

Le migrazioni esistenti che sono stati scaffolding prima di utilizzare RTM non disporrà di `maxLength` argomento specificato. Ciò significa che la lunghezza massima supportata dal database verrà utilizzata (`nvarchar(max)` in SQL Server). Questo potrebbe essere appropriato per alcune colonne, ma le colonne che fanno parte di una chiave, foreign key o indice devono essere aggiornati per includere una lunghezza massima. Per convenzione, 450 è la lunghezza massima utilizzata per le chiavi esterne, chiavi e colonne indicizzate. Se è stato configurato in modo esplicito una lunghezza nel modello, quindi è necessario utilizzare tale lunghezza.

**Identità di ASP.NET**

Questa modifica influisce su progetti che utilizzano ASP.NET Identity e sono stati creati da un pre-RTM modello di progetto. Il modello di progetto include una migrazione utilizzata per creare il database. Questa migrazione deve essere modificata per specificare una lunghezza massima di `256` per le colonne seguenti.

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

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Rimuovere "Importa" in Project. JSON

Se si fosse destinato a .NET Core con RC2, è necessario aggiungere `imports` a JSON come soluzione alternativa temporanea per alcune delle dipendenze del Core EF non supporta .NET Standard. Possono ora essere rimossi.

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
> A partire dalla versione 1.0 RTM, il [.NET Core SDK](https://www.microsoft.com/net/download/core) non supporta più `project.json` o lo sviluppo di applicazioni .NET Core usando Visual Studio 2015. È consigliabile [eseguire la migrazione da project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Se si utilizza Visual Studio, è consigliabile eseguire l'aggiornamento a [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>Piattaforma UWP: Aggiungere reindirizzamenti di associazione

Il tentativo di eseguire comandi EF nei risultati dei progetti Universal Windows Platform (UWP) l'errore seguente:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

È necessario aggiungere manualmente i reindirizzamenti di associazione per il progetto UWP. Creare un file denominato `App.config` nel progetto di cartella radice e aggiungere reindirizzamenti di per le versioni degli assembly corretto.

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
