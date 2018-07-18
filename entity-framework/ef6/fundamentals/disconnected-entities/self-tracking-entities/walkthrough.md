---
title: Procedura dettagliata entità - Entity Framework 6 con rilevamento automatico
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
caps.latest.revision: 3
ms.openlocfilehash: d07ae727ffba60a9296b34b9261acd99f7e8e3b6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121172"
---
# <a name="self-tracking-entities-walkthrough"></a>Procedura dettagliata rilevamento automatico di entità
> [!IMPORTANT]
> L'uso del modello di entità con rilevamento automatico non è più consigliabile. Continuerà a essere disponibile solo per supportare le applicazioni esistenti. Se l'applicazione richiede l'uso con grafici di entità disconnesse, prendere in considerazione altre alternative, come ad esempio [Trackable Entities](http://trackableentities.github.io/), che è una tecnologia simile alle entità con rilevamento automatico ma viene sviluppata in modo più attivo dalla community, oppure la scrittura di codice personalizzato usando le API di rilevamento delle modifiche di basso livello.

Questa procedura dettagliata illustra lo scenario in cui un servizio Windows Communication Foundation (WCF) espone un'operazione che restituisce un grafico di entità. Successivamente, un'applicazione client modifica il grafico e inoltra le modifiche a un'operazione del servizio che convalida e Salva gli aggiornamenti a un database tramite Entity Framework.

Prima di completare questa procedura dettagliata assicurarsi di leggere il [entità con rilevamento automatico](index.md) pagina.

In questa procedura dettagliata verranno completate le seguenti azioni:

-   Crea un database a cui accedere.
-   Crea una libreria di classi che contiene il modello.
-   Scambi al modello generatore di entità con rilevamento automatico.
-   Sposta le classi di entità in un progetto separato.
-   Crea un servizio WCF che espone operazioni per eseguire query e salvare le entità.
-   Crea applicazioni (Console e WPF) che utilizzano il servizio client.

Useremo Database First in questa procedura dettagliata, ma le stesse tecniche sono ugualmente applicabili a Model First.

## <a name="pre-requisites"></a>Prerequisiti

Per completare questa procedura dettagliata è necessario utilizzare una versione recente di Visual Studio.

## <a name="create-a-database"></a>Creare un Database

Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si usa Visual Studio 2012, quindi si creerà un database LocalDB.
-   Se si usa Visual Studio 2010 si creerà un database SQL Express.

È possibile procedere e generare il database.

-   Aprire Visual Studio
-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**
-   Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare **Microsoft SQL Server** come origine dati
-   Connettersi a LocalDB o SQL Express, in base alla quale è stato installato
-   Immettere **STESample** come nome del database
-   Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**
-   Il nuovo database verrà ora visualizzato in Esplora Server
-   Se si usa Visual Studio 2012
    -   Fare doppio clic sul database in Esplora Server e selezionare **nuova Query**
    -   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**
-   Se si usa Visual Studio 2010
    -   Selezionare **- Data&gt; Transact SQL Editor -&gt; nuova connessione Query...**
    -   Immettere **.\\ SQLEXPRESS** come nome del server e fare clic su **OK**
    -   Selezionare il **STESample** database dall'elenco a discesa nella parte superiore dell'editor di query
    -   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Esegui SQL**

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Creare il modello

Innanzitutto, è necessario inserire il modello in un progetto.

-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **libreria di classi**
-   Immettere **STESample** come nome, quindi fare clic su **OK**

A questo punto si creerà un modello semplice nella finestra di progettazione di Entity Framework per accedere ai database:

-   **Progetto -&gt; Aggiungi nuovo elemento...**
-   Selezionare **Data** nel riquadro a sinistra e quindi **ADO.NET Entity Data Model**
-   Immettere **BloggingModel** come nome, quindi fare clic su **OK**
-   Selezionare **genera da database** e fare clic su **successivo**
-   Immettere le informazioni di connessione per il database creato nella sezione precedente
-   Immettere **BloggingContext** come nome per la stringa di connessione e fare clic su **successivo**
-   Selezionare la casella accanto a **tabelle** e fare clic su **fine**

