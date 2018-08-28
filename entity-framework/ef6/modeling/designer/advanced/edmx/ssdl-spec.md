---
title: Specifica di SSDL - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995279"
---
# <a name="ssdl-specification"></a>Specifica SSDL
Store Schema Definition Language (SSDL) è un linguaggio basato su XML che descrive il modello di archiviazione di un'applicazione Entity Framework.

In un'applicazione Entity Framework, i metadati del modello di archiviazione viene caricato da un file con estensione ssdl, scritto in SSDL, in un'istanza del System.Data.Metadata.Edm.StoreItemCollection e sia accessibili tramite i metodi di Classe System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework Usa i metadati del modello di archiviazione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.

Entity Framework Designer (Entity Framework Designer) archivia le informazioni sul modello di archiviazione in un file con estensione edmx in fase di progettazione. In fase di compilazione Entity Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione SSDL che serve da Entity Framework in fase di esecuzione.

Le versioni di SSDL si differenziano tra loro per gli spazi dei nomi XML.

| Versione SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Elemento Association (SSDL)

Un' **Association** elemento schema store schema definition language (SSDL) consente di specificare le colonne di tabella che fanno parte di un vincolo di chiave esterna nel database sottostante. Due elementi End figlio obbligatorio specificano tabelle alle estremità dell'associazione e la molteplicità in ciascuna estremità. Elemento ReferentialConstraint facoltativo specifica le entità finali principale e dipendente dell'associazione, nonché le colonne coinvolte. Se nessun **ReferentialConstraint** è presente l'elemento, un elemento AssociationSetMapping deve essere usato per specificare i mapping delle colonne per l'associazione.

Il **Association** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   End (esattamente due elementi)
-   ReferentialConstraint (zero o più)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Association** elemento.

| Nome attributo | È obbligatorio | Valore                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Name**       | Yes         | Il nome del vincolo di chiave esterna corrispondente nel database sottostante. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a>Elemento AssociationSet (SSDL)

Il **AssociationSet** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna tra due tabelle nel database sottostante. Le colonne della tabella che fanno parte del vincolo di chiave esterna vengono specificate in un elemento di associazione. Il **associazione** elemento che corrisponde a un determinato **AssociationSet** viene specificato nell'elemento il **associazione** attributo del **AssociationSet**  elemento.

Set di associazioni SSDL viene eseguito il mapping al set di associazioni CSDL da un elemento AssociationSetMapping. Tuttavia, se è impostata l'associazione CSDL per una determinata associazione CSDL è definito con un elemento ReferentialConstraint, non corrispondente **AssociationSetMapping** elemento è necessario. In questo caso, se un' **AssociationSetMapping** l'elemento è presente, i mapping verranno sostituiti dalle **ReferentialConstraint** elemento.

Il **AssociationSet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   End (zero o due)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSet** elemento.

| Nome attributo  | È obbligatorio | Valore                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Name**        | Yes         | Nome del vincolo di chiave esterna rappresentato dal set di associazioni.                          |
| **Associazione** | Yes         | Nome dell'associazione che definisce le colonne che fanno parte del vincolo di chiave esterna. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **AssociationSet** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **AssociationSet** elemento che rappresenta il `FK_CustomerOrders` vincolo di chiave esterna nel database sottostante:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>Elemento CollectionType (SSDL)

Il **CollectionType** elemento di schema store schema definition language (SSDL) specifica che il tipo restituito della funzione è una raccolta. Il **CollectionType** elemento è figlio dell'elemento ReturnType. Il tipo di raccolta viene specificato usando l'elemento figlio di tipo di riga:

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrata una funzione che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a>Elemento CommandText (SSDL)

Il **CommandText** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che consente di definire un'istruzione SQL che viene eseguita a livello di database. Il **CommandText** elemento consente di aggiungere funzionalità simili a una stored procedure nel database, ma si definiscono le **CommandText** elemento nel modello di archiviazione.

Il **CommandText** elemento non può avere elementi figlio. Il corpo del **CommandText** elemento deve essere un'istruzione SQL valida per il database sottostante.

Attributi non sono applicabili per il **CommandText** elemento.

### <a name="example"></a>Esempio

