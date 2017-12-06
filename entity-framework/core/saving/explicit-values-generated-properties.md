---
title: "L'impostazione di valori espliciti per le proprietà generate - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>L'impostazione di valori espliciti per le proprietà generate

Una proprietà generata è una proprietà il cui valore viene generato (tramite EF o database) quando l'entità è aggiunto e/o aggiornato. Vedere [generato proprietà](../modeling/generated-properties.md) per ulteriori informazioni.

Potrebbero verificarsi situazioni in cui si desidera impostare un valore esplicito per una proprietà generata, anziché un oggetto generato.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) su GitHub.

## <a name="the-model"></a>Il modello

Il modello utilizzato in questo articolo contiene un singolo `Employee` entità.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Salvataggio di un valore esplicito durante l'aggiunta

Il `Employee.EmploymentStarted` proprietà è configurata per contenere i valori generati dal database per le nuove entità (utilizzando un valore predefinito).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Il codice seguente inserisce due dipendenti nel database.
* Per la prima, viene assegnato alcun valore per `Employee.EmploymentStarted` , in modo che rimanga impostata sul valore predefinito CLR per `DateTime`.
* Impostata per il secondo e un valore esplicito di `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Output mostra che il database genera un valore per il primo dipendente e che è stato utilizzato il valore esplicito per il secondo.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Valori espliciti nelle colonne IDENTITY di SQL Server

Per convenzione il `Employee.EmployeeId` proprietà è un archivio generato `IDENTITY` colonna.

Per la maggior parte dei casi, l'approccio illustrato in precedenza funzioneranno per le proprietà chiave. Tuttavia, per inserire valori espliciti in un Server SQL `IDENTITY` colonna, è necessario abilitare manualmente `IDENTITY_INSERT` prima di chiamare `SaveChanges()`.

> [!NOTE]  
> Abbiamo un [richiesta funzionalità](https://github.com/aspnet/EntityFramework/issues/703) nel nostro backlog per eseguire questa operazione automaticamente all'interno del provider SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Output mostra che gli ID specificati sono stati salvati nel database.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>L'impostazione di un valore esplicito durante l'aggiornamento

Il `Employee.LastPayRaise` proprietà è configurata per contenere i valori generati dal database durante gli aggiornamenti.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Per impostazione predefinita, Core EF genererà un'eccezione se si tenta di salvare un valore esplicito per una proprietà che è configurato per essere generati durante l'aggiornamento. Per evitare questo problema, è necessario per l'elenco a discesa di basso livello API dei metadati e impostare il `AfterSaveBehavior` (come illustrato in precedenza).

> [!NOTE]  
> **Le modifiche in EF Core 2.0:** nelle versioni precedenti il comportamento di salvataggio post è stato controllato tramite il `IsReadOnlyAfterSave` flag. Questo flag è stato obsoleto e sostituito da `AfterSaveBehavior`.

È inoltre disponibile un trigger nel database per generare valori per il `LastPayRaise` colonna durante `UPDATE` operazioni.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Il codice seguente consente di aumentare lo stipendio di due dipendenti nel database.
* Per la prima, viene assegnato alcun valore per `Employee.LastPayRaise` proprietà, in modo che rimanga impostato su null.
* Per il secondo è stata impostata un valore esplicito di una settimana fa (generare back fino alla retribuzione).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Output mostra che il database genera un valore per il primo dipendente e che è stato utilizzato il valore esplicito per il secondo.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
