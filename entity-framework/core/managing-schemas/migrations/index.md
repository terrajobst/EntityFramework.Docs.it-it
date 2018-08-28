---
title: Migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996522"
---
<a name="migrations"></a>Migrazioni
==========
Le migrazioni rappresentano un metodo per l'applicazione di modifiche dello schema al database, per mantenere quest'ultimo sincronizzato con il modello Entity Framework Core mantenendo i dati esistenti nel database.

<a name="creating-the-database"></a>Creazione del database
---------------------
Dopo la [definizione del modello iniziale ][1] è il momento di creare il database. A tale scopo, aggiungere una migrazione iniziale.
Installare [Entity Framework Core Tools] [ 2] ed eseguire il comando appropriato.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Nella directory **Migrations** del progetto vengono aggiunti tre file:

* **00000000000000_InitialCreate.cs**: file principale della migrazione. Contiene le operazioni necessarie per applicare la migrazione (in `Up()`) e per ripristinarla (in `Down()`).
* **00000000000000_InitialCreate.Designer.cs**: file di metadati della migrazione. Contiene informazioni usate da Entity Framework.
* **MyContextModelSnapshot.cs**: snapshot del modello corrente. Usato per determinare cosa è stato modificato durante l'aggiunta della migrazione successiva.

Il timestamp nel nome file consente di mantenerne l'ordine cronologico e di visualizzare quindi la successione delle modifiche.

> [!TIP]
> È consentito spostare i file della cartella Migrations e modificarne lo spazio dei nomi. Le nuove migrazioni vengono create come migrazioni di pari livello rispetto all'ultima.

Applicare quindi la migrazione al database per creare lo schema.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Aggiunta di un'altra migrazione
------------------------
Dopo le modifiche al modello di Entity Framework Core, lo schema del database non è sincronizzato. Per aggiornarlo, aggiungere un'altra migrazione. Il nome della migrazione può essere usato come messaggio di commit in un sistema di controllo della versione. Se, ad esempio, si apportano modifiche per salvare le recensioni di prodotti create dai clienti, un nome possibile potrebbe essere *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Dopo lo scaffolding della migrazione è consigliabile verificare l'accuratezza della migrazione stessa, aggiungendo eventuali operazioni necessarie a garantirne una corretta applicazione. La migrazione, ad esempio, può contenere le operazioni seguenti:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Queste operazioni rendono compatibile lo schema del database, ma non consentono di mantenere i nomi dei clienti esistenti. Per migliorarle, riscriverle come segue.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Durante l'aggiunta di una nuova migrazione viene visualizzato un avviso se lo scaffolding di un'operazione può causare una perdita di dati, ad esempio nel caso della rimozione di una colonna. Assicurarsi di controllare l'accuratezza di queste migrazioni con particolare attenzione.

Applicare la migrazione al database tramite il comando appropriato.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Rimozione di una migrazione
--------------------
Dopo l'aggiunta di una migrazione ci si rende talvolta conto che prima di applicarla sono necessarie altre modifiche al modello di Entity Framework Core.
Per rimuovere l'ultima migrazione, usare questo comando.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Dopo la rimozione è possibile apportare le modifiche aggiuntive al modello. La migrazione può quindi essere aggiunta di nuovo.

<a name="reverting-a-migration"></a>Ripristino di una migrazione
---------------------
Se una o più migrazioni sono già state applicate ma è necessario ripristinarle, è possibile usare lo stesso comando impiegato per applicarle, specificando però il nome della migrazione a cui si vuole eseguire il rollback.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Migrazioni vuote
----------------
È talvolta utile aggiungere una migrazione senza apportare alcuna modifica al modello. In questo caso, l'aggiunta di una nuova migrazione crea una migrazione vuota. È possibile personalizzare questa migrazione per eseguire operazioni non direttamente correlate al modello di Entity Framework Core.
Ecco alcune operazioni che è necessario gestire in questo modo:

* Ricerca full-text
* Funzioni
* Stored procedure
* Trigger
* Visualizzazioni
* e così via

<a name="generating-a-sql-script"></a>Generazione di uno script SQL
-----------------------
Per il debug delle migrazioni o la distribuzione di queste in un database di produzione è utile generare uno script SQL. Lo script può quindi essere rivisto per garantirne la correttezza e ottimizzato in base alle esigenze del database di produzione. È anche possibile combinare lo script con una tecnologia di distribuzione. Il comando di base è il seguente.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Con questo comando è possibile usare diverse opzioni.

La migrazione **di origine** deve essere l'ultima migrazione applicata al database prima dell'esecuzione dello script. Se non è stata applicata alcuna migrazione, specificare `0` (valore predefinito).

La migrazione **di destinazione**  è l'ultima migrazione applicata al database dopo l'esecuzione dello script. L'impostazione predefinita corrisponde all'ultima migrazione nel progetto.

Facoltativamente, è possibile generare uno script **idempotente**. Questo tipo di script applica le migrazioni solo se non sono già state applicate al database ed è utile se non si sa esattamente quale migrazione è stata applicata per ultima al database o se si stanno applicando migrazioni a più database a cui in precedenza sono state applicate migrazioni diverse.

<a name="applying-migrations-at-runtime"></a>Applicazione di migrazioni in runtime
------------------------------
Alcune applicazioni devono applicare migrazioni in runtime durante l'avvio o la prima esecuzione. In questo caso usare il metodo `Migrate()`.

È tuttavia necessario prestare attenzione, perché questo approccio non è adatto a tutte le situazioni. È utile per applicazioni con un database locale, ma la maggior parte delle applicazioni richiede una strategia di distribuzione più solida, ad esempio la generazione di script SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Non chiamare `EnsureCreated()` prima di `Migrate()`. `EnsureCreated()` ignora la cartella Migrations per creare lo schema, causando un errore di `Migrate()`.

> [!NOTE]
> Questo metodo si basa sul servizio `IMigrator`, che può essere usato per scenari più avanzati. Usare `DbContext.GetService<IMigrator>()` per accedervi.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