L'esempio seguente mostra una **funzione** elemento con un elemento figlio **CommandText** elemento. Esporre il **UpdateProductInOrder** fungere da un metodo in ObjectContext importandolo nel modello concettuale.  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a>Elemento DefiningQuery (SSDL)

Il **DefiningQuery** elemento schema store schema definition language (SSDL) consente di eseguire un'istruzione SQL direttamente nel database sottostante. Il **DefiningQuery** elemento viene comunemente usato, ad esempio una vista di database, ma la vista viene definita nel modello di archiviazione anziché nel database. La visualizzazione definita in un **DefiningQuery** elementi possono essere mappati a un tipo di entità nel modello concettuale tramite un elemento EntitySetMapping. Questi mapping sono di sola lettura.  

La sintassi SSDL seguente illustra la dichiarazione di un **EntitySet** aggiungendo il **DefiningQuery** elemento contenente una query usata per recuperare la visualizzazione.

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

È possibile usare le stored procedure in Entity Framework per abilitare scenari di lettura / scrittura sulle visualizzazioni. È possibile utilizzare una vista origine dati o una visualizzazione Entity SQL come tabella di base per il recupero dei dati e per l'elaborazione tramite le stored procedure delle modifiche.

È possibile usare la **DefiningQuery** elemento destinazione Microsoft SQL Server Compact 3.5. Anche se SQL Server Compact 3.5 non supporta stored procedure, è possibile implementare una funzionalità simile con il **DefiningQuery** elemento. Un'altra situazione in cui può essere utile è nella creazione di stored procedure per risolvere una mancata corrispondenza tra i tipi di dati utilizzati nel linguaggio di programmazione e quelli dell'origine dati. È possibile scrivere un **DefiningQuery** che accetta un determinato set di parametri e quindi chiama una stored procedure con un diverso set di parametri, ad esempio, una stored procedure che elimina i dati.

## <a name="dependent-element-ssdl"></a>Elemento Dependent (SSDL)

Il **dipendenti** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'estremità dipendente del vincolo di chiave esterno (chiamato anche vincolo referenziale). Il **dipendenti** elemento specifica la colonna (o le colonne) in una tabella che fanno riferimento a una colonna chiave primaria (o colonne). **PropertyRef** elementi specificano quali colonne fanno riferimento. Il principale elemento specifica le colonne chiave primaria cui fanno riferimento le colonne specificate nel **dipendenti** elemento.

Il **dipendenti** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   PropertyRef (una o più)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **dipendenti** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ruolo**       | Yes         | Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **dipendenti** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo. Il **dipendenti** elemento specifica il **CustomerId** colonna del **ordine** tabella come entità finale dipendente del vincolo.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a>Elemento Documentation (SSDL)

Il **documentazione** elemento in schema store schema definition language (SSDL) può essere usato per fornire informazioni su un oggetto definito in un elemento padre.

Il **documentazione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   **Riepilogo**: una breve descrizione dell'elemento padre. Zero o un elemento.
-   **LongDescription**: descrizione Luna dell'elemento padre. Zero o un elemento.

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **documentazione** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra le **documentazione** elemento come elemento figlio di un elemento EntityType.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a>Elemento End (SSDL)

Il **End** elemento di schema store schema definition language (SSDL) specifica la tabella e il numero di righe in un'entità finale del vincolo di chiave esterno nel database sottostante. Il **End** elemento può essere un figlio dell'elemento di associazione o l'elemento AssociationSet. In entrambi i casi, gli elementi figlio possibili e gli attributi applicabili sono diversi.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento End come figlio dell'elemento Association

