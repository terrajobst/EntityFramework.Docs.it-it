---
title: Tabella di funzioni con valori () - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489323"
---
# <a name="table-valued-functions-tvfs"></a>Funzioni con valori di tabella (TVF)
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di funzioni con valori di tabella (TVF) usando la finestra di progettazione di Entity Framework. Viene inoltre illustrato come chiamare una funzione TVF da una query LINQ.

Le TVF sono attualmente supportati solo nel flusso di lavoro Database First.

Supporto TVF è stato introdotto in Entity Framework versione 5. Si noti che per utilizzare le nuove funzionalità, ad esempio funzioni con valori di tabella, enumerazioni e tipi spaziali, deve avere come destinazione .NET Framework 4.5. Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.

Le TVF sono molto simili alle stored procedure con una differenza fondamentale: il risultato di una funzione TVF è componibile. Ciò significa che i risultati di una funzione TVF possono essere usati in una query LINQ mentre i risultati di una stored procedure non è possibile.

## <a name="watch-the-video"></a>Guarda il video

**Presentato da**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario:

- Installare il [database School](~/ef6/resources/school-database.md).

- Dispone di una versione recente di Visual Studio

## <a name="set-up-the-project"></a>Configurare il progetto

1.  Aprire Visual Studio
2.  Nel **File** dal menu **New**, quindi fare clic su **progetto**
3.  Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello
4.  Immettere **TVF** come nome del progetto e fare clic su **OK**

## <a name="add-a-tvf-to-the-database"></a>Aggiungere una funzione TVF al Database

-   Selezionare **vista -&gt; Esplora oggetti di SQL Server**
-   Se Local DB non è presente nell'elenco dei server: fare clic su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al server di database locale
-   Espandere il nodo del database locale
-   Sotto il nodo database, fare clic sul nodo del database School e selezionare **nuova Query...**
-   Nell'Editor T-SQL, incollare la seguente definizione di funzione TVF

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Scegliere il pulsante destro del mouse sull'editor T-SQL e selezionare **Execute**
-   La funzione GetStudentGradesForCourse viene aggiunto al database School

 

## <a name="create-a-model"></a>Creare un modello

1.  Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**
2.  Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel **modelli** riquadro
3.  Immettere **TVFModel.edmx** per il nome del file e quindi fare clic su **Add**
4.  Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**
5.  Fare clic su **nuova connessione** invio **(localdb)\\mssqllocaldb** nel testo del nome Server casella immettere **School** per il database fare clic su nome **OK**
6.  In Scegli di oggetti di Database nella finestra di dialogo di **tabelle** nodo, seleziona la **persona**, **StudentGrade**, e **corso** tabelle
7.  Selezionare il **GetStudentGradesForCourse** funzione che si trova sotto il **Stored procedure e funzioni** nodo nota, che inizia con Visual Studio 2012, Entity Designer consente di importazione di batch la Stored procedure e funzioni
8.  Fare clic su **fine**
9.  Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato. Tutti gli oggetti selezionati nella **Scegli oggetti di Database** nella finestra di dialogo vengono aggiunti al modello.
10. Per impostazione predefinita, la forma dei risultati di ogni importate stored procedure o funzione diventerà automaticamente un nuovo tipo complesso nel modello di entità. Ma per mappare i risultati della funzione GetStudentGradesForCourse all'entità StudentGrade: fare doppio clic su area di progettazione e seleziona **Visualizzatore modelli** nella finestra Browser modello selezionare **importazioni di funzioni**e quindi fare doppio clic il **GetStudentGradesForCourse** funzione In the modifica importazione di funzioni finestra di dialogo **entità** e scegliere **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Salvare in modo permanente e recuperare i dati

Aprire il file in cui è definito il metodo Main. Aggiungere il codice seguente nella funzione Main.

Il codice seguente viene illustrato come compilare una query che utilizza una funzione con valori di tabella. La query proietta i risultati in un tipo anonimo che contiene il titolo del corso correlato e correlate agli studenti con un livello maggiore o uguale a 3.5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata è stato descritto come eseguire il mapping di funzioni con valori di tabella (TVF) usando la finestra di progettazione di Entity Framework. È stato anche illustrato come chiamare una funzione TVF da una query LINQ.
