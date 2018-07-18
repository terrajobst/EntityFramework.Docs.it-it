---
title: Automatica le migrazioni Code First - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121113"
---
# <a name="automatic-code-first-migrations"></a>Migrazioni automatiche Code First
Processi di migrazione automatiche consente di usare migrazioni Code First senza un file di codice nel progetto per ogni modifica apportata. Non tutte le modifiche possono essere applicate automaticamente, ad esempio Rinomina colonna richiede l'uso di una migrazione basata su codice.

> [!NOTE]
> Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base. Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.

## <a name="recommendation-for-team-environments"></a>Raccomandazione per gli ambienti di Team

È possibile alternare le migrazioni automatiche e basata su codice, ma questo non è consigliato in scenari di sviluppo del team. Se si fa parte di un team di sviluppatori che usano controllo del codice sorgente è necessario utilizzare migrazioni puramente automatiche o le migrazioni basate esclusivamente sul codice. Dato i limiti di processi di migrazione automatiche è consigliabile usare le migrazioni basate su codice negli ambienti di team.

## <a name="building-an-initial-model--database"></a>Creazione del modello iniziale e del database

Per iniziare a usare le migrazioni sono necessari un progetto e un modello Code First. In questa procedura dettagliata verranno usati i modelli tradizionali **Blog** e **Post**.

-   Creare una nuova **MigrationsAutomaticDemo** applicazione Console
-   Aggiungere la versione più recente del pacchetto NuGet **EntityFramework** al progetto
    -   **Strumenti –&gt; Library Package Manager (Gestione pacchetti librerie) –&gt; Console di Gestione pacchetti**
    -   Eseguire il comando **Install-Package EntityFramework**
-   Aggiungere un file **Model.cs** con il codice riportato di seguito. Questo codice definisce una singola classe **Blog** che rappresenta il modello di dominio e una classe **BlogContext** che rappresenta il contesto Code First di Entity Framework

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   È ora possibile usare il modello per eseguire l'accesso ai dati. Aggiornare il file **Program.cs** con il codice riportato di seguito.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Eseguire l'applicazione e si noterà che un **MigrationsAutomaticCodeDemo.BlogContext** database viene creato automaticamente.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Abilitare le migrazioni

È ora possibile apportare ulteriori modifiche al modello.

-   Introdurre una proprietà Url nella classe Blog.

``` csharp
    public string Url { get; set; }
```

Se si esegue di nuovo l'applicazione, viene generata un'eccezione InvalidOperationException con il messaggio *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*) (Il modello di creazione del contesto 'BlogContext' è stato modificato dopo la creazione del database. È possibile usare Migrazioni Code First per aggiornare il database).*

Come suggerisce l'eccezione, è ora possibile iniziare a usare Migrazioni Code First. Poiché si vuole usare migrazioni automatiche dobbiamo specificare il **– EnableAutomaticMigrations** passare.

-   Eseguire la **Enable-Migrations – EnableAutomaticMigrations** comando nel Package Manager Console per questo comando è stato aggiunto un **migrazioni** cartella al nostro progetto. Questa nuova cartella contiene un file:

-   **La classe Configuration.** Questa classe consente di configurare il comportamento di Migrazioni per il contesto. In questa procedura dettagliata verrà usata solo la configurazione predefinita.
    *Poiché è presente un solo contesto Code First nel progetto, Enable-Migrations ha specificato automaticamente il tipo di contesto a cui si applica questa configurazione.*

 

## <a name="your-first-automatic-migration"></a>La prima migrazione automatica

Migrazioni Code First include due comandi principali.

-   **Add-Migration** esegue lo scaffolding della migrazione successiva in base alle modifiche apportate al modello dalla creazione dell'ultima migrazione
-   **Update-Database** applica tutte le migrazioni in sospeso al database

Si intende evitare utilizzando Add-Migration (a meno che non è realmente necessario) e lo stato attivo su consentendo migrazioni Code First automaticamente calcolare e applicare le modifiche. È possibile usare **Update-Database** ottenere migrazioni Code First per il push delle modifiche per il nostro modello (il nuovo **Blog.Ur**proprietà l) nel database.

