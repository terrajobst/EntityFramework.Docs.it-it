---
title: Query della finestra di progettazione di Stored procedure - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 29b7745c2229ce4a38ad81e11406474424adfa24
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994972"
---
# <a name="designer-query-stored-procedures"></a>Query della finestra di progettazione di Stored procedure
Questa procedura dettagliata mostra come usare Entity Framework Designer (Entity Framework Designer) per importare le stored procedure in un modello e quindi chiamare le stored procedure importate per recuperare i risultati. 

Si noti che Code First non supporta il mapping a stored procedure o funzioni. Tuttavia, è possibile chiamare stored procedure o funzioni tramite il metodo System.Data.Entity.DbSet.SqlQuery. Ad esempio:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

-   Aprire Visual Studio 2012.
-   Selezionare **File -&gt; Novità -&gt; progetto**
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.
-   Immettere **EFwithSProcsSample** come nome.
-   Scegliere **OK**.

## <a name="create-a-model"></a>Creare un modello

-   Fare clic sul progetto in Esplora soluzioni e selezionare **Add -&gt; nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **EFwithSProcsModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.
-   Fare clic su **nuova connessione**.  
    Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.  
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella finestra di dialogo Scegli oggetti di Database, verificare i **tabelle** casella di controllo per selezionare tutte le tabelle.  
    Selezionare anche le seguenti stored procedure con il **funzioni e Stored procedure** nodo: **GetStudentGrades** e **GetDepartmentName**. 

    ![Import](~/ef6/media/import.jpg)

    *Partire da Visual Studio 2012 nella finestra di progettazione di Entity Framework supporta l'importazione bulk di stored procedure. Il **Importa selezionate stored procedure e funzioni nel modello theentity** è selezionata per impostazione predefinita.*
-   Scegliere **Fine**.

Per impostazione predefinita, la forma dei risultati di ogni importate stored procedure o funzione che restituisce più di una colonna verrà portato automaticamente un nuovo tipo complesso. In questo esempio si vuole mappare i risultati del **GetStudentGrades** funzionare per il **StudentGrade** entità e i risultati del **GetDepartmentName** a **Nessuna** (**none** è il valore predefinito).

Per un'importazione di funzioni restituire un tipo di entità, le colonne restituite dalla stored procedure corrispondente devono corrispondere esattamente le proprietà scalari del tipo di entità restituite. Un'importazione di funzioni può inoltre restituire raccolte di tipi semplici, tipi complessi o nessun valore.

-   Fare doppio clic su area di progettazione e seleziona **Browser modello**.
-   In **Visualizzatore modelli**, selezionare **importazioni di funzioni**e quindi fare doppio clic il **GetStudentGrades** (funzione).
-   Nella finestra di dialogo Modifica importazione di funzioni, selezionare **Entities** e scegliere **StudentGrade**.  
    *Il **importazione della funzione è componibile** casella di controllo nella parte superiore delle **importazioni di funzioni** finestra di dialogo consente di eseguire il mapping di funzioni componibile. Se si seleziona questa casella, verranno visualizzato solo funzioni componibili (funzioni con valori di tabella) nei **Stored Procedure / nome funzione** elenco a discesa. Se non si seleziona questa casella, solo le funzioni non componibili verranno visualizzate nell'elenco.*

## <a name="use-the-model"></a>Usare il modello

Aprire il **Program.cs** file dove il **Main** metodo è definito. Aggiungere il codice seguente nella funzione Main.

Il codice chiama due stored procedure: **GetStudentGrades** (restituisce **StudentGrades** per l'oggetto specificato *StudentId*) e **GetDepartmentName** (restituisce il nome del dipartimento nel parametro di output).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parametri di output
-----------------

Se si utilizzano parametri di output, i relativi valori non sarà disponibili fino a quando i risultati sono stati letti completamente. Ciò è dovuto il comportamento sottostante di DbDataReader, vedere [recupero di dati mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) per altri dettagli.
