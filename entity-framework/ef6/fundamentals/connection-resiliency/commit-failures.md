---
title: La gestione degli errori di commit transaction - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250861"
---
# <a name="handling-transaction-commit-failures"></a>Gestione degli errori di commit delle transazioni
> [!NOTE]
> **EF6.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 6.1. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Come parte di 6.1 viene introdotto una nuova funzionalità di resilienza di connessione per Entity Framework: la possibilità di rilevare e correggere automaticamente quando gli errori di connessione temporanei interessano l'acknowledgement del commit della transazione. I dettagli completi dello scenario sono meglio descritti nel post di blog [connettività del Database SQL e il problema di idempotenza](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  In breve, lo scenario è che, quando viene generata un'eccezione durante il commit di una transazione, ci sono due le cause possibili:  

1. Il commit della transazione non riuscita nel server
2. Il commit della transazione ha avuto esito positivo nel server, ma un problema di connettività ha impedito la notifica di esito positivo di raggiungere il client  

Quando la prima situazione si verifica l'applicazione o l'utente può ripetere l'operazione, ma quando si verifica la situazione secondo i tentativi devono essere evitati e l'applicazione è stato possibile recuperare automaticamente. Il problema è che senza la possibilità di rilevare Qual era il motivo effettivo è stato segnalato un'eccezione durante il commit, l'applicazione non è possibile scegliere la scelta giusta dell'azione. La nuova funzionalità in EF 6.1 consente di controllare con il database se la transazione ha avuto esito positivo e Segui il corso a destra dell'azione in modo trasparente a Entity Framework.  

## <a name="using-the-feature"></a>Uso della funzionalità  

Per abilitare la funzionalità è necessario includere una chiamata a [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) nel costruttore delle **DbConfiguration**. Se non si ha familiarità con **DbConfiguration**, vedere [configurazione basata su codice](~/ef6/fundamentals/configuring/code-based.md). Questa funzionalità può essere utilizzata in combinazione con la ripetizione automatica dei tentativi abbiamo introdotto in EF6, che consentono la situazione in cui la transazione effettivamente non è stato possibile eseguire il commit nel server a causa di un errore temporaneo:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>Modalità di rilevamento delle transazioni  

Quando è abilitata la funzionalità, Entity Framework aggiungerà automaticamente una nuova tabella al database denominato **__Transactions**. In questa tabella viene inserita una nuova riga ogni volta che viene creata una transazione da Entity Framework e che la riga viene verificata l'esistenza se si verifica un errore della transazione durante il commit.  

Benché EF eseguirà sforzo possibile escludere le righe della tabella quando non sono più necessari, la tabella può aumentare se l'applicazione viene chiusa in modo anomalo e per tale motivo, che potrebbe essere necessario eliminare la tabella manualmente in alcuni casi.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Come gestire gli errori di commit con le versioni precedenti

Prima di 6.1 di Entity Framework non era meccanismo per gestire gli errori di commit all'interno del prodotto di Entity Framework. Esistono diversi modi per gestire questa situazione che può essere applicata alle versioni precedenti di Entity Framework 6:  

* Opzione 1: non eseguire alcuna operazione  

  La probabilità di un errore di connessione durante il commit della transazione è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.  

* Opzione 2: usare il database per reimpostare lo stato  

  1. Eliminare DbContext corrente  
  2. Creare un nuovo DbContext e ripristinare lo stato dell'applicazione dal database  
  3. Informare l'utente che l'ultima operazione potrebbe non siano stata completata correttamente  

* Opzione 3: tenere manualmente traccia della transazione  

  1. Aggiungere una tabella con rilevamento nel database usato per tenere traccia dello stato delle transazioni.  
  2. Inserire una riga nella tabella all'inizio di ogni transazione.  
  3. Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.  
     - Se la riga è presente, continua a funzionare normalmente, come la transazione è stato eseguito il commit  
     - Se la riga è assente, è possibile usare una strategia di esecuzione per ripetere l'operazione corrente.  
  4. Se il commit ha esito positivo, eliminare la riga corrispondente per evitare la crescita della tabella.  

[Questo post di blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contiene codice di esempio per questa operazione in SQL Azure.  
