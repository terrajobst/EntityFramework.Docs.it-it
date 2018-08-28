---
title: Scrittura di un Provider di Database - EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993666"
---
# <a name="writing-a-database-provider"></a>Scrittura di un Provider di Database

Per informazioni sulla scrittura di un provider di database di Entity Framework Core, vedere [in modo che si desidera scrivere un provider di Entity Framework Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) dal [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Questi post non sono stati aggiornati a partire da EF Core 1.1 e sono state apportate modifiche significative da quel momento [problema 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) tiene traccia degli aggiornamenti a questa documentazione.

La codebase di EF Core è open source e contiene diversi provider di database che può essere usato come riferimento. È possibile trovare il codice sorgente in https://github.com/aspnet/EntityFrameworkCore. Potrebbe anche essere utile esaminare il codice per i provider di terze parti usati, ad esempio [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [MySQL Pomelo](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), e [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). In particolare, questi progetti sono il programma di installazione si estendono da ed eseguire i test funzionali che vengono pubblicati in NuGet. Questo tipo di programma di installazione è fortemente consigliato.

## <a name="keeping-up-to-date-with-provider-changes"></a>Stare al passo con le modifiche di provider

A partire da lavoro dopo la versione 2.1, è stata creata una [registro delle modifiche](provider-log.md) che potrebbe essere necessario le modifiche corrispondenti nel codice del provider. Si tratta della Guida quando si aggiorna un provider esistente per lavorare con una nuova versione di Entity Framework Core.

Prima di 2.1, abbiamo utilizzato la [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette nei nostri problemi di GitHub e richieste pull per uno scopo simile. Ci impegniamo per continuare per usare le etichette di questi problemi per fornire un'indicazione di quali elementi di lavoro in una determinata versione richieda anche il lavoro da eseguire nei provider. Oggetto `providers-beware` etichetta in genere significa che l'implementazione di un elemento di lavoro può comportare l'interruzione provider, mentre un `providers-fyi` etichetta in genere significa che i provider non verrà interrotti, ma potrebbe essere necessario codice da modificare, ad esempio, per abilitare nuove funzionalità.

## <a name="suggested-naming-of-third-party-providers"></a>Suggerito di denominazione dei provider di terze parti

È consigliabile usare i seguenti nomi per i pacchetti NuGet. Ciò è coerenza con i nomi di pacchetti recapitati dal team di EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Ad esempio:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
