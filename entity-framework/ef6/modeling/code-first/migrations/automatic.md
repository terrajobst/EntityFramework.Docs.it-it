---
title: Automatica le migrazioni Code First - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283914"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="d27ee-102">Migrazioni automatiche Code First</span><span class="sxs-lookup"><span data-stu-id="d27ee-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="d27ee-103">Processi di migrazione automatiche consente di usare migrazioni Code First senza un file di codice nel progetto per ogni modifica apportata.</span><span class="sxs-lookup"><span data-stu-id="d27ee-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="d27ee-104">Non tutte le modifiche possono essere applicate automaticamente, ad esempio Rinomina colonna richiede l'uso di una migrazione basata su codice.</span><span class="sxs-lookup"><span data-stu-id="d27ee-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="d27ee-105">Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base.</span><span class="sxs-lookup"><span data-stu-id="d27ee-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="d27ee-106">Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="d27ee-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="d27ee-107">Raccomandazione per gli ambienti di Team</span><span class="sxs-lookup"><span data-stu-id="d27ee-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="d27ee-108">È possibile alternare le migrazioni automatiche e basata su codice, ma questo non è consigliato in scenari di sviluppo del team.</span><span class="sxs-lookup"><span data-stu-id="d27ee-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="d27ee-109">Se si fa parte di un team di sviluppatori che usano controllo del codice sorgente è necessario utilizzare migrazioni puramente automatiche o le migrazioni basate esclusivamente sul codice.</span><span class="sxs-lookup"><span data-stu-id="d27ee-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="d27ee-110">Dato i limiti di processi di migrazione automatiche è consigliabile usare le migrazioni basate su codice negli ambienti di team.</span><span class="sxs-lookup"><span data-stu-id="d27ee-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="d27ee-111">Creazione del modello iniziale e del database</span><span class="sxs-lookup"><span data-stu-id="d27ee-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="d27ee-112">Per iniziare a usare le migrazioni sono necessari un progetto e un modello Code First.</span><span class="sxs-lookup"><span data-stu-id="d27ee-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="d27ee-113">In questa procedura dettagliata verranno usati i modelli tradizionali **Blog** e **Post**.</span><span class="sxs-lookup"><span data-stu-id="d27ee-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="d27ee-114">Creare una nuova **MigrationsAutomaticDemo** applicazione Console</span><span class="sxs-lookup"><span data-stu-id="d27ee-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="d27ee-115">Aggiungere la versione più recente del pacchetto NuGet **EntityFramework** al progetto</span><span class="sxs-lookup"><span data-stu-id="d27ee-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="d27ee-116">**Strumenti –&gt; Library Package Manager (Gestione pacchetti librerie) –&gt; Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="d27ee-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="d27ee-117">Eseguire il comando **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="d27ee-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="d27ee-118">Aggiungere un file **Model.cs** con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d27ee-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="d27ee-119">Questo codice definisce una singola classe **Blog** che rappresenta il modello di dominio e una classe **BlogContext** che rappresenta il contesto Code First di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d27ee-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="d27ee-120">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="d27ee-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="d27ee-121">Aggiornare il file **Program.cs** con il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d27ee-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="d27ee-122">Eseguire l'applicazione e si noterà che un **MigrationsAutomaticCodeDemo.BlogContext** database viene creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d27ee-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Database LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="d27ee-124">Abilitare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="d27ee-124">Enabling Migrations</span></span>

