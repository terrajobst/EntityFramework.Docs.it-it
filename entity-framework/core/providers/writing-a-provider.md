---
title: Scrittura di un provider di database-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417883"
---
# <a name="writing-a-database-provider"></a>Scrittura di un provider di database

Per informazioni sulla scrittura di un provider di database Entity Framework Core, vedere la pagina relativa alla scrittura di [un provider EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) da [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Questi post non sono stati aggiornati a partire dal EF Core 1,1 e sono state apportate modifiche significative a partire da quel momento il [problema 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) è tenere traccia degli aggiornamenti a questa documentazione.

Il EF Core codebase è open source e contiene diversi provider di database che possono essere utilizzati come riferimento. È possibile trovare il codice sorgente in <https://github.com/aspnet/EntityFrameworkCore>. Potrebbe anche essere utile esaminare il codice per i provider di terze parti di uso comune, ad esempio [npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)e [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). In particolare, questi progetti sono impostati per estendere ed eseguire test funzionali pubblicati in NuGet. Questo tipo di installazione è fortemente consigliato.

## <a name="keeping-up-to-date-with-provider-changes"></a>Mantenersi aggiornati sulle modifiche del provider

A partire da lavoro dopo la versione 2,1, è stato creato un [log di modifiche](provider-log.md) che potrebbero richiedere modifiche corrispondenti nel codice del provider. Questa operazione è utile quando si aggiorna un provider esistente per l'utilizzo con una nuova versione di EF Core.

Prima del 2,1, abbiamo usato le etichette [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) sui problemi di GitHub e le richieste pull per uno scopo simile. Si continiue di usare queste etichette nei problemi per indicare quali elementi di lavoro in una determinata versione potrebbero richiedere il lavoro da eseguire nei provider. Un'etichetta di `providers-beware` in genere significa che l'implementazione di un elemento di lavoro potrebbe interrompere i provider, mentre un'etichetta `providers-fyi` in genere significa che i provider non verranno interrotti, ma potrebbe essere necessario modificare il codice comunque, ad esempio, per abilitare nuove funzionalità.

## <a name="suggested-naming-of-third-party-providers"></a>Denominazione consigliata per i provider di terze parti

È consigliabile usare i nomi seguenti per i pacchetti NuGet. Questo è coerente con i nomi dei pacchetti forniti dal team EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Ad esempio:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