Un' **End** elemento (come figlio del **Association** elemento) specifica la tabella e il numero di righe alla fine di un vincolo di chiave esterna con il **tipo** e**Molteplicità** rispettivamente gli attributi. Le entità finali del vincolo di chiave esterna sono definite come parte dell'associazione SSDL. L'associazione SSDL deve disporre esattamente di due entità finali.

Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   OnDelete (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **associazione** elemento.

| Nome attributo   | È obbligatorio | Valore                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Yes         | Il nome completo del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.                                                                                                                                                                                                                                                                                          |
| **Ruolo**         | No          | Il valore della **ruolo** attributo nell'elemento principale o dipendente del corrispondente elemento ReferentialConstraint (se usato).                                                                                                                                                                                                                                             |
| **Molteplicità** | Yes         | **1**, **0..1**, o **\*** a seconda del numero di righe che possono essere alla fine del vincolo di chiave esterna. <br/> **1** indica esattamente una riga presente alla fine vincolo di chiave esterna. <br/> **0..1** indica che una o nessuna riga presente alla fine vincolo di chiave esterna. <br/> **\*** indica che esistano zero, uno o più righe alla fine vincolo di chiave esterna. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

#### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna. Il **molteplicità** i valori specificati in ogni **End** elemento indicano che molte righe nel **ordini** può essere associata a una riga nella tabella il **clienti**  tabella, ma solo una riga nella **clienti** può essere associata a una riga nella tabella il **ordini** tabella. Inoltre, il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella il **clienti** tabella verrà eliminata se la riga in il **clienti** tabella viene eliminata.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento End come figlio dell'elemento AssociationSet

Il **End** elemento (come figlio di **AssociationSet** elemento) specifica una tabella in un'entità finale del vincolo di chiave esterno nel database sottostante.

Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **AssociationSet** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Elemento EntitySet**  | Yes         | Il nome del set di entità SSDL che si trova alla fine del vincolo di chiave esterna.                                      |
| **Ruolo**       | No          | Il valore di uno dei **ruolo** gli attributi specificati in uno **End** elemento dell'elemento di associazione corrispondente. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

#### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento con un **AssociationSet** elemento con due **End** elementi:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a>Elemento EntityContainer (SSDL)

Un' **EntityContainer** elemento schema store schema definition language (SSDL) descrive la struttura dell'origine dati sottostante in un'applicazione Entity Framework: i set di entità SSDL (definiti negli elementi EntitySet) rappresentano le tabelle in un database, i tipi di entità SSDL (definiti negli elementi EntityType) rappresentano le righe in una tabella e set di associazioni (definiti negli elementi di AssociationSet) rappresentano vincoli di chiave esterna in un database. Un contenitore di entità modello di archiviazione viene eseguito il mapping a un contenitore di entità del modello concettuale attraverso l'elemento EntityContainerMapping.

Un' **EntityContainer** elemento può avere zero o più elementi di documentazione. Se un **documentazione** elemento è presente, deve precedere tutti gli elementi figlio.

Un' **EntityContainer** elemento può includere zero o più degli elementi figlio seguenti (nell'ordine riportato):

-   EntitySet
-   AssociationSet
-   Elementi Annotation

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityContainer** elemento.

| Nome attributo | È obbligatorio | Valore                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del contenitore di entità. Il nome non può contenere caratteri punto (.). |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityContainer** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento che definisce due set di entità e un set di associazioni. I nomi dei tipi di entità e associazione sono qualificati dal nome dello spazio dei nomi del modello concettuale.

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a>Elemento EntitySet (SSDL)

Un' **EntitySet** elemento schema store schema definition language (SSDL) rappresenta una tabella o vista nel database sottostante. Un elemento EntityType nel linguaggio SSDL rappresenta una riga nella tabella o della vista. Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL. Il mapping tra un set di entità CSDL e un set di entità SSDL viene specificato in un elemento EntitySetMapping.

Il **EntitySet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   DefiningQuery (zero o un elemento)
-   Elementi Annotation

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySet** elemento.

> [!NOTE]
> Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias. Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.

| Nome attributo | È obbligatorio | Valore                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del set di entità.                                                              |
| **Elemento EntityType** | Yes         | Nome completo del tipo di entità per il quale il set di entità contiene delle istanze. |
| **schema**     | No          | Schema del database.                                                                     |
| **Tabella**      | No          | Tabella del database.                                                                      |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntitySet** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento che dispone di due **EntitySet** elementi e uno **AssociationSet** elemento:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a>Elemento EntityType (SSDL)

Un' **EntityType** elemento schema store schema definition language (SSDL) rappresenta una riga in una tabella o vista del database sottostante. Un elemento EntitySet in SSDL rappresenta la tabella o vista in cui si trova la riga. Il **EntityType** attributo di un **EntitySet** elemento specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL. Il mapping tra un tipo di entità SSDL e un tipo di entità CSDL viene specificato in un elemento EntityTypeMapping.

