---
title: Associazione dati con Windows Form - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384852"
---
# <a name="databinding-with-winforms"></a>Associazione dati con Windows Form
Questa procedura dettagliata illustra come associare tipi POCO ai controlli Windows Form (WinForms) sotto forma di "master-dettagli". L'applicazione usa Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.

Il modello definisce due tipi che fanno parte di relazione uno-a-molti: categoria (principal\\master) e il prodotto (dipendenti\\dettaglio). Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli Windows Form. Il framework di data binding di Windows Form consente la navigazione tra gli oggetti correlati: se si seleziona le righe della visualizzazione master, la visualizzazione di dettaglio da aggiornare con i dati figlio corrispondente.

Le schermate e i listati di codice in questa procedura dettagliata vengono forniti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 per poter completare questa procedura dettagliata.

Se si usa Visual Studio 2010, è necessario anche installare NuGet. Per altre informazioni, vedere [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Creare l'applicazione

-   Aprire Visual Studio
-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Windows** nel riquadro sinistro e **FormsApplication Windows** nel riquadro di destra
-   Immettere **WinFormswithEFSample** come nome
-   Scegliere **OK**.

## <a name="install-the-entity-framework-nuget-package"></a>Installare il pacchetto NuGet di Entity Framework

-   In Esplora soluzioni fare clic sui **WinFormswithEFSample** progetto
-   Selezionare **Gestisci pacchetti NuGet...**
-   Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto
-   Fare clic su **installare**  
    > [!NOTE]
    > Oltre all'assembly EntityFramework viene anche aggiunto un riferimento a System.ComponentModel.DataAnnotations. Se il progetto contiene un riferimento a Data. Entity, quindi verrà rimosso quando viene installato il pacchetto di Entity Framework. L'assembly Data. Entity non è più utilizzato per le applicazioni Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementazione di IListSource per le raccolte

Le proprietà della raccolta devono implementare l'interfaccia IListSource per abilitare l'associazione dati bidirezionale con l'ordinamento quando si usa Windows Form. Per eseguire questa operazione si intende estendere ObservableCollection per aggiungere funzionalità IListSource.

-   Aggiungere un **ObservableListSource** classe al progetto:
    -   Fare doppio clic sul nome del progetto
    -   Selezionare **Add -&gt; nuovo elemento**
    -   Selezionare **classe** e immettere **ObservableListSource** per il nome della classe
-   Sostituire il codice generato per impostazione predefinita con il codice seguente:

*Questa classe consente bidirezionale dei dati di associazione, nonché l'ordinamento. La classe deriva dalla classe ObservableCollection&lt;T&gt; e aggiunge un'implementazione esplicita di IListSource. Il metodo di IListSource GetList () viene implementato per restituire un'implementazione di IBindingList che resta sincronizzata con la classe ObservableCollection. L'implementazione di IBindingList generato da ToBindingList supporta l'ordinamento. Il metodo di estensione ToBindingList è definito nell'assembly EntityFramework.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definire un modello

In questa procedura dettagliata che è possibile scelta di implementare un modello utilizzando Code First o Entity Framework Designer. Completare una delle due sezioni seguenti.

### <a name="option-1-define-a-model-using-code-first"></a>Opzione 1: Definire un modello utilizzando Code First

In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First. Passare alla sezione successiva (**Option 2: definire un modello utilizzando Database First)** se si preferisce usare Database First da invertire progettare il modello da un database utilizzando la finestra di progettazione di Entity Framework

Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).

-   Aggiungere un nuovo **prodotto** classe al progetto
-   Sostituire il codice generato per impostazione predefinita con il codice seguente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Aggiungere un **categoria** classe al progetto.
-   Sostituire il codice generato per impostazione predefinita con il codice seguente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Oltre a definire le entità, è necessario definire una classe che deriva da **DbContext** ed espone **DbSet&lt;TEntity&gt;**  proprietà. Il **DbSet** proprietà informare il contesto di quali tipi di cui si desidera includere nel modello. Il **DbContext** e **DbSet** tipi sono definiti nell'assembly EntityFramework.

Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.

