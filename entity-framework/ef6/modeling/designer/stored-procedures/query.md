---
title: Stored procedure per query della finestra di progettazione-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182475"
---
# <a name="designer-query-stored-procedures"></a>Stored procedure di query della finestra di progettazione
In questa procedura dettagliata viene illustrato come utilizzare il Entity Framework Designer (Entity Designer) per importare stored procedure in un modello e quindi chiamare le stored procedure importate per recuperare i risultati. 

Si noti che Code First non supporta il mapping a stored procedure o funzioni. Tuttavia, è possibile chiamare stored procedure o funzioni utilizzando il metodo System. Data. Entity. DbSet. sqlQuery. Esempio:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- [Database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

-   Aprire Visual Studio 2012.
-   Selezionare il **progetto file-&gt; New-&gt;**
-   Nel riquadro sinistro fare clic su **Visual C @ no__t-1**, quindi selezionare il modello **console** .
-   Immettere **EFwithSProcsSample**@no__t-aggiungere1come il nome.
-   Fare clic su **OK**.

## <a name="create-a-model"></a>Creazione di un modello

-   Fare clic con il pulsante destro del mouse sul progetto Esplora soluzioni e scegliere **Aggiungi-&gt; nuovo elemento**.
-   Selezionare **dati** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro modelli.
-   Immettere **EFwithSProcsModel. edmx** per il nome del file e quindi fare clic su **Aggiungi**.
-   Nella finestra di dialogo Scegli contenuto Model selezionare **genera da database**, quindi fare clic su **Avanti**.
-   Fare clic su **nuova connessione**.  
    Nella finestra di dialogo Proprietà connessione immettere il nome del server (ad esempio, **(local DB) \\mssqllocaldb**), selezionare il metodo di autenticazione, digitare **School** for il nome del database, quindi fare clic su **OK**.  
    La finestra di dialogo scegliere la connessione dati viene aggiornata con l'impostazione di connessione al database.
-   Nella finestra di dialogo Scegli oggetti di database selezionare le **tabelle** checkbox per selezionare tutte le tabelle.  
    Inoltre, nel nodo **stored procedure e funzioni** selezionare le stored procedure seguenti: **GetStudentGrades** e **GetDepartmentName**. 

    ![Import](~/ef6/media/import.jpg)

    *Starting con Visual Studio 2012 la finestra di progettazione EF supporta l'importazione bulk di stored procedure. Per impostazione predefinita, l' **importazione delle stored procedure e delle funzioni selezionate nel modello set** è selezionata.*
-   Fare clic su **fine**.

Per impostazione predefinita, la forma risultante di ogni stored procedure o funzione importata che restituisce più colonne diventerà automaticamente un nuovo tipo complesso. In questo esempio si vuole eseguire il mapping dei risultati della funzione **GetStudentGrades** all'entità **StudentGrade** e i risultati di **GetDepartmentName** su **None** (**nessuno** è il valore predefinito).

Affinché un'importazione di funzioni restituisca un tipo di entità, le colonne restituite dalla stored procedure corrispondente devono corrispondere esattamente alle proprietà scalari del tipo di entità restituito. Un'importazione di funzioni può inoltre restituire raccolte di tipi semplici, tipi complessi o nessun valore.

-   Fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **browser modello**.
-   In **browser modello**selezionare **importazioni di funzioni**, quindi fare doppio clic sulla funzione **GetStudentGrades** .
-   Nella finestra di dialogo modifica importazione funzione selezionare **entità**  E scegliere **StudentGrade**.  
    l'importazione di funzioni *The **è componibile** nella parte superiore della finestra di dialogo **importazioni** di funzioni che consente di eseguire il mapping a funzioni componibili. Se si seleziona questa casella, solo le funzioni componibili (funzioni con valori di tabella) verranno visualizzate nell'elenco a discesa **stored procedure/nome funzione** . Se non si seleziona questa casella, nell'elenco verranno visualizzate solo le funzioni non componibili.*

## <a name="use-the-model"></a>Usare il modello

Aprire il file **Program.cs** in cui è definito il metodo **Main** . Aggiungere il codice seguente nella funzione Main.

Il codice chiama due stored procedure: **GetStudentGrades** (restituisce **StudentGrades** per il *StudentID*specificato) e **GetDepartmentName** (restituisce il nome del reparto nel parametro di output).  

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

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parametri di output
-----------------

Se vengono utilizzati parametri di output, i relativi valori non saranno disponibili fino a quando i risultati non sono stati letti completamente. A causa del comportamento sottostante di DbDataReader, vedere Recupero di [dati tramite un DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) per altri dettagli.