-   Eseguire la **Update-Database** comando nella Console di gestione pacchetti.

Il **MigrationsAutomaticDemo.BlogContext** database è stato aggiornato per includere le **Url** colonna il **blog** tabella.

 

## <a name="your-second-automatic-migration"></a>La seconda migrazione automatica

È possibile apportare un'altra modifica e consentire migrazioni Code First automaticamente il push delle modifiche al database per noi.

-   Aggiungere anche una nuova classe **Post**

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Si procede anche all'aggiunta delle raccolta **Posts** alla classe **Blog** per creare l'altro elemento della relazione tra **Blog** e **Post**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

A questo punto usare **Update-Database** per portare il database aggiornato. Specificare questa volta il flag **-Verbose** per poter visualizzare il codice SQL eseguito da Migrazioni Code First.

-   Eseguire il comando **Update-Database –Verbose** nella console di Gestione pacchetti.

## <a name="adding-a-code-based-migration"></a>Aggiunta di un codice di base della migrazione

Ora diamo un'occhiata a un elemento potrebbe essere necessario usare una migrazione basata su codice per.

-   È possibile aggiungere un **Rating** proprietà per il **Blog** classe

``` csharp
    public int Rating { get; set; }
```

È stato possibile eseguire **Update-Database** effettuare il push di queste modifiche al database. Tuttavia, stiamo aggiungendo un non-nullable **Blogs.Rating** colonna, se sono presenti dati esistenti nella tabella verrà assegnata l'impostazione predefinita CLR del tipo di dati per la nuova colonna (classificazione è numero intero, in modo che sarebbe **0**). Si supponga tuttavia di voler specificare un valore predefinito **3** in modo che le righe esistenti della tabella **Blogs** abbiano inizio con una classificazione ragionevole.
È possibile usare il comando Add-Migration per scrivere questa modifica orizzontale per una migrazione basata su codice in modo che è possibile modificarlo. Il **Add-Migration** comando consente di assegnare un nome di queste migrazioni, è possibile semplicemente chiamare le nostre **AddBlogRating**.

-   Eseguire la **Add-Migration AddBlogRating** comando nella Console di gestione pacchetti.
-   Nel **migrazioni** cartella è ora disponibile una nuova **AddBlogRating** migrazione. Il nome del file di migrazione è pre-fissa con un timestamp nella Guida in linea con l'ordinamento. È possibile modificare il codice generato per specificare un valore predefinito pari a 3 per Blog.Rating (riga 10 nel codice seguente)

*La migrazione ha anche un file code-behind che acquisisce alcuni metadati. Questi metadati consentirà migrazioni Code First replicare i processi di migrazione automatiche è stata eseguita prima della migrazione basata su codice. Questo è importante se un altro sviluppatore desidera eseguire le migrazioni del nostro o quando si è pronti per distribuire l'applicazione.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

La migrazione modificata è ora pronta. Usare **Update-Database** per aggiornare il database.

-   Eseguire la **Update-Database** comando nella Console di gestione pacchetti.

## <a name="back-to-automatic-migrations"></a>Tornare a processi di migrazione automatiche

Siamo ora possibile tornare a processi di migrazione automatiche per le modifiche più semplici. Migrazioni Code First si occuperà di eseguire le migrazioni automatiche e basato sul codice nell'ordine corretto in base ai metadati che vengono archiviati nel file code-behind per ogni migrazione basata su codice.

-   È possibile aggiungere una proprietà Post.Abstract al nostro modello

``` csharp
    public string Abstract { get; set; }
```

A questo punto è possibile usare **Update-Database** ottenere migrazioni Code First di eseguire il push di questa modifica al database utilizzando una migrazione automatica.

-   Eseguire la **Update-Database** comando nella Console di gestione pacchetti.

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata che è stato illustrato come usare processi di migrazione automatiche per inviare modifiche del modello al database. È stato anche illustrato come eseguire lo scaffolding ed eseguire le migrazioni basate su codice tra processi di migrazione automatiche, quando è necessario un maggiore controllo.
