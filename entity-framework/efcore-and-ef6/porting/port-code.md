---
title: Porting da EF6 a EF Core-porting di un modello basato sul codice-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181222"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Porting di un modello basato su codice EF6 per EF Core

Se sono state lette tutte le avvertenze e si è pronti per la porta, di seguito sono riportate alcune linee guida utili per iniziare.

## <a name="install-ef-core-nuget-packages"></a>Installare EF Core pacchetti NuGet

Per usare EF Core, installare il pacchetto NuGet per il provider di database che si vuole usare. Ad esempio, quando la destinazione è SQL Server, è necessario installare `Microsoft.EntityFrameworkCore.SqlServer`. Per informazioni dettagliate, vedere [provider di database](../../core/providers/index.md) .

Se si prevede di usare le migrazioni, è necessario installare anche il pacchetto `Microsoft.EntityFrameworkCore.Tools`.

È consigliabile lasciare installato il pacchetto NuGet EF6 (EntityFramework), perché EF Core e EF6 possono essere utilizzati side-by-side nella stessa applicazione. Tuttavia, se non si intende usare EF6 in tutte le aree dell'applicazione, la disinstallazione del pacchetto consente di ottenere errori di compilazione su frammenti di codice che richiedono attenzione.

## <a name="swap-namespaces"></a>Scambia spazi dei nomi

La maggior parte delle API usate in EF6 si trova nello spazio dei nomi `System.Data.Entity` (e nei relativi spazi dei nomi secondari). La prima modifica del codice prevede lo scambio nello spazio dei nomi `Microsoft.EntityFrameworkCore`. Si inizia in genere con il file di codice del contesto derivato, quindi si esegue questa operazione, risolvendo gli errori di compilazione man mano che si verificano.

## <a name="context-configuration-connection-etc"></a>Configurazione del contesto (connessione e così via)

Come descritto in [assicurarsi che EF Core funzionerà per l'applicazione](ensure-requirements.md), EF core ha meno magie per il rilevamento del database a cui connettersi. È necessario eseguire l'override del metodo `OnConfiguring` nel contesto derivato e utilizzare l'API specifica del provider di database per configurare la connessione al database.

La maggior parte delle applicazioni EF6 archivia la stringa di connessione nel file Applications `App/Web.config`. In EF Core la stringa di connessione viene letta usando l'API `ConfigurationManager`. Per poter usare questa API, potrebbe essere necessario aggiungere un riferimento all'assembly del Framework `System.Configuration`.

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

A questo punto, è necessario risolvere gli errori di compilazione ed esaminare il codice per verificare se le modifiche apportate al comportamento incidono sull'utente.

## <a name="existing-migrations"></a>Migrazioni esistenti

Non esiste un modo davvero possibile per trasferire le migrazioni di EF6 esistenti in EF Core.

Se possibile, è preferibile presupporre che tutte le migrazioni precedenti da EF6 siano state applicate al database e quindi avviare la migrazione dello schema da tale punto utilizzando EF Core. A tale scopo, utilizzare il comando `Add-Migration` per aggiungere una migrazione una volta che il modello viene trasferito al EF Core. Sarà quindi necessario rimuovere tutto il codice dai metodi `Up` e `Down` della migrazione con impalcatura. Le migrazioni successive vengono confrontate con il modello quando la migrazione iniziale è stata sottoposta a impalcatura.

## <a name="test-the-port"></a>Testare la porta

Solo perché l'applicazione viene compilata, non significa che la porta è stata eseguita correttamente per EF Core. È necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche del comportamento abbia avuto un impatto negativo sull'applicazione.
