---
title: Provider di database - EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: daf2e06c76ed55213243f5728548fdfd4be0e5e2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413156"
---
# <a name="database-providers"></a>Provider di database

Entity Framework Core può accedere a molti database diversi tramite librerie plug-in denominate provider di database.

## <a name="current-providers"></a>Provider correnti

> [!IMPORTANT]  
> I provider di EF Core vengono compilati da diverse origini. Non tutti i provider vengono gestiti nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Quando si prende in considerazione un provider, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti. Assicurarsi anche di esaminare la documentazione di ogni provider per informazioni dettagliate sulla compatibilità delle versioni.

> [!IMPORTANT]  
> I provider EF Core in genere funzionano tra versioni secondarie, ma non tra versioni principali. Ad esempio, un provider rilasciato per EF Core 2.1 funzionerà con EF Core 2.2, ma non funzionerà con EF Core 3.0. 

| Pacchetto NuGet                                                                                                        | Motori di database supportati | Gestore / fornitore                                                           | Note / requisiti | Compilato per la versione | Collegamenti utili                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 e versioni successive    | [Progetto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 e versioni successive         | [Progetto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Database in memoria EF Core | [Progetto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | [Limitazioni](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | API SQL di Azure Cosmos DB    | [Progetto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [docs](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Team di sviluppo Npgsql](https://github.com/npgsql)                          |                      | 3.1               | [docs](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Progetto Pomelo Foundation](https://github.com/PomeloFoundation)              |                      | 3.1               | [leggimi](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 e versioni successive            | [DevArt](https://www.devart.com/)                                             | Paid                 | 3.0               | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 e versioni successive  | [DevArt](https://www.devart.com/)                                             | Paid                 | 3.0               | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 e versioni successive     | [DevArt](https://www.devart.com/)                                             | Paid                 | 3.0               | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 e versioni successive           | [DevArt](https://www.devart.com/)                                             | Paid                 | 3.0               | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                                   | Archivia i dati nei file       | [Morris Janatzek](https://github.com/morrisjdev)                              | A scopo di sviluppo | 3.0               | [leggimi](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | File di Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | 2.2               | [leggimi](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 e 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | 2.2               | [docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [Teradata.EntityFrameworkCore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                         | Database Teradata 16.10 e versioni successive | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) | | 2.2               |[Sito Web](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 e 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | 2.1               | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | 2.1               | [leggimi](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Progetto MySQL](https://dev.mysql.com) (Oracle)                               |                      | 2.1               | [docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 e versioni successive     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   |                      | 2.1               | [Sito Web](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versione di Windows      | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versione di Linux        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versione macOS        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Server MyCAT               | [Progetto Pomelo Foundation](https://github.com/PomeloFoundation)              | Solo versione preliminare      | 1.1               | [leggimi](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Aggiunta di un provider di database all'applicazione

La maggior parte dei provider di database per EF Core viene distribuita in forma di pacchetti NuGet e può essere installata come segue:

## <a name="net-core-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Dopo l'installazione, il provider verrà configurato nel `DbContext`, nel metodo `OnConfiguring` o nel metodo `AddDbContext` se si usa un contenitore di inserimento delle dipendenze.
Ad esempio, la riga seguente consente di configurare il provider SQL Server con la stringa di connessione passata:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

I provider di database possono estendere EF Core per abilitare funzionalità univoche per database specifici.
Alcuni concetti sono comuni alla maggior parte dei database e sono inclusi nei componenti primari di EF Core.
Tali concetti includono l'espressione di query in LINQ, le transazioni e il rilevamento delle modifiche agli oggetti dopo che sono stati caricati dal database.
Alcuni concetti sono specifici di un determinato provider.
Il provider SQL Server, ad esempio, consente di [configurare le tabelle ottimizzate per la memoria](xref:core/providers/sql-server/memory-optimized-tables), una funzionalità specifica di SQL Server.
Altri concetti sono specifici di una classe di provider.
Ad esempio, i provider di EF Core per i database relazionali compilati sulla libreria `Microsoft.EntityFrameworkCore.Relational` comune, che offre le API per la configurazione della tabella e il mapping colonne, i vincoli della chiave esterna e così via. I provider vengono in genere distribuiti come pacchetti NuGet.

> [!IMPORTANT]  
> Quando viene rilasciata una nuova versione di patch di EF Core, spesso include aggiornamenti per il pacchetto `Microsoft.EntityFrameworkCore.Relational`.
> Quando si aggiunge un provider di database relazionale, il pacchetto diventa una dipendenza transitiva dell'applicazione.
> Molti provider vengono però rilasciati in modo indipendente da EF Core e potrebbero non essere aggiornati in modo da dipendere dalla versione di patch più recente del pacchetto.
> Per assicurarsi di ottenere tutte le correzioni di bug, è consigliabile aggiungere la versione di patch di `Microsoft.EntityFrameworkCore.Relational` come dipendenza diretta dell'applicazione.