Il **EntityType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Chiave (zero o un elemento)
-   Elementi Annotation

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityType** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del tipo di entità. Questo valore corrisponde generalmente al nome della tabella in cui il tipo di entità rappresenta una riga. Questo valore non può contenere punti (.). |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con due proprietà:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a>Elemento Function (SSDL)

Il **funzione** elemento di schema store schema definition language (SSDL) specifica una stored procedure esistente nel database sottostante.

Il **funzione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Parametro (zero o più)
-   CommandText (zero o più)
-   ReturnType (zero o più)
-   Elementi Annotation (zero o più)

Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** elemento o il **ReturnType** attributo (vedere sotto), ma non entrambi.

È possibile importare stored procedure specificate nel modello di archiviazione nel modello concettuale di un'applicazione. Per altre informazioni, vedere [esecuzione di query con Stored procedure](~/ef6/modeling/designer/stored-procedures/query.md). Il **funzione** elemento può essere usato anche per definire funzioni personalizzate nel modello di archiviazione.  

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **funzione** elemento.

> [!NOTE]
> Alcuni attributi (non elencati qui) possono essere qualificati con il **archiviare** alias. Questi attributi vengono utilizzati per la procedura guidata Aggiorna modello quando si aggiorna un modello.

| Nome attributo             | È obbligatorio | Valore                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                   | Yes         | Nome della stored procedure.                                                                                                                                                                                  |
| **ReturnType**             | No          | Tipo restituito della stored procedure.                                                                                                                                                                           |
| **Aggregate**              | No          | **True** se la stored procedure restituisce un valore di aggregazione; in caso contrario **False**.                                                                                                                                  |
| **BuiltIn**                | No          | **True** se la funzione è un oggetto incorporato<sup>1</sup> funzione; in caso contrario **False**.                                                                                                                                  |
| **StoreFunctionName**      | No          | Nome della stored procedure.                                                                                                                                                                                  |
| **NiladicFunction**        | No          | **True** se la funzione è senza parametri<sup>2</sup> funzione; **False** in caso contrario.                                                                                                                                   |
| **IsComposable**           | No          | **True** se la funzione è componibile<sup>3</sup> funzione; **False** in caso contrario.                                                                                                                                |
| **ParameterTypeSemantics** | No          | Enumerazione che definisce la semantica dei tipi utilizzata per risolvere gli overload della funzione. L'enumerazione è definita nel file manifesto del provider per ogni definizione di funzione. Il valore predefinito è **AllowImplicitConversion**. |
| **schema**                 | No          | Nome dello schema in cui viene definita la stored procedure.                                                                                                                                                   |

<sup>1</sup> una funzione predefinita è una funzione definita nel database. Per informazioni sulle funzioni definite nel modello di archiviazione, vedere l'elemento CommandText (SSDL).

<sup>2</sup> una funzione senza parametri è una funzione che non accetta parametri e, quando viene chiamato, non richiede le parentesi.

<sup>3</sup> due funzioni sono componibili se l'output di una funzione può essere l'input per l'altra funzione.

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **funzione** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

Nell'esempio seguente un **funzione** elemento che corrisponde al **UpdateOrderQuantity** stored procedure. La stored procedure accetta due parametri e non restituisce alcun valore.

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a>Elemento Key (SSDL)

Il **chiave** elemento schema store schema definition language (SSDL) rappresenta la chiave primaria di una tabella nel database sottostante. **Chiave** è un elemento figlio di un elemento EntityType che rappresenta una riga in una tabella. La chiave primaria è definita nel **Key** elemento facendo riferimento a uno o più elementi di proprietà definite nel **EntityType** elemento.

Il **chiave** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   PropertyRef (una o più)
-   Elementi Annotation

Attributi non sono applicabili per il **chiave** elemento.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con una chiave che fa riferimento a una proprietà:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a>Elemento OnDelete (SSDL)

Il **OnDelete** elemento schema store schema definition language (SSDL) riflette il comportamento del database quando viene eliminata una riga che fa parte di un vincolo di chiave esterna. Se l'azione viene impostata su **Cascade**, quindi verranno eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione. Se l'azione viene impostata su **None**, quindi non vengono eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione. Un' **OnDelete** è un elemento figlio dell'elemento finale.

