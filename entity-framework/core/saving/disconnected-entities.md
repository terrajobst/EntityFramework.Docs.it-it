---
title: "Entità disconnesse - Core EF"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a>Entità disconnesse

Un'istanza di DbContext rileverà automaticamente le entità restituite dal database. Le modifiche apportate a queste entità verranno quindi rilevate quando viene chiamato SaveChanges e il database verrà aggiornato in base alle esigenze. Vedere [salvare base](basic.md) e [dati correlati](related-data.md) per informazioni dettagliate.

Tuttavia, talvolta le entità vengono interrogate utilizzando un'unica istanza di contesto e quindi salvati utilizzando un'istanza diversa. Questo accade spesso "disconnessi" scenari, ad esempio un'applicazione web in cui le entità sono query inviate al client, modificate, inviate al server in una richiesta e quindi salvate. In questo caso, il contesto della secondo istanza deve sapere se le entità sono nuovo (deve essere inserito) o esistente (deve essere aggiornato).

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) di questo articolo in GitHub.

## <a name="identifying-new-entities"></a>Identificazione di nuove entità

### <a name="client-identifies-new-entities"></a>Client identifica nuove entità

Il caso più semplice da affrontare è quando il client comunica al server se l'entità è nuovo o esistente. Ad esempio, spesso la richiesta di inserire una nuova entità è diversa dalla richiesta per aggiornare un'entità esistente.

Nella parte restante di questa sezione vengono illustrati i casi in cui è necessario determinare in altro modo per inserire o aggiornare.

### <a name="with-auto-generated-keys"></a>Con le chiavi generate automaticamente

Il valore di una chiave generata automaticamente spesso può essere utilizzato per determinare se un'entità deve essere inserita o aggiornata. Se la chiave non è stato impostato (ad esempio ancora il valore predefinito CLR di null, zero e così via), quindi l'entità deve essere nuovo e inserimento. D'altra parte, se è stato impostato il valore della chiave, quindi deve essere già stato precedentemente salvato e ora è necessario aggiornare. In altre parole, se la chiave ha un valore, quindi l'entità è stata eseguita una query, inviato al client e ha ora tornare da aggiornare.

È facile verificare la presenza di una chiave non impostata quando è noto il tipo di entità:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Tuttavia, EF dispone anche di un metodo incorporato per eseguire questa operazione per qualsiasi tipo di entità e tipo di chiave:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Chiavi sono impostate come entità vengono rilevate dal contesto, anche se l'entità è nello stato Added. In questo modo, quando si attraversano un grafico di entità e decidere cosa fare con ogni tipo, ad esempio, quando si utilizza l'API TrackGraph. Il valore della chiave deve essere utilizzato solo nel modo illustrato di seguito _prima_ viene effettuata qualsiasi chiamata a tenere traccia dell'entità.

### <a name="with-other-keys"></a>Con altre chiavi

Un altro meccanismo è necessario per identificare nuove entità quando i valori di chiave non vengono generati automaticamente. Esistono due approcci generali per questo:
 * Query per l'entità
 * Passare un flag dal client

Per eseguire una query per l'entità, è sufficiente utilizzare il metodo di ricerca:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Rientra nell'ambito di questo documento per visualizzare il codice completo per il passaggio di un flag da un client. In un'app web, in genere significa che effettua richieste diverse per diverse azioni, o passando a uno stato della richiesta, quindi estraendolo nel controller.

## <a name="saving-single-entities"></a>Salvataggio delle entità singola

Se è noto o meno un'istruzione insert o update è necessaria, quindi Aggiungi o aggiorna può essere utilizzato in modo appropriato:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Tuttavia, se l'entità utilizza valori di chiave generato automaticamente, il metodo Update può essere utilizzato per entrambi i casi:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Il metodo di aggiornamento è in genere contrassegna l'entità per l'aggiornamento, inserimento non. Tuttavia, l'entità dispone di una chiave generata automaticamente, se non è stato impostato alcun valore di chiave, quindi l'entità invece viene contrassegnato automaticamente per inserire.

