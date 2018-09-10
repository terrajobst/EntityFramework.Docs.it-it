---
title: Data Binding con WPF - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: e6df90db17d39d3aa91275800a6414fed40fb5db
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251154"
---
# <a name="databinding-with-wpf"></a>Data Binding con WPF
Questa procedura dettagliata viene illustrato come associare controlli WPF in un form di "master-detail" tipi POCO. L'applicazione usa le API di Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.

Il modello definisce due tipi che fanno parte di relazione uno-a-molti: **categoria** (principal\\master) e **prodotto** (dipendenti\\dettaglio). Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli WPF. Il framework di associazione dati WPF consente la navigazione tra gli oggetti correlati: se si seleziona le righe della visualizzazione master, la visualizzazione di dettaglio da aggiornare con i dati figlio corrispondente.

Le schermate e i listati di codice in questa procedura dettagliata vengono forniti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Usare l'opzione 'Object' per la creazione di origini dati WPF

Con la versione precedente di Entity Framework è stato usato per consiglia di usare la **Database** durante la creazione di una nuova origine dati basata su un modello creato con la finestra di progettazione di Entity Framework. Questo era perché la finestra di progettazione genera classi di entità derivato da EntityObject e un contesto che deriva da ObjectContext. Usando l'opzione di Database sarebbe consentono di scrivere il codice più efficace per l'interazione con questa superficie dell'API.

Le finestre di progettazione di Entity Framework per Visual Studio 2012 e Visual Studio 2013 generano un contesto che deriva da DbContext insieme alle classi di entità POCO semplice. Con Visual Studio 2010 è consigliabile dello swapping in un modello di generazione di codice che usa DbContext, come descritto più avanti in questa procedura dettagliata.

Quando si usa la superficie dell'API DbContext è consigliabile usare la **oggetto** opzione quando si crea una nuova origine dati, come illustrato in questa procedura dettagliata.

Se necessario, è possibile [ripristinare la generazione di codice basato su ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) per i modelli creati con la finestra di progettazione di Entity Framework.

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 per poter completare questa procedura dettagliata.

Se si usa Visual Studio 2010, è necessario anche installare NuGet. Per altre informazioni, vedere [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Creare l'applicazione

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel riquadro sinistro e **WPFApplication** nel riquadro di destra
-   Immettere **WPFwithEFSample** come nome
-   Scegliere **OK**.

## <a name="install-the-entity-framework-nuget-package"></a>Installare il pacchetto NuGet di Entity Framework

-   In Esplora soluzioni fare clic sui **WinFormswithEFSample** progetto
-   Selezionare **Gestisci pacchetti NuGet...**
-   Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto
-   Fare clic su **installare**  
    >[!NOTE]
    > Oltre all'assembly EntityFramework viene anche aggiunto un riferimento a System.ComponentModel.DataAnnotations. Se il progetto contiene un riferimento a Data. Entity, quindi verrà rimosso quando viene installato il pacchetto di Entity Framework. L'assembly Data. Entity non è più utilizzato per le applicazioni Entity Framework 6.

## <a name="define-a-model"></a>Definire un modello

In questa procedura dettagliata che è possibile scelta di implementare un modello utilizzando Code First o Entity Framework Designer. Completare una delle due sezioni seguenti.

### <a name="option-1-define-a-model-using-code-first"></a>Opzione 1: Definire un modello utilizzando Code First

In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First. Passare alla sezione successiva (**Option 2: definire un modello utilizzando Database First)** se si preferisce usare Database First da invertire progettare il modello da un database utilizzando la finestra di progettazione di Entity Framework

Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).

-   Aggiungere una nuova classe per il **WPFwithEFSample:**
    -   Fare doppio clic sul nome del progetto
    -   Selezionare **aggiungere**, quindi **nuovo elemento**
    -   Selezionare **classe** e immettere **prodotto** per il nome della classe
-   Sostituire il **prodotto** definizione con il codice seguente della classe:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione. In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.

Oltre a definire le entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà. Alla classe DbSet&lt;TEntity&gt; proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.

Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.

-   Aggiungere un nuovo **ProductContext** classe al progetto con la definizione seguente:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Compilare il progetto.

### <a name="option-2-define-a-model-using-database-first"></a>Opzione 2: Definire un modello utilizzando Database First

Questa sezione illustra come usare Database First per decompilare il modello da un database utilizzando la finestra di progettazione di Entity Framework. Se è stata completata la sezione precedente (**Option 1: definire un modello utilizzando Code First)**, quindi ignorare questa sezione e passare direttamente alla sezione la **il caricamento Lazy** sezione.

#### <a name="create-an-existing-database"></a>Creare un Database esistente

In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.

Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si usa Visual Studio 2010 si creerà un database SQL Express.
-   Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.

È possibile procedere e generare il database.

