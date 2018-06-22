---
title: Porting da EF6 a EF Core - il Porting di un modello basato su codice
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052951"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Porting di un modello basato su codice EF6 a EF Core

Se sei arrivato a leggere tutti i problemi e si è pronti a porta, di seguito sono riportate alcune linee guida che consentono di iniziare.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet Core EF

Per l'utilizzo di Entity Framework Core, si installa il pacchetto NuGet per il provider di database che si desidera utilizzare. Ad esempio, se la destinazione SQL Server, è preferibile installare `Microsoft.EntityFrameworkCore.SqlServer`. Vedere [i provider di Database](../../core/providers/index.md) per informazioni dettagliate.

Se si intende utilizzare migrazioni, quindi è necessario installare anche il `Microsoft.EntityFrameworkCore.Tools` pacchetto.

È possibile lasciare il pacchetto NuGet EF6 (EntityFramework) installato, come EF Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione. Tuttavia, se non si intende utilizzare EF6 nelle aree dell'applicazione, quindi disinstallare il pacchetto consente di assegnare gli errori di compilazione in parti di codice che richiedono attenzione.

## <a name="swap-namespaces"></a>Scambia gli spazi dei nomi

La maggior parte delle API che si utilizzano in EF6 presenti il `System.Data.Entity` dello spazio dei nomi (e correlate sottospazi dei nomi). È la prima modifica di codice da scambiare per il `Microsoft.EntityFrameworkCore` dello spazio dei nomi. In genere iniziare con il file di codice di contesto derivata e quindi tenta di risolvere da qui, risolvere gli errori di compilazione che si verificano.

## <a name="context-configuration-connection-etc"></a>Configurazione del contesto (connessione e così via).

Come descritto in [verificare EF verrà operazione principale per l'applicazione](ensure-requirements.md), Core EF ha meno chiave per la connessione al database di rilevamento. È necessario eseguire l'override di `OnConfiguring` metodo sul contesto derivata e utilizzare l'API specifiche del provider di database per la connessione al database del programma di installazione.

La maggior parte delle applicazioni EF6 archiviano la stringa di connessione nelle applicazioni `App/Web.config` file. In Entity Framework Core, leggere la stringa di connessione utilizzando il `ConfigurationManager` API. Potrebbe essere necessario aggiungere un riferimento di `System.Configuration` assembly framework per poter usare questa API.

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

A questo punto, si tratta di errori di compilazione di indirizzamento e revisione del codice per vedere se le modifiche del comportamento è necessario tenere conto.

## <a name="existing-migrations"></a>Migrazioni esistente

Non è effettivamente di porta esistente migrazioni EF6 a EF Core in modo appropriato.

Se possibile, è consigliabile presupporre che tutte le migrazioni precedenti da EF6 sono state applicate al database e quindi avviare la migrazione dello schema da che punto usando EF Core. A tale scopo, utilizzare il `Add-Migration` comando per aggiungere una migrazione dopo che il modello viene trasferito a EF Core. Quindi rimuovere tutto il codice dal `Up` e `Down` metodi della migrazione di scaffolding. Migrazioni successive verranno confrontate al modello quando è stata scaffolding che la migrazione iniziale.

## <a name="test-the-port"></a>La porta di test

Il fatto che l'applicazione viene compilata, non significa che correttamente eseguito il porting a EF Core. È necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche del comportamento hanno effetti negativi sull'applicazione.