## <a name="swap-to-ste-code-generation"></a>Scambio per la generazione di codice STE

A questo punto è necessario disabilitare la generazione di codice predefinita e lo scambio di entità con rilevamento automatico.

### <a name="if-you-are-using-visual-studio-2012"></a>Se si usa Visual Studio 2012

-   Espandere **BloggingModel.edmx** nelle **Esplora soluzioni** ed eliminare i **BloggingModel.tt** e **BloggingModel.Context.tt** 
     *Verrà disabilitata la generazione di codice predefinita*
-   Fare doppio clic su un'area vuota di Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**
-   Selezionare **Online** dal riquadro a sinistra e cercare **STE Generator**
-   Selezionare il **generatore di Incolla per C\#**  modello, immettere **STETemplate** come nome, quindi fare clic su **Add**
-   Il **STETemplate.tt** e **STETemplate.Context.tt** vengono aggiunti file annidati sotto il file BloggingModel.edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Se si usa Visual Studio 2010

-   Fare doppio clic su un'area vuota di Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**
-   Selezionare **codice** nel riquadro a sinistra e quindi **ADO.NET self-Tracking Entity Generator**
-   Immettere **STETemplate** come nome, quindi fare clic su **Add**
-   Il **STETemplate.tt** e **STETemplate.Context.tt** file vengono aggiunti direttamente al progetto

## <a name="move-entity-types-into-separate-project"></a>Spostare i tipi di entità nel progetto separato

Per usare le entità con rilevamento automatico l'applicazione client deve accedere alle classi di entità generato dal modello. Poiché non si desidera esporre l'intero modello per l'applicazione client si userà per spostare le classi di entità in un progetto separato.

Il primo passaggio consiste arrestano la generazione di classi di entità nel progetto esistente:

-   Fare clic su **STETemplate.tt** nelle **Esplora soluzioni** e selezionare **proprietà**
-   Nel **delle proprietà** finestra chiaro **TextTemplatingFileGenerator** dal **CustomTool** proprietà
-   Espandere **STETemplate.tt** nelle **Esplora soluzioni** ed eliminare tutti i file annidati

Successivamente, si userà per aggiungere un nuovo progetto e generare le classi di entità in esso

-   **File -&gt; Add -&gt; progetto...**
-   Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **libreria di classi**
-   Immettere **STESample.Entities** come nome, quindi fare clic su **OK**
-   **Progetto -&gt; Aggiungi elemento esistente...**
-   Passare il **STESample** cartella del progetto
-   Selezionare questa opzione per visualizzare **tutti i file (\*.\*)**
-   Selezionare il **STETemplate.tt** file
-   Fare clic sulla freccia a discesa accanto al **Add** e selezionare **Aggiungi come collegamento**

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

Eseguiremo, inoltre, per assicurarsi che le classi di entità vengono generate nella directory lo stesso spazio dei nomi come contesto. Solo questo riduce il numero dell'utilizzo delle istruzioni che è necessario aggiungere in tutta la nostra applicazione.

-   Fare clic su collegato **STETemplate.tt** nelle **Esplora soluzioni** e selezionare **proprietà**
-   Nel **delle proprietà** finestra set **Custom Tool Namespace** a **STESample**

Il codice generato dal modello di STE sarà necessario un riferimento a **Serialization** per compilare. Questa libreria è necessaria per WCF **DataContract** e **DataMember** gli attributi utilizzati sui tipi di entità serializzabili.

-   Fare clic con il pulsante destro sul **STESample.Entities** del progetto **Esplora soluzioni** e selezionare **Aggiungi riferimento...**
    -   In Visual Studio 2012 - selezionare la casella accanto a **Serialization** e fare clic su **OK**
    -   In Visual Studio 2010 - selezionare **Serialization** e fare clic su **OK**

Infine, il progetto con il contesto in esso sarà necessario un riferimento ai tipi di entità.