<span data-ttu-id="d27ee-125">È ora possibile apportare ulteriori modifiche al modello.</span><span class="sxs-lookup"><span data-stu-id="d27ee-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="d27ee-126">Introdurre una proprietà Url nella classe Blog.</span><span class="sxs-lookup"><span data-stu-id="d27ee-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="d27ee-127">Se si esegue di nuovo l'applicazione, viene generata un'eccezione InvalidOperationException con il messaggio *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*) (Il modello di creazione del contesto 'BlogContext' è stato modificato dopo la creazione del database. È possibile usare Migrazioni Code First per aggiornare il database).*</span><span class="sxs-lookup"><span data-stu-id="d27ee-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="d27ee-128">Come suggerisce l'eccezione, è ora possibile iniziare a usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="d27ee-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="d27ee-129">Poiché si vuole usare migrazioni automatiche dobbiamo specificare il **– EnableAutomaticMigrations** passare.</span><span class="sxs-lookup"><span data-stu-id="d27ee-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="d27ee-130">Eseguire la **Enable-Migrations – EnableAutomaticMigrations** comando nel Package Manager Console per questo comando è stato aggiunto un **migrazioni** cartella al nostro progetto.</span><span class="sxs-lookup"><span data-stu-id="d27ee-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="d27ee-131">Questa nuova cartella contiene un file:</span><span class="sxs-lookup"><span data-stu-id="d27ee-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="d27ee-132">**La classe Configuration.**</span><span class="sxs-lookup"><span data-stu-id="d27ee-132">**The Configuration class.**</span></span> <span data-ttu-id="d27ee-133">Questa classe consente di configurare il comportamento di Migrazioni per il contesto.</span><span class="sxs-lookup"><span data-stu-id="d27ee-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="d27ee-134">In questa procedura dettagliata verrà usata solo la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d27ee-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="d27ee-135">*Poiché è presente un solo contesto Code First nel progetto, Enable-Migrations ha specificato automaticamente il tipo di contesto a cui si applica questa configurazione.*</span><span class="sxs-lookup"><span data-stu-id="d27ee-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="d27ee-136">La prima migrazione automatica</span><span class="sxs-lookup"><span data-stu-id="d27ee-136">Your First Automatic Migration</span></span>

