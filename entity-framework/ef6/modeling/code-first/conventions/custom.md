---
title: Codice personalizzato prima convenzioni - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
caps.latest.revision: 3
ms.openlocfilehash: 24d6f1bd5eb2ff8be59b9eedd1c4156709fa42fb
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121365"
---
# <a name="custom-code-first-conventions"></a>Convenzioni del primo codice personalizzato
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Quando si usa il modello viene calcolato da classi usando un set di convenzioni Code First. Il valore predefinito [convenzioni del primo codice](~/ef6/modeling/code-first/conventions/built-in.md) determinare aspetti, ad esempio quale proprietà diventa la chiave primaria di un'entità, il nome della tabella esegue il mapping a un'entità, e quali precisione e scala per impostazione predefinita contiene una colonna decimale.

A volte queste convenzioni predefinite non sono idonee per il modello e si hanno per colmarli configurando molte entità individuali usando le annotazioni dei dati o l'API Fluent. Convenzioni del primo codice personalizzati consentono di definire le convenzioni che forniscono i valori predefiniti della configurazione per il modello. In questa procedura dettagliata, verranno analizzati i diversi tipi di convenzioni personalizzate e come creare ciascuno di essi.


## <a name="model-based-conventions"></a>Convenzioni basato su modello

Questa pagina illustra l'API DbModelBuilder per le convenzioni personalizzate. Questa API dovrebbero essere sufficiente per la maggior parte delle convenzioni personalizzate di creazione. Tuttavia, è inoltre disponibile la possibilità di creare le convenzioni basato su modello - convenzioni che consentono di modificare il modello finale dopo averla creata, per gestire scenari avanzati. Per altre informazioni, vedere [basato su modello convenzioni](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Il nostro modello

Iniziamo con la definizione di un modello semplice che è possibile usare con la convenzioni. Aggiungere le classi seguenti al progetto.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Introduzione a convenzioni personalizzate

Scriviamo una convenzione che consente di configurare qualsiasi proprietà di chiave da utilizzare come chiave primaria per il tipo di entità denominata.

Le convenzioni sono abilitate nel generatore di modelli, è possibile accedere eseguendo l'override OnModelCreating nel contesto. Aggiornare la classe ProductContext come indicato di seguito:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

A questo punto, qualsiasi proprietà nel modello denominato chiave sarà configurata come chiave primaria di qualsiasi entità propria parte di.

Possiamo inoltre fare la convenzioni più specifica filtrando in base al tipo di proprietà che si intende configurare:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Si configurerà tutte le proprietà denominate chiave primaria chiave relative entità, ma solo se sono un numero intero.

Una funzionalità interessante del metodo IsKey è costituito da sommano tra loro. Ciò significa che se si chiama IsKey su più proprietà e tutti diventano parte di una chiave composta. Un'avvertenza per questo è che, quando si specificano più proprietà per una chiave è necessario specificare anche un ordine di tali proprietà. È possibile farlo tramite una chiamata di HasColumnOrder metodo simile al seguente:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Questo codice configurerà i tipi nel modello a dispone di una chiave composta costituita la colonna chiave int e la colonna di nome di stringa. Se si visualizza il modello nella finestra di progettazione si presenta come segue:

![compositeKey](~/ef6/media/compositekey.png)

Un altro esempio di convenzioni proprietà consiste nel configurare tutte le proprietà di data/ora nel mio modello per eseguire il mapping a un tipo datetime2 in SQL Server invece di data/ora. È possibile ottenere questo risultato con gli elementi seguenti:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Classi di convenzione

Un altro modo per definire le convenzioni consiste nell'utilizzare una convenzione di classe per incapsulare la convenzione. Quando si usa una classe convenzione è creare un tipo che eredita dalla classe convenzione System.Data.Entity.ModelConfiguration.Conventions nello spazio dei nomi.

È possibile creare una classe convenzione in base alla convenzione datetime2 che è stato illustrato in precedenza eseguendo queste operazioni:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Per indicare a Entity Framework per utilizzare questa convenzione che è aggiungerlo alla raccolta di convenzioni in OnModelCreating, che se sono state eseguite con la procedura dettagliata avrà un aspetto simile al seguente:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Come si può notare è aggiungere un'istanza di convenzione per l'insieme di convenzioni. Eredità dalla convenzione fornisce un modo pratico di raggruppamento e le convenzioni di condivisione tra progetti o team. Potrebbe, ad esempio, hai una libreria di classi con un set di convenzioni che tutte le organizzazioni che proietta di uso comune.

 

