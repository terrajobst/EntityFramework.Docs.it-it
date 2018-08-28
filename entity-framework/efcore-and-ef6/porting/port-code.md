---
title: Conversione da EF6 a EF Core - conversione di un modello basato su codice
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997049"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Porting di un modello basato su codice EF6 a EF Core

Se sei arrivato a leggere tutti i problemi e si è pronti a porta, ecco alcune linee guida che consentono di iniziare.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet di Entity Framework Core

Per usare EF Core, si installa il pacchetto NuGet per il provider di database da usare. Ad esempio, quando la destinazione SQL Server, è preferibile installare `Microsoft.EntityFrameworkCore.SqlServer`. Visualizzare [provider di Database](../../core/providers/index.md) per informazioni dettagliate.

Se si prevede di usare le migrazioni, quindi è consigliabile installare anche il `Microsoft.EntityFrameworkCore.Tools` pacchetto.

È possibile lasciare il pacchetto NuGet di Entity Framework 6 (Entity Framework) installato, come EF Core ed EF6 può essere utilizzata side-by-side nella stessa applicazione. Tuttavia, se non si intende usare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto documentazione offre errori di compilazione in parti di codice che richiedono attenzione.

## <a name="swap-namespaces"></a>Scambia gli spazi dei nomi

La maggior parte delle API che usano in EF6 si trovano nel `System.Data.Entity` dello spazio dei nomi (e relativi spazi dei nomi secondari). La prima modifica di codice consiste nell'invertire al `Microsoft.EntityFrameworkCore` dello spazio dei nomi. In genere iniziare con il file di codice del contesto derivato e quindi scoprire da qui, risolvere gli errori di compilazione appena si verificano.

## <a name="context-configuration-connection-etc"></a>Configurazione del contesto (connessione e così via).

Come descritto in [assicurarsi che EF Core verrà operazioni dell'applicazione](ensure-requirements.md), EF Core ha meno magic intorno a rilevare il database a cui connettersi. È necessario eseguire l'override di `OnConfiguring` (metodo) nel contesto derivato e usare l'API specifiche del provider di database per configurare la connessione al database.

La maggior parte delle applicazioni EF6 archiviano la stringa di connessione nelle applicazioni `App/Web.config` file. In EF Core, leggere questa stringa di connessione usando il `ConfigurationManager` API. Potrebbe essere necessario aggiungere un riferimento al `System.Configuration` assembly del framework per poter usare questa API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Aggiornare il codice

A questo punto, è una questione di indirizzamento errori di compilazione e revisione del codice per vedere se le modifiche del comportamento possibili conseguenze per te.

## <a name="existing-migrations"></a>Migrazioni esistenti

Non c'è davvero una modalità fattibile per trasferire le migrazioni esistenti EF6 a EF Core.

Se possibile, è consigliabile presupporre che tutti i precedenti migrazioni da EF6 sono state applicate al database e quindi avvia la migrazione dello schema da che punto di usando Entity Framework Core. A tale scopo, si utilizzerebbe il `Add-Migration` comando per aggiungere una migrazione una volta il modello viene trasferito a EF Core. Quindi rimuovere tutto il codice dal `Up` e `Down` metodi della migrazione con scaffolding. Migrazioni successive confronterà al modello quando è stato eseguito lo scaffolding che la migrazione iniziale.

## <a name="test-the-port"></a>La porta di test

Il fatto che l'applicazione viene compilata, non significa che è al termine del trasferimento in EF Core. È necessario testare tutte le aree dell'applicazione per garantire che nessuna delle modifiche di comportamento hanno effetti negativi a causa dell'applicazione.