> [!TIP]  
> Questo comportamento è stato introdotto in Entity Framework Core 2.0. Per le versioni precedenti è sempre necessario selezionare in modo esplicito l'opzione Aggiungi o Aggiorna.

Se l'entità non utilizza le chiavi generate automaticamente, quindi l'applicazione deve decidere se l'entità deve essere inserita o aggiornata: ad esempio:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Ecco i passaggi sono:
* Se trova restituisce null, il database non contiene già il blog con tale ID, pertanto è chiamare aggiungere contrassegnarla per l'inserimento.
* Se trova restituisce un'entità, quindi esista nel database e il contesto ora sta rilevando l'entità esistente
  * È quindi possibile utilizzare SetValues per impostare i valori per tutte le proprietà nell'entità a quelle fornite dal client.
  * La chiamata SetValues contrassegnerà l'entità da aggiornare in base alle esigenze.

> [!TIP]  
> SetValues solo viene contrassegnato come modificato le proprietà che hanno valori diversi da quelli in entità rilevata. Ciò significa che quando viene inviato l'aggiornamento, verranno aggiornate solo le colonne effettivamente modificati. (E se è stato modificato nulla, verrà inviato alcun aggiornamento affatto.)

## <a name="working-with-graphs"></a>Utilizzo dei grafici

### <a name="all-newall-existing-entities"></a>Tutte le entità esistenti con nuove/all

Un esempio di utilizzo di grafici inserimento o aggiornamento di un blog con la raccolta di messaggi associati. Se tutte le entità nel grafico devono essere inserite o devono essere aggiornati, il processo è identico a quello descritto sopra per singole entità. Ad esempio, un grafico di blog e annunci simile al seguente:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

è possibile inserire simile al seguente:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

La chiamata al metodo Add contrassegna il blog che tutti i post da inserire.

Analogamente, se necessario aggiornare tutte le entità in un grafico, quindi aggiornamento può essere utilizzato:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Il blog e tutte le relative pubblicazioni verranno contrassegnate per essere aggiornati.

### <a name="mix-of-new-and-existing-entities"></a>Combinazione di entità nuove ed esistenti

Con le chiavi generate automaticamente, Update può essere utilizzato nuovamente per gli inserimenti e aggiornamenti, anche se il grafico contiene una combinazione di entità che richiedono l'inserimento e quelli che richiedono l'aggiornamento:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aggiornamento contrassegnerà qualsiasi entità nel grafico, blog o post, per l'inserimento, se non dispone di un set di valore della chiave, mentre tutte le altre entità sono contrassegnati per l'aggiornamento.

Come prima, se non si usa chiavi generate automaticamente, una query e alcune operazioni di elaborazione possono essere utilizzati:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Gestione delle eliminazioni

Delete può essere difficile da gestire dall'assenza di un'entità indica spesso che deve essere eliminato. Un modo per risolvere questo problema consiste nell'utilizzare "eliminazioni reversibili" in modo che l'entità viene contrassegnato come eliminato anziché effettivamente in corso l'eliminazione. Elimina quindi assume lo stesso come aggiornamenti. Eliminazioni reversibili possono essere implementate utilizzando [i filtri di query](xref:core/querying/filters).

Per le eliminazioni true, un modello comune consiste nell'utilizzare un'estensione del modello di query per eseguire essenzialmente diff. un grafico Ad esempio:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Internamente, Add, collegamento e aggiornamento utilizzare attraversamento del grafico con una determinazione eseguita per ogni entità come se si deve essere contrassegnato come Added (per inserire), Modified (aggiornare), non modificato (non eseguire alcuna operazione), o eliminati (eliminare). Questo meccanismo viene esposta tramite l'API TrackGraph. Ad esempio, si supponga che, quando il client invia un grafico di entità imposta alcuni flag per ogni entità che indica la modalità di gestione. TrackGraph può quindi essere utilizzato per elaborare questo flag:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

I flag vengono visualizzati solo come parte dell'entità per semplicità dell'esempio. In genere i flag potrebbero far parte di un DTO o qualche altro stato incluso nella richiesta.
