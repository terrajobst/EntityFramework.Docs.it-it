---
title: Finestra di progettazione TPT (ereditarietà)-Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
caps.latest.revision: 3
ms.openlocfilehash: c3ccb44f931b830a96a553d5af1e722a9ca4bbf0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121191"
---
# <a name="designer-tpt-inheritance"></a>Finestra di progettazione TPT (ereditarietà)
Questa procedura dettagliata viene illustrato come implementare l'ereditarietà di (TPT) per ogni tipo di tabella nel modello usando Entity Framework Designer (Entity Framework Designer). L'ereditarietà tabella per tipo prevede l'utilizzo di una tabella separata nel database per la gestione dei dati relativi alle proprietà non ereditate e alle proprietà chiave di ogni tipo della gerarchia di ereditarietà.

In questa procedura dettagliata verrà eseguito il mapping di **Course** (tipo di base), **OnlineCourse** (deriva dal corso), e **OnsiteCourse** (deriva da **corso**) entità alle tabelle con gli stessi nomi. Verrà creato un modello dal database e quindi modificare il modello per implementare l'ereditarietà tabella per tipo.

È possibile inoltre iniziare con il primo modello e quindi generare il database dal modello. Per impostazione predefinita, la finestra di progettazione di Entity Framework utilizza la strategia TPT e pertanto qualsiasi ereditarietà nel modello verranno mappati a tabelle distinte.

## <a name="other-inheritance-options"></a>Altre opzioni di ereditarietà

Tabella per gerarchia (TPH) è un altro tipo di ereditarietà nelle quali un database tabella viene utilizzata per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.  Per informazioni su come eseguire il mapping di ereditarietà tabella per gerarchia con Entity Designer, vedere [Entity Framework Designer l'ereditarietà](~/ef6/modeling/designer/inheritance/tph.md). 

Si noti che, la tabella per tipo concreto tipo (TP) ed misto ereditarietà modelli sono supportati dal runtime di Entity Framework, ma non sono supportati dalla finestra di progettazione di Entity Framework. Se si desidera utilizzare TPC o misto eredità, sono disponibili due opzioni: utilizzare Code First, oppure modificare manualmente i file con estensione EDMX. Se si sceglie di usare il file EDMX, nella finestra Dettagli Mapping verrà inserita nella "modalità provvisoria" e non sarà in grado di utilizzare la finestra di progettazione per modificare i mapping.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

-   Aprire Visual Studio 2012.
-   Selezionare **File -&gt; Novità -&gt; progetto**
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.
-   Immettere **TPTDBFirstSample** come nome.
-   Scegliere **OK**.

## <a name="create-a-model"></a>Creare un modello

-   Il progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **TPTModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model casella, selezionare * * Genera da database * * e quindi fare clic su **successivo**.
-   Fare clic su **nuova connessione**.
    Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella finestra di dialogo Scegli oggetti di Database, sotto il nodo tabelle, selezionare la **reparto**, **corso OnlineCourse e OnsiteCourse** tabelle.
-   Scegliere **Fine**.

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato. Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di Database vengono aggiunti al modello.

## <a name="implement-table-per-type-inheritance"></a>Implementare l'ereditarietà tabella per tipo

-   Nell'area di progettazione, fare doppio clic il **OnlineCourse** tipo di entità e selezionare **proprietà**.
-   Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.
-   Fare doppio clic il **OnsiteCourse** tipo di entità e selezionare **proprietà**.
-   Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.
-   Fare doppio clic su (riga) dell'associazione tra il **OnlineCourse** e **corso** i tipi di entità.
    Selezionare **Elimina dal modello**.
-   Fare doppio clic sull'associazione tra il **OnsiteCourse** e **corso** i tipi di entità.
    Selezionare **Elimina dal modello**.

Ora si eliminerà la **CourseID** proprietà dal **OnlineCourse** e **OnsiteCourse** perché queste classi ereditano **CourseID** da il **corso** tipo di base.

-   Fare doppio clic sui **CourseID** proprietà delle **OnlineCourse** tipo di entità e quindi selezionare **Elimina da modello**.
-   Fare doppio clic sui **CourseID** proprietà delle **OnsiteCourse** tipo di entità e quindi selezionare **Elimina da modello**
-   A questo punto l'ereditarietà tabella per tipo risulta implementata.

![TABELLA PER TIPO](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Usare il modello

Aprire il **Program.cs** file dove il **Main** metodo è definito. Incollare il codice seguente nel **Main** (funzione). Il codice esegue tre query. La prima query restituisce tutti i **corsi** correlate al reparto specificato. La seconda query Usa la **OfType** metodo restituisca **OnlineCourses** correlate al reparto specificato. Restituisce la terza query **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