## <a name="custom-attributes"></a>Attributi personalizzati

Un altro ottimo uso convenzioni consiste nell'abilitare nuovi attributi da utilizzare quando si configura un modello. Per illustrare questo concetto, è possibile creare un attributo che è possibile usare per contrassegnare le proprietà della stringa come non Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

A questo punto, passiamo alla creazione di una convenzione per applicare questo attributo per il nostro modello:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Questa convenzione è possibile aggiungere l'attributo non Unicode a uno dei nostri proprietà stringa che indica che la colonna nel database verrà archiviata come varchar invece di nvarchar.

Un aspetto da notare circa questa convenzione è che se si inserisce l'attributo non Unicode in qualsiasi elemento diverso da una proprietà stringa, quindi verrà generata un'eccezione. Ciò avviene perché non è possibile configurare IsUnicode su qualsiasi tipo diverso da una stringa. In questo caso, successivamente sarà possibile effettuare la convenzione più specifica, in modo che filtri i dati che non è una stringa.

Quando la convenzione precedente funziona per la definizione di attributi personalizzati è disponibile un'altra API che possono essere molto più semplice da utilizzare, soprattutto quando si desidera utilizzare le proprietà dalla classe di attributi.

Per questo esempio si userà per aggiornare l'attributo e modificarlo in un attributo IsUnicode, in modo che risulti simile al seguente:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Dopo aver ottenuto ciò, è possibile impostare un valore booleano nell'attributo per indicare la convenzione o meno una proprietà deve essere Unicode. Possiamo creare nella convenzione di che abbiamo già accedendo ClrProperty della classe di configurazione simile al seguente:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Ciò è abbastanza semplice, ma c'è un modo più conciso di raggiungere questo obiettivo tramite la presenza di un metodo per le convenzioni di API. La presenza di metodo ha un parametro di tipo Func&lt;PropertyInfo, T&gt; PropertyInfo che accetta lo stesso come Where (metodo), ma è previsto per restituire un oggetto. Se l'oggetto restituito è null, quindi non verrà configurata la proprietà, ovvero è possibile filtrare le proprietà con esso come Where, ma è diverso in quanto verrà inoltre acquisire l'oggetto restituito e passarlo al metodo Configure. Questo codice funziona come segue:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Gli attributi personalizzati non sono l'unico motivo per usare la presenza di un metodo, è utile qualsiasi luogo in cui è necessario prendere in considerazione un elemento che si sta filtrando quando si configurano i tipi o la proprietà.

 

## <a name="configuring-types"></a>Configurazione dei tipi

Tutti i nostri convenzioni finora sono stati per le proprietà, ma è presente un'altra area delle convenzioni di API per la configurazione dei tipi nel modello. L'esperienza è simile alle convenzioni che abbiamo visto finora, ma le opzioni di configurazione all'interno dovrà essere eseguita all'entità anziché proprietà livello.

Una delle operazioni che possono essere molto utile per le convenzioni a livello di tipo in fase di modifica la tabella convenzione di denominazione, eseguire il mapping a uno schema esistente che è diverso da quello predefinito di Entity Framework o per creare un nuovo database con una convenzione di denominazione diversa. A tale scopo è necessario prima di tutto un metodo che può accettare le informazioni sul tipo per un tipo nel modello e restituire cosa deve essere il nome della tabella per quel tipo:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Questo metodo accetta un tipo e restituisce una stringa che usa lettere minuscole con caratteri di sottolineatura anziché CamelCase. Nel nostro modello ciò significa che la classe ProductCategory verrà mappata a una tabella denominata product\_categoria anziché ProductCategories.

Dopo aver ottenuto tale metodo poterlo chiamare una convenzione simile al seguente:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Questa convenzione consente di configurare tutti i tipi nel modello per eseguire il mapping al nome della tabella che viene restituito dal nostro metodo GetTableName. Questa convenzione è l'equivalente alla chiamata al metodo ToTable per ogni entità nel modello tramite l'API Fluent.

Un aspetto da sottolineare a proposito è che quando si chiama EF ToTable richiederà la stringa specificata dall'utente come nome della tabella esatto, senza la pluralizzazione che farebbe normalmente quando si determina i nomi di tabella. Ecco perché il nome della tabella dalla convenzione viene prodotto\_categoria anziché prodotto\_categorie. Afferma che la convenzione effettuando una chiamata al servizio di pluralizzazione noi stessi.

