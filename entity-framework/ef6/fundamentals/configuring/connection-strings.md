---
title: Le stringhe di connessione e modelli - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122760"
---
# <a name="connection-strings-and-models"></a>Modelli e le stringhe di connessione
In questo argomento illustra come Entity Framework consente di individuare la connessione di database da usare e il modo in cui è possibile modificarlo. I modelli creati con Code First e la finestra di progettazione di Entity Framework sono descritte in questo argomento.  

In genere un'applicazione Entity Framework Usa una classe derivata da DbContext. Questa classe derivata verrà chiamare uno dei costruttori nella classe base DbContext al controllo:  

- Modalità di connessione il contesto a un database, ovvero come una stringa di connessione è trovare/usata  
- Se verrà utilizzato il contesto di calcolare un modello utilizzando Code First o caricare un modello creato con la finestra di progettazione di Entity Framework  
- Opzioni avanzate aggiuntive  

I frammenti seguenti illustrano che alcuni modi i costruttori DbContext è utilizzabile.  

## <a name="use-code-first-with-connection-by-convention"></a>Utilizzare Code First con connessione per convenzione  

Se non è ancora eventuali altre opzioni di configurazione nell'applicazione, quindi chiamare il costruttore senza parametri in DbContext causerà DbContext per l'esecuzione in modalità Code First con una connessione al database creata per convenzione. Ad esempio:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

In questo esempio DbContext Usa il nome completo dello spazio dei nomi di class—Demo.EF.BloggingContext—as del contesto derivato il nome del database e crea una stringa di connessione per il database usando SQL Express o LocalDB. Se entrambi sono installati, SQL Express verrà utilizzato.  

Visual Studio 2010 include SQL Express per impostazione predefinita e Visual Studio 2012 e versioni successive include LocalDB. Durante l'installazione, il pacchetto EntityFramework NuGet controlla quali server di database è disponibile. Il pacchetto NuGet aggiornerà quindi il file di configurazione, impostare il server di database predefinito che usa Code First quando si crea una connessione per convenzione. Se SQL Express è in esecuzione, verrà utilizzato. Se SQL Express non è disponibile quindi LocalDB verrà registrato come impostazione predefinita invece. Non vengono apportate modifiche al file di configurazione se contiene già un'impostazione per la factory di connessione predefinita.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Uso di Code First con una connessione con convenzione e nome del database specificato  

Se non è ancora eventuali altre opzioni di configurazione nell'applicazione, quindi chiamare il costruttore string in DbContext con il nome del database da usare causerà DbContext per l'esecuzione in modalità Code First con una connessione al database creata per convenzione per il database di tale nome. Ad esempio:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

In questo esempio DbContext Usa "BloggingDatabase" come nome del database e crea una stringa di connessione per il database usando SQL Express (installato con Visual Studio 2010) o database locale (installato con Visual Studio 2012). Se entrambi sono installati, SQL Express verrà utilizzato.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Utilizzare Code First con stringa di connessione nel file app.config/web.config  

È possibile scegliere di inserire una stringa di connessione nel file app. config o Web. config. Ad esempio:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Si tratta di un metodo semplice per usare un server di database diverso da SQL Express o LocalDB DbContext, ovvero l'esempio precedente viene specificato un database di SQL Server Compact Edition.  

Se il nome della stringa di connessione corrisponde al nome del contesto (con o senza qualifica dello spazio dei nomi) quindi si troverà da DbContext quando viene usato il costruttore senza parametri. Se il nome di stringa di connessione è diverso dal nome del contesto è possibile indicare a DbContext per usare la connessione in modalità Code First passando il nome di stringa di connessione per il costruttore DbContext. Ad esempio:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

In alternativa, è possibile usare il formato "nome =\<nome stringa di connessione\>" per la stringa passata al costruttore DbContext. Ad esempio:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Questo formato rende esplicita che si prevede che la stringa di connessione possano essere trovati in file di configurazione. Se una stringa di connessione con il nome specificato non viene trovata, verrà generata un'eccezione.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Database/Model First con stringa di connessione nel file app.config/web.config  

I modelli creati con la finestra di progettazione di Entity Framework sono diversi da Code First in quanto il modello esiste già e non viene generato dal codice quando viene eseguita l'applicazione. Il modello è in genere presente come un file EDMX nel progetto.  

La finestra di progettazione aggiungerà una stringa di connessione Entity Framework nel file app. config o Web. config. Questa stringa di connessione è speciale perché contiene informazioni su come trovare le informazioni nel file EDMX. Ad esempio:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

La finestra di progettazione di Entity Framework genera anche codice che chiede di DbContext per usare la connessione passando il nome di stringa di connessione per il costruttore DbContext. Ad esempio:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext è in grado di caricare il modello esistente (anziché di utilizzare Code First per calcolarla dal codice) perché la stringa di connessione è una stringa di connessione EF contenente i dettagli del modello da usare.  

## <a name="other-dbcontext-constructor-options"></a>Altre opzioni di costruttore DbContext  

La classe DbContext contiene altri costruttori e i modelli di utilizzo che consentono alcuni scenari più avanzati. Alcune di queste sono:  

- È possibile usare la classe DbModelBuilder per compilare un modello Code First senza creare un'istanza di un'istanza di DbContext. Il risultato di questo è un oggetto DbModel. È quindi possibile passare questo oggetto DbModel a uno dei costruttori DbContext quando si è pronti per creare l'istanza di DbContext.  
- È possibile passare una stringa di connessione completa per DbContext anziché solo il nome di stringa del database o una connessione. Per impostazione predefinita questa stringa di connessione viene usata con il provider SqlClient; Ciò può essere modificato impostando un'implementazione diversa della IConnectionFactory nel contesto. Database.DefaultConnectionFactory.  
- È possibile usare un oggetto DbConnection esistente passandolo al costruttore oggetto DbContext. Se l'oggetto di connessione è un'istanza di EntityConnection, quindi il modello specificato nella connessione sarà utilizzata, anziché calcolando un modello utilizzando Code First. Se l'oggetto è un'istanza di un altro tipo, ad esempio, SqlConnection, quindi il contesto viene utilizzato per la modalità Code First.  
- È possibile passare un oggetto ObjectContext esistente a un costruttore DbContext per creare un oggetto DbContext ritorno a capo il contesto esistente. Può essere utilizzato per le applicazioni esistenti che utilizza ObjectContext ma che vuole sfruttare i vantaggi di DbContext in alcune parti dell'applicazione.  