-   Aggiungere un nuovo **ProductContext** classe al progetto.
-   Sostituire il codice generato per impostazione predefinita con il codice seguente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

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

Entity Framework genera codice dal modello usando i modelli T4. I modelli forniti con Visual Studio o scaricato da Visual Studio gallery servono per utilizzo generico. Ciò significa che le entità generate da questi modelli semplici ICollection&lt;T&gt; proprietà. Tuttavia, quando si esegue il data binding è preferibile usare proprietà di raccolta che implementano IListSource. Questo è il motivo per cui viene creata la classe ObservableListSource precedente e verrà ora modificare i modelli per rendere utilizzare di questa classe.

-   Aprire il **Esplora soluzioni** e trovare **ProductModel.edmx** file
-   Trovare il **ProductModel.tt** file che verrà annidato in file ProductModel.edmx

    ![Modello di prodotto](~/ef6/media/productmodeltemplate.png)

-   Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio
-   Trovare e sostituire le due occorrenze di "**ICollection**"con"**ObservableListSource**". Questi si trovano in circa righe 296 e 484.
-   Trova e Sostituisci la prima occorrenza di "**HashSet**"con"**ObservableListSource**". Questo evento si trova circa alla riga 50. **No** sostituire la seconda occorrenza della HashSet trovato in un secondo momento nel codice.
-   Salvare il file ProductModel.tt. Ciò deve generare il codice per le entità da rigenerare. Se il codice non verranno rigenerati automaticamente, quindi fare clic con il pulsante destro su ProductModel.tt e scegliere "Esegui strumento personalizzato".

Se si apre ora il file Category.cs (che è annidato sotto ProductModel.tt), si dovrebbe vedere che la raccolta di prodotti ha il tipo **ObservableListSource&lt;prodotto&gt;**.

Compilare il progetto.

## <a name="lazy-loading"></a>Caricamento lazy

Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione. In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.

Entity Framework offre un'opzione per caricare entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione. Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione una query separata verrà eseguita sul database se il contenuto non è già nel contesto.

Quando si usano i tipi di entità POCO, Entity Framework consente di ottenere il caricamento lazy creando istanze di tipi proxy derivato in fase di esecuzione e quindi si esegue l'override proprietà virtuali nelle classi per aggiungere l'hook del caricamento. Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare navigazione getter di proprietà come **pubbliche** e **virtual** (**Overridable** in Visual Basic), e di classe non deve essere **sealed** (**NotOverridable** in Visual Basic). Quando il Database di utilizzando le proprietà di navigazione prima vengono eseguite automaticamente virtuale per abilitare il caricamento lazy. Nella sezione Code First è stato scelto di rendere le proprietà di navigazione virtuale per lo stesso motivo

## <a name="bind-object-to-controls"></a>Associare oggetti ai controlli

Aggiungere le classi definite nel modello come origini dati per questa applicazione Windows Form.

-   Dal menu principale, selezionare **progetto -&gt; Aggiungi nuova origine dati...**
    (in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...** )
-   Nella pagina di scegliere una finestra del tipo di origine dati, selezionare **oggetti** e fare clic su **successivo**
-   Selezionare la finestra di dialogo di oggetti dati, Espandi il **WinFormswithEFSample** due volte e selezionare **categoria** non è necessario selezionare l'origine dati del prodotto, perché si otterrà ad esso tramite il prodotto proprietà dell'origine dati di categoria.

    ![origine dati](~/ef6/media/datasource.png)

-   Fare clic su **Finish.** 
     *Se la finestra Origini dati non verrà visualizzati, selezionare * * * - visualizzazione&gt; Other Windows -&gt; Zdroje dat**
-   Premere l'icona della puntina, in modo che la finestra Origini dati non automaticamente nascondere. Potrebbe essere necessario premere il pulsante Aggiorna se è già visualizzata la finestra.

    ![Origine dati 2](~/ef6/media/datasource2.png)

