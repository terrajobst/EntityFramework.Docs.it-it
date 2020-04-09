---
title: Associazione dati con WPF - EF6Databinding with WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639148"
---
> [!IMPORTANT]
> **Questo documento è valido solo per WPF in .NET Framework**
>
> In questo documento viene descritta l'associazione dati per WPF in .NET Framework.This document describes databinding for WPF on the .NET Framework. Per i nuovi progetti .NET Core, è consigliabile usare Entity Framework Core anziché Entity Framework 6.For new .NET Core projects, we recommend use [EF Core](/ef/core) instead of Entity Framework 6. La documentazione per l'associazione dati in EF Core viene tenuta traccia in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).

# <a name="databinding-with-wpf"></a>Data binding con WPF
In questa procedura dettagliata viene illustrato come associare i tipi POCO ai controlli WPF in un form "master-detail". L'applicazione usa le API di Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.

Il modello definisce due tipi che partecipano **Category** alla relazione\\uno-a-molti: Categoria (principale master) e **Prodotto** (dettaglio dipendente).\\ Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli WPFWPF. Il framework di associazione dati WPFWPF consente lo spostamento tra oggetti correlati: la selezione di righe nella visualizzazione master causa l'aggiornamento della visualizzazione dettagli con i dati figlio corrispondenti.

Le schermate e gli elenchi di codice in questa procedura dettagliata sono tratti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Usare l'opzione 'Oggetto' per la creazione di origini dati WPFUse the 'Object' Option for Creating WPF Data Sources

Con la versione precedente di Entity Framework è consigliabile usare l'opzione Database quando si crea una nuova origine dati basata su un modello creato con la finestra di progettazione di Entity Framework.With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer. Ciò è dovuto al fatto che la finestra di progettazione genera un contesto derivato da ObjectContext e le classi di entità derivate da EntityObject.This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject. L'uso dell'opzione Database ti aiuterà a scrivere il codice migliore per interagire con questa superficie API.

Le finestre di progettazione di Entity Framework per Visual Studio 2012 e Visual Studio 2013 generano un contesto che deriva da DbContext insieme a semplici classi di entità POCO. Con Visual Studio 2010 è consigliabile passare a un modello di generazione di codice che usa DbContext come descritto più avanti in questa procedura dettagliata.

Quando si usa la superficie dell'API DbContext è necessario usare l'opzione **Oggetto** quando si crea una nuova origine dati, come illustrato in questa procedura dettagliata.

Se necessario, è possibile ripristinare la generazione di [codice basata su ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) per i modelli creati con la finestra di progettazione di EF.

## <a name="pre-requisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario che Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 sia installato.