-   Fare clic con il pulsante destro sul **STESample** del progetto **Esplora soluzioni** e selezionare **Aggiungi riferimento...**
    -   In Visual Studio 2012 - selezionare **soluzione** nel riquadro sinistro, selezionare la casella accanto a **STESample.Entities** e fare clic su **OK**
    -   In Visual Studio 2010 - selezionare il **progetti** scheda, seleziona **STESample.Entities** e fare clic su **OK**

>[!NOTE]
> Un'altra opzione per spostare i tipi di entità in un progetto separato consiste nello spostare il file di modello, anziché il relativo collegamento dalla posizione predefinita. In questo caso, è necessario aggiornare il **inputFile** variabili nel modello per fornire il percorso relativo al file con estensione edmx (in questo esempio che sarà **.. \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Creare un servizio WCF

A questo punto è possibile aggiungere un servizio WCF di esporre i dati, si inizierà creando il progetto.

-   **File -&gt; Add -&gt; progetto...**
-   Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione del servizio WCF**
-   Immettere **STESample.Service** come nome, quindi fare clic su **OK**
-   Aggiungere un riferimento per la **Data. Entity** assembly
-   Aggiungere un riferimento per la **STESample** e **STESample.Entities** progetti

È necessario copiare la stringa di connessione Entity Framework per questo progetto in modo che si trova in fase di esecuzione.

-   Aprire il **app. config** del file per la * * STESample * * progetto e copiare la **connectionStrings** elemento
-   Incollare il **connectionStrings** come elemento figlio dell'elemento il **configuration** elemento del **Web. config** del file nel **STESample.Service** progetto

A questo punto è possibile implementare il servizio effettivo.

-   Aprire **IService1.cs** e sostituire il contenuto con il codice seguente

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Aprire **Service1.svc** e sostituire il contenuto con il codice seguente

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Uso del servizio da un'applicazione Console

Creare un'applicazione console che usa il nostro servizio.

-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione Console**
-   Immettere **STESample.ConsoleTest** come nome, quindi fare clic su **OK**
-   Aggiungere un riferimento per la **STESample.Entities** progetto

È necessario un riferimento al nostro servizio WCF

-   Fare doppio clic sui **STESample.ConsoleTest** del progetto nelle **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**
-   Fare clic su **individuare**
-   Immettere **BloggingService** come spazio dei nomi e fare clic su **OK**

Sarà quindi possibile scrivere codice per utilizzare il servizio.

-   Aprire **Program.cs** e sostituire il contenuto con il codice seguente.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

È ora possibile eseguire l'applicazione per vederla in azione.

-   Fare doppio clic sui **STESample.ConsoleTest** del progetto nelle **Esplora soluzioni** e selezionare **Debug -&gt; Avvia nuova istanza**

Si noterà l'output seguente quando viene eseguita l'applicazione.

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Uso del servizio da un'applicazione WPF

Creare un'applicazione WPF che usa il nostro servizio.

-   **File -&gt; New -&gt; progetto...**
-   Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione WPF**
-   Immettere **STESample.WPFTest** come nome, quindi fare clic su **OK**
-   Aggiungere un riferimento per la **STESample.Entities** progetto

È necessario un riferimento al nostro servizio WCF

-   Fare doppio clic sui **STESample.WPFTest** del progetto nelle **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**
-   Fare clic su **individuare**
-   Immettere **BloggingService** come spazio dei nomi e fare clic su **OK**

Sarà quindi possibile scrivere codice per utilizzare il servizio.

-   Aprire **MainWindow. XAML** e sostituire il contenuto con il codice seguente.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Aprire il code-behind per MainWindow (**MainWindow.xaml.cs**) e sostituire il contenuto con il codice seguente

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

È ora possibile eseguire l'applicazione per vederla in azione.

-   Fare doppio clic sui **STESample.WPFTest** del progetto nelle **Esplora soluzioni** e selezionare **Debug -&gt; Avvia nuova istanza**
-   È possibile modificare i dati utilizzando la schermata e salvare il file tramite il servizio utilizzando il **salvare** pulsante

![WPF](~/ef6/media/wpf.png)