Nel codice seguente si userà il [risoluzione delle dipendenze](~/ef6/fundamentals/configuring/dependency-resolution.md) funzionalità aggiunta in EF6 per recuperare il servizio di pluralizzazione che EF sarebbe utilizzato e rendere plurali i nome della tabella.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> La versione generica di GetService è un metodo di estensione nello spazio dei nomi System.Data.Entity.Infrastructure.DependencyResolution, sarà necessario aggiungere un utilizzo dell'istruzione il contesto per usarlo.

### <a name="totable-and-inheritance"></a>ToTable ed ereditarietà

Un altro aspetto importante di ToTable è che se si esegue il mapping in modo esplicito un tipo per una determinata tabella, quindi è possibile modificare la strategia di mapping che useranno Entity Framework. Se si chiama ToTable per ogni tipo in una gerarchia di ereditarietà, passando il nome del tipo come il nome della tabella come in precedenza, quindi si modificherà la strategia di mapping tabella Per gerarchia (TPH) predefinita alla tabella Per tipo TPT (). Il modo migliore per descrivere questo è un esempio concreto con:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Per impostazione predefinita i dipendenti e manager viene eseguito il mapping alla stessa tabella (dipendenti) nel database. La tabella conterrà sia i dipendenti e ai Manager con una colonna discriminatore che indicherà il tipo di istanza viene memorizzato in ogni riga. Si tratta di mapping tabella per gerarchia in quanto non esiste una sola tabella per gerarchia. Tuttavia, se si chiama ToTable su entrambi classe quindi ogni tipo verrà invece eseguire il mapping alla relativa tabella, nota anche come TPT poiché ogni tipo ha un proprio tabella.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Il codice precedente eseguirà il mapping a una struttura di tabella che è simile a quanto segue:

![tptExample](~/ef6/media/tptexample.jpg)

È possibile evitare questo problema e gestire il mapping di tabella per gerarchia predefinita, in due modi:

1.  Chiamare ToTable con lo stesso nome di tabella per ogni tipo nella gerarchia.
2.  Chiamare ToTable solo sulla classe di base della gerarchia, nel nostro esempio che potrebbe essere dipendente.

 

## <a name="execution-order"></a>Ordine di esecuzione

Convenzioni di operano in modo wins ultimo quello utilizzato per l'API Fluent. Ciò significa che se si scrive due convenzioni che consentono di configurare la stessa opzione della stessa proprietà, quindi quello più recente per l'esecuzione wins. Ad esempio, nel codice seguente è impostata la lunghezza massima di tutte le stringhe fino a 500 ma configuriamo quindi tutte le proprietà denominate Name nel modello di avere una lunghezza massima pari a 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Poiché la convenzione per impostare della lunghezza massima di 250 è successiva a quella che consente di impostare tutte le stringhe fino a 500, tutte le proprietà denominate Name nel modello avrà una lunghezza massima di 250 durante qualsiasi altra stringa, ad esempio le descrizioni, è 500. L'uso di convenzioni in questo modo significa che è possibile fornire una convenzione generale per i tipi o le proprietà nel modello e quindi overide per subset diversi.

L'API Fluent e le annotazioni dei dati è anche utilizzabile per eseguire l'override di una convenzione in casi specifici. Nell'esempio riportato sopra se avessimo usato l'API Fluent per impostare la lunghezza massima di una proprietà quindi, sarebbe possibile avere inserirlo prima o dopo la convenzione, perché l'API Fluent più specifica avrà sempre la precedenza tramite la convenzione di configurazione più generale.

 

## <a name="built-in-conventions"></a>Convenzioni predefinite

Poiché le convenzioni personalizzate può esserne influenzate dalle convenzioni Code First predefiniti, può essere utile aggiungere convenzioni da eseguire prima o dopo un'altra convenzione. A tale scopo è possibile utilizzare i metodi AddBefore e AddAfter della raccolta convenzioni in DbContext derivata. Il codice seguente sarebbe aggiungere la classe convenzione creato in precedenza, in modo che venga eseguito prima incorporato nella convenzione di individuazione.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Si sta per essere più utile quando si aggiungono le convenzioni che dovranno essere eseguiti prima o dopo le convenzioni predefinite, un elenco delle convenzioni predefinite è disponibili qui: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

È anche possibile rimuovere le convenzioni che non si desidera applicate al modello. Per rimuovere una convenzione, usare il metodo Remove. Ecco un esempio di rimozione di PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
