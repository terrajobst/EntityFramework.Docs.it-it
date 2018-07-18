---
title: L'aggiornamento a Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
caps.latest.revision: 3
ms.openlocfilehash: d22f0d686e6b8e3922d37245386af7723bf08ba1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122781"
---
# <a name="upgrading-to-entity-framework-6"></a>L'aggiornamento a Entity Framework 6

Nelle versioni precedenti di Entity Framework il codice è stato suddiviso tra le librerie di base (principalmente System) fornite come parte di .NET Framework e librerie (OOB) di out-of-band (principalmente EntityFramework) fornite in un pacchetto NuGet. EF6 richiede il codice dalle librerie di base e lo incorpora nelle librerie di OOB. Questo è stato necessario per consentire a Entity Framework essere reso open source e per poter essere in grado di evolvere a un ritmo diverso da .NET Framework. La conseguenza di ciò è che le applicazioni dovranno essere ricompilati utilizzando i tipi di spostata.

Questo dovrebbe essere semplice per le applicazioni che usano DbContext come fornito in Entity Framework 4.1 e versioni successive. È necessario per le applicazioni che usano ObjectContext un po' più lavoro ma non ancora difficile da eseguire.

Ecco un elenco di controllo delle operazioni da eseguire per aggiornare un'applicazione esistente a EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Installare il pacchetto NuGet di Entity Framework 6

È necessario eseguire l'aggiornamento per il nuovo runtime di Entity Framework 6.

1. Pulsante destro del mouse sul progetto, quindi selezionare **Gestisci pacchetti NuGet...**  
2. Sotto il **Online** scheda selezionare **EntityFramework** e fare clic su **installare**  
   > [!NOTE]
   > Se una versione precedente dell'installazione del pacchetto EntityFramework NuGet si eseguirà l'aggiornamento a EF6.

In alternativa, è possibile eseguire il comando seguente dalla Console di gestione pacchetti:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Assicurarsi che vengano rimossi i riferimenti ad assembly a System

Installazione del pacchetto NuGet di Entity Framework 6 deve rimuovere automaticamente qualsiasi riferimento a Data. Entity dal progetto per l'utente.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Scambia i modelli di progettazione (EDMX di Entity Framework) per usare la generazione di codice di Entity Framework 6.x

Se si dispone di tutti i modelli creati con la finestra di progettazione di Entity Framework, è necessario aggiornare i modelli di generazione di codice per generare codice compatibile con Entity Framework 6.

> [!NOTE]
> Sono disponibili attualmente soltanto i modelli del generatore di DbContext di Entity Framework 6.x per Visual Studio 2012 e 2013.

1. Eliminare i modelli di generazione del codice esistente. Questi file verranno in genere è denominati  **\<edmx_file_name\>tt** e  **\<edmx_file_name\>. Context.tt** ed essere annidato sotto il file edmx in Esplora soluzioni. È possibile selezionare i modelli in Esplora soluzioni e premere il **CANC** chiave per eliminarli.  
   > [!NOTE]
   > Nei progetti di sito Web i modelli verranno non essere annidati sotto il file edmx, ma elencati insieme a esso in Esplora soluzioni.  

   > [!NOTE]
   > Nei progetti Visual Basic.NET è necessario abilitare 'Mostra tutti i file' per poter visualizzare i file di modello annidato.
2. Aggiungere modello di generazione codice appropriato di EF 6.x. Aprire il modello nella finestra di progettazione di Entity Framework, pulsante destro del mouse su area di progettazione e seleziona **Aggiungi elemento di generazione di codice...**
    - Se si usa l'API DbContext (scelta consigliata) quindi **EF 6.x DbContext Generator** saranno disponibili nella **dati** scheda.  
      > [!NOTE]
      > Se si usa Visual Studio 2012, è necessario installare gli strumenti di Entity Framework 6 con questo modello. Visualizzare [ottenere Entity Framework](~/ef6/fundamentals/install.md) per informazioni dettagliate.  

    - Se si usa l'API ObjectContext, dovrai selezionare i **Online** scheda e cercare **EF 6.x EntityObject Generator**.  
3. Se tutte le personalizzazioni sono applicati per i modelli di generazione di codice che è necessario applicare nuovamente per i modelli aggiornati.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Aggiornare gli spazi dei nomi per qualsiasi tipo EF core in uso

Gli spazi dei nomi per i tipi DbContext e Code First non sono stati modificati. Ciò significa che per molte applicazioni che usano Entity Framework 4.1 o versioni successive non è necessario apportare alcuna modifica.

I tipi, ad esempio ObjectContext disponibili in precedenza System sono stati spostati nuovi spazi dei nomi. Ciò significa che potrebbe essere necessario aggiornare il *usando* oppure *importazione* direttive per la compilazione EF6.

La regola generale per lo spazio dei nomi è che qualsiasi tipo in System.Data.* viene spostata nel System.Data.Entity.Core.*. In altre parole, sufficiente inserire **Entity.Core.** Dopo aver System. Data. Ad esempio:

- System.Data.EntityException = > System. Data. **Entity.Core.** EntityException  
- System.Data.Objects.ObjectContext = > System. Data. **Entity.Core.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System. Data. **Entity.Core.** Objects.DataClasses.RelationshipManager  

Questi tipi sono nel *Core* gli spazi dei nomi perché non vengono usati direttamente per la maggior parte delle applicazioni basate su DbContext. Alcuni tipi che fanno parte della System sono ancora usati comunemente e direttamente per le applicazioni basate su DbContext e pertanto non sono stati spostati nel *Core* gli spazi dei nomi. Questi sono:

- System.Data.EntityState = > System. Data. **Entità.** EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System. Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Questa classe è stata rinominata; una classe con il vecchio nome esiste già e funziona, ma ora contrassegnato come obsoleto.  
- System.Data.Objects.EntityFunctions = > System. Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Questa classe è stata rinominata; una classe con il vecchio nome esiste già e funziona, ma ora contrassegnato come obsoleto.)  
- Classi spaziale (ad esempio, DbGeography, DbGeometry) sono stati spostati dalla System.Data.Spatial = > System. Data. **Entità.** Spaziali

> [!NOTE]
> Alcuni tipi nello spazio dei nomi System. Data sono in System che non è un assembly di Entity Framework. Questi tipi non è sono spostato e pertanto non subiscono gli spazi dei nomi.
