---
title: Supporto di enum - Code First - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489505"
---
# <a name="enum-support---code-first"></a>Supporto di enum - Code First
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Questa procedura dettagliata video e procedura dettagliata viene illustrato come utilizzare tipi enum con Code First di Entity Framework. Viene inoltre illustrato come usare enumerazioni in una query LINQ.

Questa procedura dettagliata utilizzerà Code First per creare un nuovo database, ma è anche possibile usare [Code First per eseguire il mapping a un database esistente](~/ef6/modeling/code-first/workflows/existing-database.md).

Supporto di enumerazione è stata introdotta in Entity Framework 5. Per usare le nuove funzionalità come le enumerazioni, tipi di dati spaziali e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5. Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.

In Entity Framework, un'enumerazione può avere i seguenti tipi sottostanti: **Byte**, **Int16**, **Int32**, **Int64** , o **SByte**.

## <a name="watch-the-video"></a>Guarda il video
In questo video viene illustrato come utilizzare tipi enum con Code First di Entity Framework. Viene inoltre illustrato come usare enumerazioni in una query LINQ.

**Presentato da**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.

 

## <a name="set-up-the-project"></a>Configurare il progetto

1.  Aprire Visual Studio 2012
2.  Nel **File** dal menu **New**, quindi fare clic su **progetto**
3.  Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello
4.  Immettere **EnumCodeFirst** come nome del progetto e fare clic su **OK**

## <a name="define-a-new-model-using-code-first"></a>Definire un nuovo modello tramite Code First

Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio). Il codice seguente definisce la classe di reparto.

Il codice definisce anche l'enumerazione DepartmentNames. Per impostazione predefinita, l'enumerazione presenta **int** tipo. La proprietà Name nella classe Department è del tipo DepartmentNames.

Aprire il file Program.cs e incollare le seguenti definizioni di classe.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Definire DbContext tipo derivato

Oltre a definire le entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà. Alla classe DbSet&lt;TEntity&gt; proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.

Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.

I tipi DbContext e DbSet definiti nell'assembly EntityFramework. Si aggiungerà un riferimento a questa DLL usando il pacchetto EntityFramework NuGet.

1.  In Esplora soluzioni fare doppio clic sul nome del progetto.
2.  Selezionare **Gestisci pacchetti NuGet...**
3.  Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto.
4.  Fare clic su **installare**

Si noti che oltre all'assembly di Entity Framework, i riferimenti agli assembly System.ComponentModel.DataAnnotations e Data. Entity vengono aggiunti anche.

Nella parte superiore del file Program.cs, aggiungere la seguente istruzione using:

``` csharp
using System.Data.Entity;
```

Nel file Program.cs aggiungere la definizione di contesto. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Salvare in modo permanente e recuperare i dati

Aprire il file Program.cs in cui è definito il metodo Main. Aggiungere il codice seguente nella funzione Main. Il codice aggiunge un nuovo oggetto reparto al contesto. Quindi Salva i dati. Inoltre, il codice esegue una query LINQ che restituisce un reparto dove name è DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Visualizzazione Database generato

Quando si esegue la prima volta che l'applicazione, Entity Framework crea un database. Perché abbiamo installato Visual Studio 2012, il database verrà creato nell'istanza di Local DB. Per impostazione predefinita, Entity Framework nome del database dopo il nome completo del contesto derivato (per questo esempio corrisponde **EnumCodeFirst.EnumTestContext**). Le volte successive che verrà utilizzato il database esistente.  

Si noti che se si apportano modifiche al modello dopo aver creato il database, è necessario utilizzare migrazioni Code First per aggiornare lo schema del database. Visualizzare [Code First per un nuovo Database](~/ef6/modeling/code-first/workflows/new-database.md) per un esempio di usare le migrazioni.

Per visualizzare i database e i dati, eseguire le operazioni seguenti:

1.  Nel menu principale di Visual Studio 2012, selezionare **View**  - &gt; **Esplora oggetti di SQL Server**.
2.  Se Local DB non è presente nell'elenco dei server, fare clic il pulsante destro del mouse su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al Istanza di Local DB
3.  Espandere il nodo del database locale
4.  Espandi la **database** per visualizzare il nuovo database e passare alla cartella il **reparto** si noti che Code First non crea una tabella in cui viene eseguito il mapping al tipo di enumerazione di tabella
5.  Per visualizzare i dati, fare doppio clic sulla tabella e selezionare **Visualizza dati**

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata è stato illustrato come usare i tipi di enumerazione con Code First di Entity Framework. 