Un' **OnDelete** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **OnDelete** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Azione**     | Yes         | **CASCADE** oppure **None**. (Il valore **Restricted** è valido ma presenta lo stesso comportamento **None**.) |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **OnDelete** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che definisce il **FK\_CustomerOrders** vincolo di chiave esterna. Il **OnDelete** elemento indica che tutte le righe nel **ordini** che fanno riferimento a una determinata riga nella tabella di **clienti** tabella verrà eliminata se la riga nella finestra di **Clienti** tabella viene eliminata.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a>Elemento Parameter (SSDL)

Il **parametro** elemento schema store schema definition language (SSDL) è un figlio dell'elemento di funzione che specifica i parametri per una stored procedure nel database.

Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del parametro.                                                                                                                                                                                                      |
| **Type**       | Yes         | Tipo del parametro.                                                                                                                                                                                                             |
| **Modalità**       | No          | **Nelle**, **Out**, o **InOut** a seconda che il parametro sia un input, output o parametro di input/output.                                                                                                                |
| **MaxLength**  | No          | Lunghezza massima del parametro.                                                                                                                                                                                            |
| **Precisione**  | No          | Precisione del parametro.                                                                                                                                                                                                 |
| **Scala**      | No          | Scala del parametro.                                                                                                                                                                                                     |
| **SRID**       | No          | Identificatore di riferimento spaziale del sistema. Valido solo per i parametri di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra una **funzione** elemento che dispone di due **parametro** gli elementi che specificano i parametri di input:

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a>Elemento Principal (SSDL)

