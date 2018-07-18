---
title: Personalizzare la tabella di cronologia migrazioni - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
caps.latest.revision: 3
ms.openlocfilehash: 3805d99be6d37d037096f5c5fb69fd30197c1e91
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121322"
---
# <a name="customizing-the-migrations-history-table"></a>La tabella di cronologia le migrazioni di personalizzazione
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

> [!NOTE]
> Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base. Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.

## <a name="what-is-migrations-history-table"></a>Che cos'è la tabella di cronologia migrazioni?

Tabella di cronologia migrazioni è una tabella utilizzata dal migrazioni Code First per archiviare informazioni dettagliate sulle migrazioni applicate al database. Per impostazione predefinita è il nome della tabella nel database \_ \_MigrationHistory e viene creato quando si applica al primo non di migrazione del database. In Entity Framework 5 questa tabella è una tabella di sistema se l'applicazione utilizzata database Microsoft Sql Server. Questo è stato modificato in Entity Framework 6 tuttavia e la tabella di cronologia migrazioni non è più contrassegnata una tabella di sistema.

## <a name="why-customize-migrations-history-table"></a>Il motivo per cui personalizzare la tabella di cronologia migrazioni?

Tabella di cronologia migrazioni dovrebbe essere utilizzate esclusivamente per le migrazioni Code First e può essere modificato manualmente può interrompere le migrazioni. Tuttavia in alcuni casi la configurazione predefinita non è appropriata e la tabella deve essere personalizzato, ad esempio:

-   È necessario modificare i nomi e/o i facet di colonne per abilitare un 3<sup>rd</sup> provider le migrazioni di terze parti
-   Si desidera modificare il nome della tabella
-   È necessario usare uno schema diverso per il \_ \_MigrationHistory tabella
-   È necessario archiviare dati aggiuntivi per una determinata versione del contesto e pertanto è necessario aggiungere un'altra colonna alla tabella

## <a name="words-of-precaution"></a>Parole di precauzione

Modificare la tabella di cronologia della migrazione è potente, ma è necessario prestare attenzione a non esagerare. Runtime di Entity Framework attualmente non verifica se la tabella di cronologia migrazioni personalizzato è compatibile con il runtime. Se non è l'applicazione potrebbe interrompersi durante l'esecuzione o si comportano in modo imprevedibile. Questo è ancora più importante se si usano più contesti per ogni database nel qual caso più contesti possono usare la stessa tabella di cronologia di migrazione per archiviare le informazioni sulle migrazioni.

## <a name="how-to-customize-migrations-history-table"></a>Come personalizzare la tabella di cronologia migrazioni?

Prima di iniziare è necessario sapere che è possibile personalizzare la tabella di cronologia migrazioni solo prima di applicare la prima migrazione. A questo punto, al codice.

In primo luogo, è necessario creare una classe derivata dalla classe System.Data.Entity.Migrations.History.HistoryContext. La classe HistoryContext è derivata dalla classe DbContext in modo che la configurazione della tabella di cronologia migrazioni è molto simile alla configurazione dei modelli di Entity Framework con l'API fluent. È sufficiente eseguire l'override del metodo OnModelCreating e usare l'API fluent per configurare la tabella.

>[!NOTE]
> In genere quando si configurano modelli EF non occorre chiamare base. Orderingcontext.cs dal metodo OnModelCreating sottoposto a override in quanto il DbContext.OnModelCreating() contiene un corpo vuoto. Ciò non avviene quando si configura la tabella di cronologia delle migrazioni. In questo caso la prima operazione da eseguire nell'override Orderingcontext.cs è effettivamente chiamare base. Orderingcontext.cs. La tabella di cronologia migrazioni verrà configurato in modo predefinito che quindi possibile perfezionare nel metodo di overriding.

Si supponga di che voler rinominare la tabella di cronologia le migrazioni e inserirlo in uno schema personalizzato denominato "admin". Anche l'amministratore del database desidera è possibile rinominare la colonna MigrationId migrazione\_ID.  È possibile ottenere questo risultato creando la seguente classe derivata da HistoryContext:

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Una volta pronto il HistoryContext personalizzato è necessario apportare EF consapevole registrandolo tramite [configurazione basata su codice](http://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

È praticamente tutto. A questo punto è possibile passare alla Console di gestione pacchetti, Enable-Migrations, Add-Migration e infine Update-Database. Dovrebbe essere generato in aggiunta al database di una tabella di cronologia migrazioni configurata in base ai dettagli specificati nella classe HistoryContext derivato.

![Database](~/ef6/media/database.png)