-   In Esplora soluzioni fare doppio clic il **Form1.cs** file per aprire il form principale nella finestra di progettazione.
-   Selezionare il **categoria** origine dati e trascinarlo nel form. Per impostazione predefinita, un DataGridView nuovo (**categoryDataGridView**) e i controlli della barra degli strumenti di navigazione vengono aggiunti alla finestra di progettazione. Questi controlli sono associati a BindingSource (**categoryBindingSource**) e associazione Navigator (**categoryBindingNavigator**) che vengono creati anche i componenti.
-   Modificare le colonne nel **categoryDataGridView**. Sarà necessario impostare il **CategoryId** colonna in sola lettura. Il valore per il **CategoryId** proprietà viene generata dal database risultato del salvataggio dei dati.
    -   Il pulsante destro del controllo DataGridView e selezionare Modifica colonne...
    -   Selezionare la colonna CategoryId e imposta ReadOnly su True
    -   Fare clic su OK
-   Selezionare i prodotti che si trova sotto l'origine dati di categoria e trascinarla nel form. Il productDataGridView e productBindingSource vengono aggiunti al form.
-   Modificare le colonne di productDataGridView. Si vuole nascondere le colonne CategoryId e la categoria e impostare ProductId in sola lettura. Risultato del salvataggio dei dati, il valore per la proprietà ProductId viene generato dal database.
    -   Il pulsante destro del controllo DataGridView e selezionare **Modifica colonne...** .
    -   Selezionare il **ProductId** colonna e impostare **ReadOnly** al **True**.
    -   Selezionare il **CategoryId** colonna e premere il **rimuovere** pulsante. Eseguire la stessa operazione con il **categoria** colonna.
    -   Premere **OK**.

    Finora, sono associati ai nostri controlli DataGridView BindingSource componenti nella finestra di progettazione. Nella sezione successiva si aggiungerà codice per il code-behind per impostare categoryBindingSource.DataSource alla raccolta di entità che vengono attualmente rilevate dall'oggetto DbContext. Quando viene trascinato e rilasciato i prodotti che si trova sotto la categoria, il Windows Form ha richiesto automaticamente di impostazione di proprietà productsBindingSource.DataSource da proprietà categoryBindingSource e productsBindingSource.DataMember ai prodotti. A causa di questa associazione, nel productDataGridView verranno visualizzati solo i prodotti appartenenti alla categoria correntemente selezionata.
-   Abilitare la **salvare** sulla barra degli strumenti di spostamento fare clic sul pulsante destro del mouse e selezionando **abilitato**.

    ![Finestra di progettazione modulo 1](~/ef6/media/form1-designer.png)

-   Aggiungere il gestore eventi per il salvataggio pulsante facendo doppio clic sul pulsante. Questo verrà aggiunto il gestore dell'evento e consentono di ottenere il code-behind del form. Il codice per il **categoryBindingNavigatorSaveItem\_fare clic su** gestore eventi verrà aggiunto nella sezione successiva.

## <a name="add-the-code-that-handles-data-interaction"></a>Aggiungere il codice che gestisce l'interazione dei dati

A questo punto si aggiungerà il codice per usare il ProductContext per eseguire l'accesso ai dati. Aggiornare il codice per la finestra del form principale, come illustrato di seguito.

Il codice dichiara un'istanza con esecuzione prolungata di ProductContext. L'oggetto ProductContext viene usato per eseguire query e salvare i dati nel database. Il metodo Dispose () sull'istanza ProductContext viene quindi chiamato dal metodo OnClosing sottoposto a override. I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Testare l'applicazione di Windows Form

-   Compilazione ed eseguire l'applicazione è possibile testare la funzionalità.

    ![Modulo 1 prima del salvataggio](~/ef6/media/form1beforesave.png)

-   Dopo il salvataggio le chiavi di archiviazione generati vengono visualizzate sullo schermo.

    ![Modulo 1 dopo salvataggio](~/ef6/media/form1aftersave.png)

-   Se è stato utilizzato Code First, quindi si noterà anche che un **WinFormswithEFSample.ProductContext** database viene creato automaticamente.

    ![Esplora oggetti di server](~/ef6/media/serverobjexplorer.png)
