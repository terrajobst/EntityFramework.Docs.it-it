---
title: Spaziali - Code - First per Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 744ea58c7a8796ed20adc304b3928551eb6928a4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490597"
---
# <a name="spatial---code-first"></a>Spaziali - Code First
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di tipi di dati spaziali con Code First di Entity Framework. Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.

Questa procedura dettagliata utilizzerà Code First per creare un nuovo database, ma è anche possibile usare [Code First per un database esistente](~/ef6/modeling/code-first/workflows/existing-database.md).

Supporto per il tipo spaziale è stata introdotta in Entity Framework 5. Si noti che per usare le nuove funzionalità, ad esempio tipo spaziale, enumerazioni e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5. Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.

Per utilizzare i tipi di dati spaziali è anche necessario usare un provider di Entity Framework con supporto spaziale. Visualizzare [supporto del provider per i tipi spaziali](~/ef6/fundamentals/providers/spatial-support.md) per altre informazioni.

Esistono due tipi di dati spaziali principale: geografia e geometria. Il tipo di dati geography archivia dati ellissoidali (ad esempio, cui è coordinate GPS latitudine e longitudine). Il tipo di dati geometry rappresenta un sistema di coordinate Euclideo (piano).

## <a name="watch-the-video"></a>Guarda il video
In questo video viene illustrato come eseguire il mapping di tipi di dati spaziali con Code First di Entity Framework. Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.

**Presentato da**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.

## <a name="set-up-the-project"></a>Configurare il progetto

1.  Aprire Visual Studio 2012
2.  Nel **File** dal menu **New**, quindi fare clic su **progetto**
3.  Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello
4.  Immettere **SpatialCodeFirst** come nome del progetto e fare clic su **OK**

## <a name="define-a-new-model-using-code-first"></a>Definire un nuovo modello tramite Code First

Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio). Il codice seguente definisce la classe University.

L'università ha la proprietà Location del tipo di DbGeography. Per usare il tipo di DbGeography, è necessario aggiungere un riferimento all'assembly Data. Entity e anche aggiungere il System.Data.Spatial istruzione using.

Aprire il file Program.cs e incollare quanto segue usando istruzioni all'inizio del file:

``` csharp
using System.Data.Spatial;
```

Aggiungere la definizione di classe University seguente al file Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
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

Si noti che oltre all'assembly EntityFramework, un riferimento all'assembly System.ComponentModel.DataAnnotations viene anche aggiunto.

Nella parte superiore del file Program.cs, aggiungere la seguente istruzione using:

``` csharp
using System.Data.Entity;
```

Nel file Program.cs aggiungere la definizione di contesto. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Salvare in modo permanente e recuperare i dati

Aprire il file Program.cs in cui è definito il metodo Main. Aggiungere il codice seguente nella funzione Main.

Il codice aggiunge due nuovi oggetti University al contesto. Proprietà spaziali vengono inizializzate tramite il metodo DbGeography.FromText. Il punto di geografia è rappresentato come WellKnownText viene passato al metodo. Il codice quindi Salva i dati. Quindi, LINQ query che restituisce un oggetto University dove il percorso è più vicino alla posizione specificata, viene costruita ed eseguita.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Visualizzazione Database generato

Quando si esegue la prima volta che l'applicazione, Entity Framework crea un database. Perché abbiamo installato Visual Studio 2012, il database verrà creato nell'istanza di Local DB. Per impostazione predefinita, Entity Framework nome del database dopo il nome completo del contesto derivato (in questo esempio corrisponde **SpatialCodeFirst.UniversityContext**). Le volte successive che verrà utilizzato il database esistente.  

Si noti che se si apportano modifiche al modello dopo aver creato il database, è necessario utilizzare migrazioni Code First per aggiornare lo schema del database. Visualizzare [Code First per un nuovo Database](~/ef6/modeling/code-first/workflows/new-database.md) per un esempio di usare le migrazioni.

Per visualizzare i database e i dati, eseguire le operazioni seguenti:

1.  Nel menu principale di Visual Studio 2012, selezionare **View**  - &gt; **Esplora oggetti di SQL Server**.
2.  Se Local DB non è presente nell'elenco dei server, fare clic il pulsante destro del mouse su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al l'istanza di Local DB
3.  Espandere il nodo del database locale
4.  Espandi la **database** per visualizzare il nuovo database e passare alla cartella il **università** tabella
5.  Per visualizzare i dati, fare doppio clic sulla tabella e selezionare **Visualizza dati**

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata è stato illustrato come usare i tipi spaziali con Code First di Entity Framework. 