-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**
-   Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati

    ![Modifica origine dati](~/ef6/media/changedatasource.png)

-   Connettersi a LocalDB o SQL Express, in base alla quale è stato installato e immettere **prodotti** come nome del database

    ![Aggiungi connessione LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Aggiungi connessione Express](~/ef6/media/addconnectionexpress.png)

-   Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**

    ![Crea database](~/ef6/media/createdatabase.png)

-   Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**
-   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Modelli di reverse engineering

Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.

-   **Progetto -&gt; Aggiungi nuovo elemento...**
-   Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **ProductModel** come nome, quindi fare clic su **OK**
-   Verrà avviata la **procedura guidata Entity Data Model**
-   Selezionare **genera da Database** e fare clic su **successivo**

    ![Scelta dei contenuti del modello](~/ef6/media/choosemodelcontents.png)

-   Selezionare la connessione al database creato nella prima sezione, immettere **ProductContext** come nome della stringa di connessione, scegliere **successivo**

    ![Scegliere la connessione](~/ef6/media/chooseyourconnection.png)

-   Selezionare la casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'

    ![Scegli oggetti di](~/ef6/media/chooseyourobjects.png)

Una volta completato il processo di reverse engineering il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer. Un file app. config è stato aggiunto anche al progetto con i dettagli della connessione per il database.

#### <a name="additional-steps-in-visual-studio-2010"></a>Passaggi aggiuntivi in Visual Studio 2010

Se si lavora in Visual Studio 2010 è necessario aggiornare la finestra di progettazione di Entity Framework per utilizzare la generazione del codice di Entity Framework 6.

-   Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**
-   Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**
-   Selezionare il **EF 6.x generatore di DbContext per C\#,** immettere **ProductsModel** come il nome e fare clic su Aggiungi

#### <a name="updating-code-generation-for-data-binding"></a>L'aggiornamento di generazione del codice per il data binding

Entity Framework genera codice dal modello usando i modelli T4. I modelli forniti con Visual Studio o scaricato da Visual Studio gallery servono per utilizzo generico. Ciò significa che le entità generate da questi modelli semplici ICollection&lt;T&gt; proprietà. Tuttavia, quando si esegue data binding WPF con è opportuno utilizzare **ObservableCollection** per le proprietà della raccolta in modo che WPF può tenere traccia delle modifiche apportate alle raccolte. A tal fine verrà modificare i modelli per l'uso ObservableCollection.