<span data-ttu-id="d27ee-137">Migrazioni Code First include due comandi principali.</span><span class="sxs-lookup"><span data-stu-id="d27ee-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="d27ee-138">**Add-Migration** esegue lo scaffolding della migrazione successiva in base alle modifiche apportate al modello dalla creazione dell'ultima migrazione</span><span class="sxs-lookup"><span data-stu-id="d27ee-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="d27ee-139">**Update-Database** applica tutte le migrazioni in sospeso al database</span><span class="sxs-lookup"><span data-stu-id="d27ee-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="d27ee-140">Si intende evitare utilizzando Add-Migration (a meno che non è realmente necessario) e lo stato attivo su consentendo migrazioni Code First automaticamente calcolare e applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="d27ee-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="d27ee-141">È possibile usare **Update-Database** ottenere migrazioni Code First per il push delle modifiche per il nostro modello (il nuovo **Blog.Ur**proprietà l) nel database.</span><span class="sxs-lookup"><span data-stu-id="d27ee-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="d27ee-142">Eseguire la **Update-Database** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="d27ee-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="d27ee-143">Il **MigrationsAutomaticDemo.BlogContext** database è stato aggiornato per includere le **Url** colonna il **blog** tabella.</span><span class="sxs-lookup"><span data-stu-id="d27ee-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="d27ee-144">La seconda migrazione automatica</span><span class="sxs-lookup"><span data-stu-id="d27ee-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="d27ee-145">È possibile apportare un'altra modifica e consentire migrazioni Code First automaticamente il push delle modifiche al database per noi.</span><span class="sxs-lookup"><span data-stu-id="d27ee-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="d27ee-146">Aggiungere anche una nuova classe **Post**</span><span class="sxs-lookup"><span data-stu-id="d27ee-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="d27ee-147">Si procede anche all'aggiunta delle raccolta **Posts** alla classe **Blog** per creare l'altro elemento della relazione tra **Blog** e **Post**</span><span class="sxs-lookup"><span data-stu-id="d27ee-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="d27ee-148">A questo punto usare **Update-Database** per portare il database aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d27ee-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="d27ee-149">Specificare questa volta il flag **-Verbose** per poter visualizzare il codice SQL eseguito da Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="d27ee-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="d27ee-150">Eseguire il comando **Update-Database –Verbose** nella console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="d27ee-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="d27ee-151">Aggiunta di un codice di base della migrazione</span><span class="sxs-lookup"><span data-stu-id="d27ee-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="d27ee-152">Ora diamo un'occhiata a un elemento potrebbe essere necessario usare una migrazione basata su codice per.</span><span class="sxs-lookup"><span data-stu-id="d27ee-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="d27ee-153">È possibile aggiungere un **Rating** proprietà per il **Blog** classe</span><span class="sxs-lookup"><span data-stu-id="d27ee-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="d27ee-154">È stato possibile eseguire **Update-Database** effettuare il push di queste modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="d27ee-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="d27ee-155">Tuttavia, stiamo aggiungendo un non-nullable **Blogs.Rating** colonna, se sono presenti dati esistenti nella tabella verrà assegnata l'impostazione predefinita CLR del tipo di dati per la nuova colonna (classificazione è numero intero, in modo che sarebbe **0**).</span><span class="sxs-lookup"><span data-stu-id="d27ee-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="d27ee-156">Si supponga tuttavia di voler specificare un valore predefinito **3** in modo che le righe esistenti della tabella **Blogs** abbiano inizio con una classificazione ragionevole.</span><span class="sxs-lookup"><span data-stu-id="d27ee-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="d27ee-157">È possibile usare il comando Add-Migration per scrivere questa modifica orizzontale per una migrazione basata su codice in modo che è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="d27ee-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="d27ee-158">Il **Add-Migration** comando consente di assegnare un nome di queste migrazioni, è possibile semplicemente chiamare le nostre **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="d27ee-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="d27ee-159">Eseguire la **Add-Migration AddBlogRating** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="d27ee-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="d27ee-160">Nel **migrazioni** cartella è ora disponibile una nuova **AddBlogRating** migrazione.</span><span class="sxs-lookup"><span data-stu-id="d27ee-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="d27ee-161">Il nome del file di migrazione è pre-fissa con un timestamp nella Guida in linea con l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="d27ee-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="d27ee-162">È possibile modificare il codice generato per specificare un valore predefinito pari a 3 per Blog.Rating (riga 10 nel codice seguente)</span><span class="sxs-lookup"><span data-stu-id="d27ee-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="d27ee-163">*La migrazione ha anche un file code-behind che acquisisce alcuni metadati. Questi metadati consentirà migrazioni Code First replicare i processi di migrazione automatiche è stata eseguita prima della migrazione basata su codice. Questo è importante se un altro sviluppatore desidera eseguire le migrazioni del nostro o quando si è pronti per distribuire l'applicazione.*</span><span class="sxs-lookup"><span data-stu-id="d27ee-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="d27ee-164">La migrazione modificata è ora pronta. Usare **Update-Database** per aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="d27ee-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="d27ee-165">Eseguire la **Update-Database** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="d27ee-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="d27ee-166">Tornare a processi di migrazione automatiche</span><span class="sxs-lookup"><span data-stu-id="d27ee-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="d27ee-167">Siamo ora possibile tornare a processi di migrazione automatiche per le modifiche più semplici.</span><span class="sxs-lookup"><span data-stu-id="d27ee-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="d27ee-168">Migrazioni Code First si occuperà di eseguire le migrazioni automatiche e basato sul codice nell'ordine corretto in base ai metadati che vengono archiviati nel file code-behind per ogni migrazione basata su codice.</span><span class="sxs-lookup"><span data-stu-id="d27ee-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="d27ee-169">È possibile aggiungere una proprietà Post.Abstract al nostro modello</span><span class="sxs-lookup"><span data-stu-id="d27ee-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="d27ee-170">A questo punto è possibile usare **Update-Database** ottenere migrazioni Code First di eseguire il push di questa modifica al database utilizzando una migrazione automatica.</span><span class="sxs-lookup"><span data-stu-id="d27ee-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="d27ee-171">Eseguire la **Update-Database** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="d27ee-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="d27ee-172">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d27ee-172">Summary</span></span>

<span data-ttu-id="d27ee-173">In questa procedura dettagliata che è stato illustrato come usare processi di migrazione automatiche per inviare modifiche del modello al database.</span><span class="sxs-lookup"><span data-stu-id="d27ee-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="d27ee-174">È stato anche illustrato come eseguire lo scaffolding ed eseguire le migrazioni basate su codice tra processi di migrazione automatiche, quando è necessario un maggiore controllo.</span><span class="sxs-lookup"><span data-stu-id="d27ee-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