Se si utilizza Visual Studio 2010, è necessario installare anche NuGet. Per ulteriori informazioni, vedere [Installazione di NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Creare l'applicazione

-   Aprire Visual Studio.
-   **File&gt; -&gt; Nuovo - Progetto....**
-   Selezionare **Windows** nel riquadro sinistro e **WPFApplication** nel riquadro destro
-   Immettere **WPFwithEFSample** come nome
-   Selezionare **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Installare il pacchetto NuGet di Entity FrameworkInstall the Entity Framework NuGet package

-   In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **WinFormswithEFSample**
-   Selezionare **Gestisci pacchetti NuGet...**
-   Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la scheda **Online** e scegliere il pacchetto **EntityFramework**
-   Fare clic su **Installa**  
    >[!NOTE]
    > Oltre all'assembly EntityFramework, viene aggiunto anche un riferimento a System.ComponentModel.DataAnnotations. Se il progetto contiene un riferimento a System.Data.Entity, verrà rimosso quando viene installato il pacchetto EntityFramework. L'assembly System.Data.Entity non viene più utilizzato per le applicazioni Entity Framework 6.

## <a name="define-a-model"></a>Definizione di un modello

In questa procedura dettagliata è possibile scegliere di implementare un modello utilizzando Code First o la finestra di progettazione di EF. Completare una delle due sezioni seguenti.

### <a name="option-1-define-a-model-using-code-first"></a>Opzione 1: Definire un modello utilizzando Code FirstOption 1: Define a Model using Code First

In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First. Passare alla sezione successiva **(Opzione 2: Definire un modello utilizzando Prima Database)** se si preferisce utilizzare Database First per decodificare il modello da un database usando la finestra di progettazione di EF

Quando si usa lo sviluppo Code First, in genere si inizia scrivendo classi .NET Framework che definiscono il modello concettuale (dominio).

-   Aggiungere una nuova classe a **WPFwithEFSample:**
    -   Fare clic con il pulsante destro del mouse sul nome del progetto
    -   Selezionare **Aggiungi**, quindi **Nuovo elemento**
    -   Selezionare **Classe** e immettere **Prodotto** come nome della classe
-   Sostituire la definizione della classe **Product** con il codice seguente:

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

Il **Products** proprietà il **Category** classe e **Category** proprietà il **Product** classe sono proprietà di navigazione. In Entity Framework, le proprietà di navigazione consentono di esplorare una relazione tra due tipi di entità.

Oltre a definire le entità, è necessario definire una classe che deriva&lt;da&gt; DbContext ed espone le proprietà DbSet TEntity. Le proprietà&lt;DbSet TEntity&gt; informa il contesto dei tipi che si desidera includere nel modello.

Un'istanza del tipo derivato DbContext gestisce gli oggetti entità in fase di esecuzione, che include il popolamento di oggetti con i dati di un database, il rilevamento delle modifiche e la persistenza dei dati nel database.

-   Aggiungere una nuova classe ProductContext al progetto con la definizione seguente:Add a new **ProductContext** class to the project with the following definition:

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

### <a name="option-2-define-a-model-using-database-first"></a>Opzione 2: Definire un modello utilizzando Database FirstOption 2: Define a model using Database First

In questa sezione viene illustrato come utilizzare Database First per decodificare il modello da un database utilizzando la finestra di progettazione di EF. Se è stata completata la sezione precedente (**Opzione 1: Definire un modello utilizzando Code First),** ignorare questa sezione e passare direttamente alla sezione **Caricamento lazy.**

#### <a name="create-an-existing-database"></a>Creare un database esistenteCreate an Existing Database

In genere quando si utilizza la destinazione di un database esistente, questo verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.

Il server di database installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si utilizza Visual Studio 2010 verrà creato un database SQL Express.
-   Se si utilizza Visual Studio 2012 quindi verrà creato un database [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Andiamo avanti e generare il database.

-   **Visualizza&gt; - Esplora server**
-   Fare clic con il pulsante destro del mouse su **Connessioni dati -&gt; Aggiungi connessione...**
-   Se non si è connessi a un database da Esplora server prima di dover selezionare Microsoft SQL Server come origine dati

    ![Modifica origine dati](~/ef6/media/changedatasource.png)

-   Connettersi a LocalDB o SQL Express, a seconda di quale è stato installato, e immettere **Products** come nome del database

    ![Aggiungi localDB connessione](~/ef6/media/addconnectionlocaldb.png)

    ![Aggiungi Connessione Rapida](~/ef6/media/addconnectionexpress.png)

-   Selezionare **OK** e verrà chiesto se si desidera creare un nuovo database, selezionare **Sì**

    ![Create Database](~/ef6/media/createdatabase.png)

-   Il nuovo database verrà visualizzato in Esplora server, fare clic con il pulsante destro del mouse su di esso e selezionare **Nuova query**
-   Copiare il codice SQL seguente nella nuova query, quindi fare clic con il pulsante destro del mouse sulla query e selezionare **Esegui**

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

#### <a name="reverse-engineer-model"></a>Modello Ingegnere inverso

Utilizzeremo Entity Framework Designer, incluso come parte di Visual Studio, per creare il modello.

-   **Progetto&gt; - Aggiungi nuovo elemento...**
-   Selezionare **Dati** dal menu a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **ProductModel** come nome e fare clic su **OK**
-   Verrà avviata la **procedura guidata Entity Data Model**
-   Selezionare **Genera da database** e fare clic su **Avanti**

    ![Scelta dei contenuti del modello](~/ef6/media/choosemodelcontents.png)

-   Selezionare la connessione al database creato nella prima sezione, immettere **ProductContext** come nome della stringa di connessione e fare clic su **Avanti**

    ![Scegli la tua connessione](~/ef6/media/chooseyourconnection.png)

-   Fare clic sulla casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'

    ![Scegli i tuoi oggetti](~/ef6/media/chooseyourobjects.png)

Una volta completato il processo di decodifica, il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer. Un file App.config è stato aggiunto al progetto con i dettagli di connessione per il database.

#### <a name="additional-steps-in-visual-studio-2010"></a>Passaggi aggiuntivi in Visual Studio 2010

Se si lavora in Visual Studio 2010 quindi sarà necessario aggiornare la finestra di progettazione di EF per utilizzare la generazione di codice EF6.

-   Fare clic con il pulsante destro del mouse su un punto vuoto del modello nella finestra di progettazione di EF e selezionare **Aggiungi elemento di generazione codice...**
-   Selezionare **Modelli online** dal menu a sinistra e cercare **DbContext**
-   Selezionare il **generatore DbContext di\#EF 6.x per C ,** immettere **ProductsModel** come nome e fare clic su Aggiungi

#### <a name="updating-code-generation-for-data-binding"></a>Aggiornamento della generazione di codice per l'associazione datiUpdating code generation for data binding

EF genera codice dal modello utilizzando i modelli T4. I modelli forniti con Visual Studio o scaricati dalla raccolta di Visual Studio sono destinati a scopi generali. Ciò significa che le entità generate da&lt;&gt; questi modelli dispongono di proprietà ICollection T semplici. Tuttavia, quando si esegue l'associazione dati utilizzando WPFWPF è consigliabile usare **ObservableCollection** per le proprietà della raccolta in modo che WPFWPF possa tenere traccia delle modifiche apportate alle raccolte. A tal fine si modificheranno i modelli per utilizzare ObservableCollection.

-   Aprire **Esplora soluzioni** e trovare il file **ProductModel.edmx**
-   Trovare il file **di ProductModel.tt** che verrà nidificato nel file ProductModel.edmx

    ![Modello di modello di prodotto WPFWPF Product Model Template](~/ef6/media/wpfproductmodeltemplate.png)

-   Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio
-   Trovare e sostituire le due occorrenze di "**ICollection**" con "**ObservableCollection**". Questi si trovano approssimativamente alle linee 296 e 484.
-   Trovare e sostituire la prima occorrenza di "**HashSet**" con "**ObservableCollection**". Questa occorrenza si trova approssimativamente alla riga 50. **Non** sostituire la seconda occorrenza di HashSet presente più avanti nel codice.
-   Trovare e sostituire l'unica occorrenza di "**System.Collections.Generic**" con "**System.Collections.ObjectModel**". Questo si trova approssimativamente alla linea 424.
-   Salvare il file di ProductModel.tt. In questo modo il codice per le entità da rigenerare. Se il codice non viene rigenerato automaticamente, fare clic con il pulsante destro del mouse su ProductModel.tt e scegliere "Esegui strumento personalizzato".

Se ora si apre il file di Category.cs (annidato in ProductModel.tt), si note che la raccolta Products ha il tipo **ObservableCollection&lt;Product&gt;**.

Compilare il progetto.

## <a name="lazy-loading"></a>Caricamento lazy

Il **Products** proprietà il **Category** classe e **Category** proprietà il **Product** classe sono proprietà di navigazione. In Entity Framework, le proprietà di navigazione consentono di esplorare una relazione tra due tipi di entità.

EF offre un'opzione di caricamento delle entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione. Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione verrà eseguita una query separata sul database se il contenuto non è già presente nel contesto.

Quando si usano i tipi di entità POCO, Entity Framework raggiunge il caricamento lazy creando istanze di tipi proxy derivati durante il runtime e quindi eseguire l'override delle proprietà virtuali nelle classi per aggiungere l'hook di caricamento. Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare i getter di proprietà di navigazione come **public** e **virtual** **(Overridable** in Visual Basic) e la classe non deve essere **sealed** (**NotOverridable** in Visual Basic). Quando si usano le proprietà di navigazione Di primo Database vengono automaticamente rese virtuali per abilitare il caricamento lazy. Nella sezione Code First abbiamo scelto di rendere virtuali le proprietà di navigazione per lo stesso motivo

## <a name="bind-object-to-controls"></a>Associa oggetto ai controlli

Aggiungere le classi definite nel modello come origini dati per questa applicazione WPFWPF.

-   Fare doppio clic su **MainWindow.xaml** in Esplora soluzioni per aprire il form principale
-   Dal menu principale, selezionare **Progetto -&gt; Aggiungi nuova origine dati ...**
    (in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...**)
-   Nella finestra Scegliere un tipo di origine dati, selezionare **Oggetto** e fare clic su **Avanti**
-   Nella finestra di dialogo Seleziona oggetti dati, aprire il **WPFwithEFSample** due volte e selezionare **Categoria**  
    *Non è necessario selezionare l'origine dati **Product,** perché verrà eseguito il percorso tramite la proprietà **del prodotto**nell'origine dati **Category***  

    ![Selezione di oggetti dati](~/ef6/media/selectdataobjects.png)

-   Fare clic su **Fine**.
-   La finestra Origini dati viene aperta accanto alla finestra MainWindow.xaml *Se la finestra Origini dati non viene visualizzata, selezionare Visualizza - **&gt; Altre origini&gt; dati Windows** *
-   Premere l'icona a forma di puntina, in modo che la finestra Origini dati non si nasconda automaticamente. Potrebbe essere necessario premere il pulsante di aggiornamento se la finestra era già visibile.

    ![Origini dati](~/ef6/media/datasources.png)

-   Selezionare l'origine dati **Categoria** e trascinarla nel modulo.

Quando abbiamo trascinato questa fonte, è successo quanto segue:

-   La risorsa **categoryViewSource** e il controllo **categoryDataGrid** sono stati aggiunti a XAML 
-   La proprietà DataContext nell'elemento Grid padre è stata impostata su "'StaticResource **categoryViewSource'".**La risorsa **categoryViewSource** funge da origine\\di associazione per l'elemento Grid padre esterno. Gli elementi Grid interni ereditano quindi il valore DataContext dall'elemento padre Grid (la proprietà ItemsSource di categoryDataGrid è impostata su ""Binding")

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

## <a name="adding-a-details-grid"></a>Aggiunta di una griglia dettagli

Ora che abbiamo una griglia per visualizzare categorie possiamo aggiungere una griglia di dettagli per visualizzare i prodotti associati.

-   Selezionare la proprietà **Products** nell'origine dati **Categoria** e trascinarla nel form.
    -   La risorsa **categoryProductsViewSource** e la griglia **productDataGrid** vengono aggiunte a XAML
    -   Il percorso di associazione per questa risorsa è impostato su Products
    -   Il framework di associazione dati WPF garantisce che in productDataGrid vengano visualizzati solo i prodotti correlati alla categoria selezionataWPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**
-   Dalla casella degli strumenti trascinare **Button** nel form. Impostare la proprietà **Name** su **buttonSave** e la proprietà **Content su** **Save**.

Il modulo dovrebbe essere simile al seguente:

![Finestra di progettazione](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Aggiungere il codice che gestisce l'interazione dei datiAdd Code that Handles Data Interaction

È il momento di aggiungere alcuni gestori eventi alla finestra principale.

-   Nella finestra XAML, fare clic sull'elemento ** &lt;Window,** selezionare la finestra principale
-   Nella finestra **Proprietà** scegliere **Eventi** in alto a destra, quindi fare doppio clic sulla casella di testo a destra dell'etichetta **Caricato**

    ![Proprietà della finestra principale](~/ef6/media/mainwindowproperties.png)

-   Aggiungere anche l'evento **Click** per il pulsante **Salva** facendo doppio clic sul pulsante Salva nella finestra di progettazione. 

In questo modo si accede al code-behind per il form, verrà ora modificato il codice per utilizzare il ProductContext per eseguire l'accesso ai dati. Aggiornare il codice per MainWindow come illustrato di seguito.

Il codice dichiara un'istanza a esecuzione prolungata di **ProductContext**. **L'oggetto ProductContext** viene utilizzato per eseguire query e salvare i dati nel database. **Dispose()** nell'istanza **ProductContext** viene quindi chiamato dal metodo **OnClosing** sottoposto a override.I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.

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

## <a name="test-the-wpf-application"></a>Testare l'applicazione WPFTest the WPF Application

-   Compilare l'applicazione ed eseguirla. Se è stato utilizzato Code First, si noterà che viene creato automaticamente un database **WPFwithEFSample.ProductContext.**
-   Immettere un nome di categoria nella griglia superiore e i nomi dei prodotti nella griglia inferiore *Non immettere nulla nelle colonne ID, perché la chiave primaria viene generata dal database*

    ![Finestra principale con nuove categorie e prodotti](~/ef6/media/screen1.png)

-   Premere il pulsante **Salva** per salvare i dati nel database

Dopo la chiamata a **SaveChanges()** di DbContext, gli ID vengono popolati con i valori generati dal database. Poiché abbiamo chiamato **Refresh()** dopo **SaveChanges()** i controlli **DataGrid** vengono aggiornati anche con i nuovi valori.

![Finestra principale con gli URL popolati](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Risorse aggiuntive

Per altre informazioni sull'associazione dati alle raccolte tramite WPFWPF, vedere [questo argomento](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) nella documentazione di WPFWPF.  