Il **Principal** in schema store schema definition language (SSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'entità finale del vincolo di chiave esterno (chiamato anche vincolo referenziale). Il **Principal** elemento specifica la colonna chiave primaria (o le colonne) in una tabella cui viene fatto riferimento da un'altra colonna (o colonne). **PropertyRef** elementi specificano quali colonne fanno riferimento. Elemento Dependent specifica le colonne che fanno riferimento le colonne chiave primaria specificati nel **Principal** elemento.

Il **Principal** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   PropertyRef (una o più)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Principal** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ruolo**       | Yes         | Lo stesso valore di **ruolo** (se utilizzato) dell'attributo dell'elemento End corrispondente; in caso contrario, il nome della tabella contenente la colonna di riferimento. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Principal** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente illustra un elemento di associazione che utilizza un **ReferentialConstraint** elemento per specificare le colonne che partecipano le **FK\_CustomerOrders** chiave esterna vincolo. Il **dell'entità** elemento specifica il **CustomerId** colonna del **cliente** tabella come entità finale principale del vincolo.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a>Elemento Property (SSDL)

Il **proprietà** elemento schema store schema definition language (SSDL) rappresenta una colonna in una tabella nel database sottostante. **Proprietà** elementi sono elementi figlio degli elementi EntityType, che rappresentano le righe in una tabella. Ciascuna **proprietà** elemento definito in un **EntityType** elemento rappresenta una colonna.

Oggetto **proprietà** elemento non può avere elementi figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                  | Yes         | Nome della colonna corrispondente.                                                                                                                                                                                           |
| **Type**                  | Yes         | Tipo della colonna corrispondente.                                                                                                                                                                                           |
| **Ammette valori null**              | No          | **True** (valore predefinito) o **False** a seconda del fatto che la colonna corrispondente può avere un valore null.                                                                                                                  |
| **DefaultValue**          | No          | Valore predefinito della colonna corrispondente.                                                                                                                                                                                  |
| **MaxLength**             | No          | Lunghezza massima della colonna corrispondente.                                                                                                                                                                                 |
| **FixedLength**           | No          | **True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come una stringa di lunghezza fissa.                                                                                                              |
| **Precisione**             | No          | Precisione della colonna corrispondente.                                                                                                                                                                                      |
| **Scala**                 | No          | Scala della colonna corrispondente.                                                                                                                                                                                          |
| **Unicode**               | No          | **True** oppure **False** a seconda se il valore della colonna corrispondente essere archiviato come stringa Unicode.                                                                                                                   |
| **regole di confronto**             | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                   |
| **SRID**                  | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | No          | **None**, **Identity** (se il valore della colonna corrispondente è un'identità generata nel database) oppure **calcolata** (se il valore della colonna corrispondente viene calcolato nel database). Non valido per le proprietà di tipo di riga. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con l'elemento figlio due **proprietà** elementi:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a>Elemento PropertyRef (SSDL)

Il **PropertyRef** elemento schema store schema definition language (SSDL) fa riferimento a una proprietà definita in un elemento EntityType per indicare che la proprietà eseguirà uno dei seguenti ruoli:

-   Far parte della chiave primaria della tabella che la **EntityType** rappresenta. Uno o più **PropertyRef** elementi possono essere utilizzati per definire una chiave primaria. Per altre informazioni, vedere l'elemento Key.
-   Rappresenterà l'entità finale dipendente o principale di un vincolo referenziale. Per altre informazioni, vedere ReferentialConstraint (elemento).

Il **PropertyRef** elemento può avere solo elementi figlio seguenti:

-   Documentazione (zero o un elemento)
-   Elementi Annotation

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **PropertyRef** elemento.

| Nome attributo | È obbligatorio | Valore                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Yes         | Nome della proprietà alla quale viene fatto riferimento. |

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **PropertyRef** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra una **PropertyRef** elemento usato per definire una chiave primaria facendo riferimento a una proprietà definita in un **EntityType** elemento.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a>Elemento ReferentialConstraint (SSDL)

Il **ReferentialConstraint** elemento schema store schema definition language (SSDL) rappresenta un vincolo di chiave esterna (chiamato anche vincolo referenziale) nel database sottostante. Le entità finali principale e dipendente del vincolo vengono specificate dagli elementi figlio principale e dipendente, rispettivamente. Colonne che partecipano all'estremità principale e dipendente vengono fatto riferimento con gli elementi PropertyRef.

Il **ReferentialConstraint** è un elemento figlio facoltativo dell'elemento Association. Se un **ReferentialConstraint** elemento non viene usato per eseguire il mapping il vincolo foreign key specificato nella **Association** elemento, un AssociationSetMapping elemento deve essere usato per eseguire questa operazione.

Il **ReferentialConstraint** elemento può avere elementi figlio seguenti:

-   Documentazione (zero o un elemento)
-   Entità (esattamente un elemento)
-   Dipendente (esattamente un elemento)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReferentialConstraint** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che utilizza un **ReferentialConstraint** elemento per specificare le colonne che fanno parte del **FK\_CustomerOrders**  vincolo di chiave esterna:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a>Elemento ReturnType (SSDL)

Il **ReturnType** elemento di schema store schema definition language (SSDL) specifica il tipo restituito per una funzione che viene definito in un **funzione** elemento. Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.

Il tipo restituito di una funzione viene specificato con il **tipo** attributo o il **ReturnType** elemento.

Il **ReturnType** elemento può avere elementi figlio seguenti:

- Elemento CollectionType (uno)  

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente usa un' **funzione** che restituisce una raccolta di righe.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a>Elemento RowType (SSDL)

Oggetto **RowType** elemento schema store schema definition language (SSDL) definisce una struttura senza nome come reso tipo per una funzione definita nell'archivio.

Oggetto **RowType** è l'elemento figlio di **CollectionType** elemento:

Oggetto **RowType** elemento può avere elementi figlio seguenti:

- Proprietà (una o più)  

### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrata una funzione di archivio che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a>Elemento Schema (SSDL)

Il **Schema** elemento schema store schema definition language (SSDL) è l'elemento radice di una definizione di modello di archiviazione. Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello di archiviazione.

Il **Schema** elemento può contenere zero o più degli elementi figlio seguenti:

-   Associazione
-   EntityType
-   EntityContainer
-   Funzione

Il **Schema** elemento utilizza le **Namespace** attributo per definire lo spazio dei nomi per gli oggetti di tipo e associazione di entità in un modello di archiviazione. All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.

Uno spazio dei nomi del modello di archiviazione è diverso dallo spazio dei nomi XML del **Schema** elemento. Uno spazio dei nomi del modello di archiviazione (come definito per il **Namespace** attributi) è un contenitore logico per tipi di entità e tipi di associazione. Lo spazio dei nomi XML (indicato dal **xmlns** attributo) di un **Schema** elemento è lo spazio dei nomi predefinito per gli elementi figlio e attributi del **Schema** elemento. Spazi dei nomi XML nel formato http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a SSDL. Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Schema** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spazio dei nomi**             | Yes         | Spazio dei nomi del modello di archiviazione. Il valore della **Namespace** viene usato in modo da formare il nome completo di un tipo. Ad esempio, se un **EntityType** denominata *cliente* è lo spazio dei nomi examplemodel. Store, quindi specificare il nome completo del **EntityType** è Examplemodel. <br/> Le stringhe seguenti non possono essere usate come valore per il **Namespace** attributo: **System**, **temporaneo**, oppure **Edm**. Il valore per il **Namespace** attributo non può essere identico al valore per il **Namespace** attributo nell'elemento Schema CSDL. |
| **Alias**                 | No          | Identificatore utilizzato al posto del nome dello spazio dei nomi. Ad esempio, se un **EntityType** denominata *Customer* è nello spazio dei nomi examplemodel. Store e il valore del **Alias** attributo è *StorageModel*, è possibile utilizzare storagemodel. Customer come nome completo del **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Provider**              | Yes         | Provider di dati.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Yes         | Token che indica al provider quale manifesto del provider restituire. Non è definito alcun formato per il token. I valori per il token sono definiti dal provider. Per informazioni sui token del manifesto del provider SQL Server, vedere SqlClient per Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Esempio

L'esempio seguente mostra una **Schema** elemento che contiene un' **EntityContainer** elemento, due **EntityType** elementi e uno **associazione** elemento.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a>Attributi di annotazione

Gli attributi di annotazione in Store Schema Definition Language (SSDL) sono attributi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sugli elementi del modello di archiviazione. Oltre ad avere una struttura XML valida, agli attributi di annotazione si applicano i vincoli seguenti:

-   Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a SSDL.
-   I nomi completi di due attributi di annotazione non devono essere uguali.

È possibile applicare più attributi di annotazione a un determinato elemento SSDL. Metadati contenuti negli elementi di annotazione sono accessibili in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.

### <a name="example"></a>Esempio

L'esempio seguente illustra un elemento EntityType che presenta un attributo di annotazione applicato per il **OrderId** proprietà. L'esempio illustra anche un elemento di annotazione aggiunto per il **EntityType** elemento.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a>Elementi Annotation (SSDL)

Gli elementi Annotation in SSDL (Store Schema Definition Language) sono elementi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sul modello di archiviazione. Oltre ad avere una struttura XML valida, agli elementi Annotation si applicano i vincoli seguenti:

-   Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a SSDL.
-   I nomi completi di due elementi Annotation non devono essere uguali.
-   Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento SSDL.

Più elementi Annotation possono essere figli di un dato elemento SSDL. A partire da .NET Framework versione 4, i metadati contenuti in elementi annotation sono accessibile in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.

### <a name="example"></a>Esempio

L'esempio seguente illustra un elemento EntityType contenente un elemento annotation (**CustomElement**). L'esempio mostra anche un attributo di annotazione applicato per il **OrderId** proprietà.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a>Facet (SSDL)

I facet in schema store schema definition language (SSDL) rappresentano vincoli sui tipi di colonna sono specificati negli elementi proprietà. I facet vengono implementati come attributi XML sul **proprietà** elementi.

Nella tabella seguente vengono descritti i facet supportati in SSDL:

| Facet           | Descrizione                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **regole di confronto**   | Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.                                                                                                             |
| **FixedLength** | Specifica se la lunghezza del valore della colonna può variare.                                                                                                                                                                                                  |
| **MaxLength**   | Specifica la lunghezza massima del valore della colonna.                                                                                                                                                                                                           |
| **Precisione**   | Per le proprietà di tipo **decimale**, specifica il numero di cifre può avere un valore della proprietà. Per le proprietà di tipo **tempo**, **DateTime**, e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria di secondi del valore della colonna. |
| **Scala**       | Specifica il numero di cifre a destra del separatore decimale per il valore della colonna.                                                                                                                                                                      |
| **Unicode**     | Indica se il valore della colonna viene archiviato come Unicode.                                                                                                                                                                                                    |