-   Aprire il **Esplora soluzioni** e trovare **ProductModel.edmx** file
-   Trovare il **ProductModel.tt** file che verrà annidato in file ProductModel.edmx

    ![Modello di prodotto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio
-   Trovare e sostituire le due occorrenze di "**ICollection**"con"**ObservableCollection**". Questi si trovano approssimativamente alle righe 296 e 484.
-   Trova e Sostituisci la prima occorrenza di "**HashSet**"con"**ObservableCollection**". Questo evento si trova approssimativamente alla riga 50. **No** sostituire la seconda occorrenza della HashSet trovato in un secondo momento nel codice.
-   Trovare e sostituire l'unica occorrenza di "**System.Collections.Generic**"con"**ObjectModel**". Questo si trova approssimativamente alla riga 424.
-   Salvare il file ProductModel.tt. Ciò deve generare il codice per le entità da rigenerare. Se il codice non verranno rigenerati automaticamente, quindi fare clic con il pulsante destro su ProductModel.tt e scegliere "Esegui strumento personalizzato".

Se si apre ora il file Category.cs (che è annidato sotto ProductModel.tt), si dovrebbe vedere che la raccolta di prodotti ha il tipo **ObservableCollection&lt;prodotto&gt;**.

Compilare il progetto.

## <a name="lazy-loading"></a>Caricamento lazy

Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione. In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.

Entity Framework offre un'opzione per caricare entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione. Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione una query separata verrà eseguita sul database se il contenuto non è già nel contesto.

Quando si usano i tipi di entità POCO, Entity Framework consente di ottenere il caricamento lazy creando istanze di tipi proxy derivato in fase di esecuzione e quindi si esegue l'override proprietà virtuali nelle classi per aggiungere l'hook del caricamento. Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare navigazione getter di proprietà come **pubbliche** e **virtual** (**Overridable** in Visual Basic), e di classe non deve essere **sealed** (**NotOverridable** in Visual Basic). Quando il Database di utilizzando le proprietà di navigazione prima vengono eseguite automaticamente virtuale per abilitare il caricamento lazy. Nella sezione Code First è stato scelto di rendere le proprietà di navigazione virtuale per lo stesso motivo

## <a name="bind-object-to-controls"></a>Associare oggetti ai controlli

Aggiungere le classi definite nel modello come origini dati per questa applicazione WPF.

-   Fare doppio clic su **MainWindow. XAML** in Esplora soluzioni aprire il form principale
-   Dal menu principale, selezionare **progetto -&gt; Aggiungi nuova origine dati...**
    (in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...** )
-   Nella pagina scegliere Typewindow un'origine dati, selezionare **oggetti** e fare clic su **successivo**
-   Selezionare la finestra di dialogo di oggetti dati, Espandi il **WPFwithEFSample** due volte e selezionare **categoria**  
    *Non è necessario selezionare il **prodotto** dell'origine dati, perché verrà visualizzato a esso tramite il **prodotto**del proprietà il **categoria** zdroj dat*  

    ![Selezionare gli oggetti dati](~/ef6/media/selectdataobjects.png)

-   Fare clic su **fine.**
-   Viene aperta la finestra Origini dati accanto alla finestra di MainWindow. XAML *se la finestra Origini dati non verrà visualizzati, selezionare **visualizzazione -&gt; Other Windows -&gt; Zdroje dat***
-   Premere l'icona della puntina, in modo che la finestra Origini dati non automaticamente nascondere. Potrebbe essere necessario premere il pulsante Aggiorna se è già visualizzata la finestra.

    ![Data Sources](~/ef6/media/datasources.png)

-   Selezionare la * * categoria * * dei dati di origine e trascinarla nel form.

Di seguito è accaduto quando abbiamo trascinato per questa origine:

-   Il **categoryViewSource** risorse e il * * controllo categoryDataGrid * * sono stati aggiunti a XAML. Per altre informazioni sulle DataViewSources, vedere http://bea.stollnitz.com/blog/?p=387.
-   La proprietà DataContext sull'elemento della griglia padre è stata impostata su "{StaticResource **categoryViewSource** }".  Il **categoryViewSource** risorsa viene usata come origine del binding per l'outer\\elemento griglia padre. Gli elementi della griglia interni ereditano quindi il valore di DataContext dall'elemento padre della griglia (del categoryDataGrid ItemsSource è impostata su "{Binding}"). 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Aggiungere una griglia dei dettagli

Ora che abbiamo una griglia per visualizzare le categorie è possibile aggiungere una griglia di dettagli per visualizzare i prodotti associati.

-   Selezionare la * * prodotti * * proprietà sotto la * * categoria * * dei dati di origine e trascinarla nel form.
    -   Il **categoryProductsViewSource** risorse e **productDataGrid** griglia vengono aggiunti a XAML
    -   Il percorso di associazione per questa risorsa è impostato su prodotti
    -   Framework di associazione dati WPF assicura che solo i prodotti correlati alla categoria selezionata viene visualizzata **productDataGrid**
-   Dalla casella degli strumenti, trascinare **pulsante** al form. Impostare il **nome** proprietà **buttonSave** e il **contenuto** proprietà **Salva**.

Il modulo dovrebbe essere simile al seguente:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Aggiungere il codice che gestisce l'interazione dei dati

È possibile aggiungere alcuni gestori di eventi alla finestra principale.

-   Nella finestra XAML, fare clic sui  **&lt;finestra** elemento, si seleziona la finestra principale
-   Nel **delle proprietà** finestra scegliere **eventi** in alto a destra, quindi fare doppio clic sulla casella di testo a destra del **Loaded** etichetta

    ![Proprietà della finestra principale](~/ef6/media/mainwindowproperties.png)

-   Aggiungere anche il **fare clic su** evento per il **salvare** pulsante facendo doppio clic sul pulsante Salva nella finestra di progettazione. 

Verrà visualizzata per il code-behind del form, ora si modificherà il codice per usare il ProductContext per eseguire l'accesso ai dati. Aggiornare il codice per MainWindow come illustrato di seguito.

Il codice dichiara un'istanza con esecuzione prolungata **ProductContext**. Il **ProductContext** oggetto viene usato per eseguire query e salvare i dati nel database. Il **Dispose**() sulle **ProductContext** istanza viene quindi chiamata da sottoposto a override **OnClosing** (metodo). I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Testare l'applicazione WPF

-   Compilare l'applicazione ed eseguirla. Se è stato utilizzato Code First, quindi è possibile notare che un **WPFwithEFSample.ProductContext** database viene creato automaticamente.
-   Immettere un nome di categoria nei nomi di prodotto e griglia superiore nella griglia inferiore *non immette alcuna stringa nelle colonne ID, poiché la chiave primaria è generata dal database*

    ![Finestra principale di con prodotti e le nuove categorie](~/ef6/media/screen1.png)

-   Premere il **salvare** consente di salvare i dati nel database

Dopo la chiamata a di DbContext **SaveChanges**(), gli ID vengono popolati con i valori del database generato. Perché abbiamo chiamato **Refresh**() dopo **SaveChanges**() la **DataGrid** controlli vengono aggiornati con i nuovi valori.

![Finestra principale con gli ID popolati](~/ef6/media/screen2.png)
