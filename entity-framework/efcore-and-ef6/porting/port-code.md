---
title: Conversione da EF6 a EF Core - Porting di un modello basato su codice - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419637"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Conversione di un modello basato su codice EF6 in EF Core

Se hai letto tutte le avvertenze e sei pronto per il porting, ecco alcune linee guida per aiutarti a iniziare.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet di EF CoreInstall EF Core NuGet packages

Per utilizzare EF Core, installare il pacchetto NuGet per il provider di database che si desidera utilizzare. Ad esempio, quando si esegue la `Microsoft.EntityFrameworkCore.SqlServer`destinazione di SQL Server, è necessario installare . Per informazioni [dettagliate,](../../core/providers/index.md) vedere Provider di database.

Se si prevede di utilizzare le migrazioni, `Microsoft.EntityFrameworkCore.Tools` è necessario installare anche il pacchetto.

È bene lasciare installato il pacchetto EF6 NuGet (EntityFramework), come Entity Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione. Tuttavia, se non si intende utilizzare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto contribuirà a dare errori di compilazione su parti di codice che richiedono attenzione.

## <a name="swap-namespaces"></a>Scambia spazi dei nomi

La maggior parte delle API utilizzate in `System.Data.Entity` EF6 si trovano nello spazio dei nomi (e negli spazi dei nomi secondari correlati). La prima modifica del codice `Microsoft.EntityFrameworkCore` consiste nello scambiare allo spazio dei nomi. In genere si inizia con il file di codice di contesto derivato e quindi elaborare da lì, risolvere gli errori di compilazione come si verificano.

## <a name="context-configuration-connection-etc"></a>Configurazione del contesto (connessione, ecc.)

Come descritto in [Assicurarsi che EF Core funzioni per l'applicazione](ensure-requirements.md), EF Core ha meno magia intorno il rilevamento del database a cui connettersi. È necessario eseguire `OnConfiguring` l'override del metodo nel contesto derivato e utilizzare l'API specifica del provider di database per configurare la connessione al database.

La maggior parte delle applicazioni EF6 archivia la stringa di connessione nel file di applicazioni. `App/Web.config` In EF Core, leggere questa `ConfigurationManager` stringa di connessione utilizzando l'API. Potrebbe essere necessario aggiungere un `System.Configuration` riferimento all'assembly del framework per poter utilizzare questa API.

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

A questo punto, si tratta di risolvere gli errori di compilazione e di esaminare il codice per vedere se le modifiche al comportamento avranno un impatto sull'utente.

## <a name="existing-migrations"></a>Migrazioni esistenti

Non esiste un modo fattibile per eseguire il porting delle migrazioni di EF6 esistenti a EF Core.There isn't really a feasible way to port existing EF6 migrations to EF Core.

Se possibile, è consigliabile presupporre che tutte le migrazioni precedenti da EF6 sono state applicate al database e quindi avviare la migrazione dello schema da quel punto utilizzando EF Core. A tale scopo, è `Add-Migration` necessario utilizzare il comando per aggiungere una migrazione una volta che il modello viene eseguito il porting a EF Core.To do this, you would use the command to add a migration once the model is ported to EF Core. È quindi necessario rimuovere `Up` tutto `Down` il codice dai metodi e della migrazione con scaffolding. Le migrazioni successive verranno confrontate con il modello quando è stato eseguito lo scaffolding della migrazione iniziale.

## <a name="test-the-port"></a>Testare la porta

Solo perché l'applicazione viene compilata, non significa che è stato eseguito correttamente il porting a EF Core.Just because your application compiles, does not mean it is successfully ported to EF Core. Sarà necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche di comportamento abbia influito negativamente sull'applicazione.
