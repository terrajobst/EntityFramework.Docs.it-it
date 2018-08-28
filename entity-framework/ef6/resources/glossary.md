---
title: Entity Framework glossario - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4ad56c4d655e004d97c3537707fa6b13c7acf88e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997719"
---
# <a name="entity-framework-glossary"></a>Glossario di Entity Framework
## <a name="code-first"></a>Code First
Creazione di un modello di Entity Framework usando il codice. Il modello può avere come destinazione e di database esistente o di un nuovo database.

## <a name="context"></a>Contesto
Una classe che rappresenta una sessione con il database, consentendo di eseguire una query e salvare i dati. Un contesto deriva dalla classe ObjectContext o DbContext.

## <a name="convention-code-first"></a>Convenzione (Code First)
Una regola che usa Entity Framework per dedurre la forma del modello dalle classi.

## <a name="database-first"></a>Prima di tutto di database
Creazione di un modello di Entity Framework, tramite la finestra di progettazione di Entity Framework, che è destinato a un database esistente.

## <a name="eager-loading"></a>Caricamento eager
Modello di caricamento dei dati correlati in una query per un tipo di entità carica anche entità correlate come parte della query.

## <a name="ef-designer"></a>EF Designer
Una finestra di progettazione visiva di Visual Studio che consente di creare un modello di Entity Framework usando le caselle e linee.

## <a name="entity"></a>Entità
Classe o oggetto che rappresenta i dati dell'applicazione come clienti, prodotti e ordini.

## <a name="entity-data-model"></a>Entity Data Model
Un modello che descrive le entità e le relazioni tra di essi. Entity Framework utilizza EDM per descrivere il modello concettuale in cui i programmi per sviluppatori. EDM si basa sul modello entità-relazione introdotto dal ripristino di emergenza. Peter Chen. Il modello EDM è stato originariamente sviluppato con l'obiettivo primario di divenire common data model in una suite di tecnologie per sviluppatori e il server da Microsoft. Modello EDM viene usato anche come parte del protocollo OData.

## <a name="explicit-loading"></a>Caricamento esplicito
Modello di caricamento dei dati correlati in cui gli oggetti correlati vengono caricati chiamando un'API.

## <a name="fluent-api"></a>API Fluent
Un'API che può essere usata per configurare un modello Code First.

## <a name="foreign-key-association"></a>Associazione di chiavi esterne
Un'associazione tra entità in cui una proprietà che rappresenta la chiave esterna è incluso nella classe dell'entità dipendente. Ad esempio, prodotto contiene una proprietà CategoryId.

## <a name="identifying-relationship"></a>Relazione di identificazione
Relazione in cui la chiave primaria dell'entità principale fa parte della chiave primaria dell'entità dipendente. In questo tipo di relazione, l'entità dipendente non può esistere senza l'entità principale.

## <a name="independent-association"></a>Associazione indipendente
Un'associazione tra entità in cui è presente alcuna proprietà che rappresenta la chiave esterna nella classe dell'entità dipendente. Ad esempio, una classe di prodotto contiene una relazione per categoria, ma nessuna proprietà CategoryId. Entity Framework rileva lo stato dell'associazione indipendentemente dallo stato delle entità finali dell'associazione di due.

## <a name="lazy-loading"></a>Caricamento lazy
Modello di caricamento dei dati correlati in cui gli oggetti correlati vengono caricati automaticamente quando si accede a una proprietà di navigazione.

## <a name="model-first"></a>A partire dal modello
Creazione di un modello di Entity Framework, tramite la finestra di progettazione di Entity Framework, che viene quindi usato per creare un nuovo database.

## <a name="navigation-property"></a>Proprietà di navigazione
Proprietà di un'entità a cui fa riferimento a un'altra entità. Ad esempio, prodotto contiene una proprietà di navigazione di categoria e categoria contiene una proprietà di navigazione di prodotti.

## <a name="poco"></a>POCO
Acronimo di Plain-Old CLR Object. Classe utente semplice che non ha dipendenze con qualsiasi framework. Nel contesto di Entity Framework, una classe di entità che non deriva da EntityObject, implementa le interfacce o contiene tutti gli attributi definiti in Entity Framework. Tali classi di entità che vengono disaccoppiati da framework di persistenza sono anche detto "non riconoscono la persistenza".  

## <a name="relationship-inverse"></a>Inverso di relazione
Estremo opposto di una relazione, ad esempio, di prodotto. Categoria e la categoria. Prodotto.

## <a name="self-tracking-entity"></a>Entità con rilevamento automatico
Un'entità compilata da un modello di generazione di codice che consente lo sviluppo a N livelli.

## <a name="table-per-concrete-type-tpc"></a>Tipo di tabella per tipo concreto (TP)
Un metodo di mapping dell'ereditarietà in cui viene eseguito il mapping ciascun tipo non astratto nella gerarchia per separare tabella nel database.

## <a name="table-per-hierarchy-tph"></a>Tabella per gerarchia (TPH)
Un metodo di mapping dell'ereditarietà in cui tutti i tipi nella gerarchia vengono mappati alla stessa tabella nel database. Discriminatore colonna/e viene usato per identificare il tipo di ogni riga è associato.

## <a name="table-per-type-tpt"></a>Tabella per tipo TPT)
Un metodo di mapping dell'ereditarietà in cui le proprietà comuni di tutti i tipi nella gerarchia vengono mappate alla stessa tabella nel database, ma le proprietà univoche per ogni tipo vengono eseguito il mapping a una tabella separata.

## <a name="type-discovery"></a>Individuazione del tipo
Il processo di identificazione di tipi che devono far parte di un modello di Entity Framework.
