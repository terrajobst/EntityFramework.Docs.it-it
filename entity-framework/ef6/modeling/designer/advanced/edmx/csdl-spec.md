---
title: Specifica di CSDL - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: f5bf0dc75a8195e9af979c9e044f36171f46c9b7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490519"
---
# <a name="csdl-specification"></a>Specifica CSDL
Conceptual Schema Definition Language (CSDL) è un linguaggio basato su XML che descrive le entità, le relazioni e le funzioni che costituiscono un modello concettuale di un'applicazione basata sui dati. Questo modello concettuale può essere usato da Entity Framework o WCF Data Services. I metadati descritti con CSDL vengono utilizzati da Entity Framework per eseguire il mapping di entità e relazioni definite in un modello concettuale a un'origine dati. Per altre informazioni, vedere [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) e [specifica MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL è l'implementazione di Entity Framework di Entity Data Model.

In un'applicazione Entity Framework, i metadati del modello concettuale viene caricato da un file con estensione CSDL, scritto in CSDL, in un'istanza del System.Data.Metadata.Edm.EdmItemCollection e sia accessibili tramite i metodi di Classe System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework Usa i metadati del modello concettuale per tradurre le query sul modello concettuale in comandi specifici dell'origine dati.

Entity Framework Designer archivia informazioni sul modello concettuale in un file con estensione edmx in fase di progettazione. In fase di compilazione, Entity Framework Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione CSDL che serve da Entity Framework in fase di esecuzione.

Le versioni di CSDL si differenziano tra loro per gli spazi dei nomi XML.

| Versione CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Elemento Association (CSDL)

Un' **Association** elemento definisce una relazione tra due tipi di entità. Un'associazione deve specificare i tipi di entità coinvolti nella relazione e il possibile numero di tipi di entità a ogni entità finale della relazione, che è noto come molteplicità. La molteplicità di un'entità finale dell'associazione può avere un valore di uno (1), zero o uno (0..1) o molti (\*). Queste informazioni vengono specificate in due elementi End figlio.

Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.

In un'applicazione, un'istanza di un'associazione rappresenta un'associazione specifica tra istanze di tipi di entità. Le istanze dell'associazione sono raggruppate logicamente in un set di associazioni.

Un' **Association** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   End (esattamente 2 elementi)
-   ReferentialConstraint (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Association** elemento.

| Nome attributo | È obbligatorio | Valore                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | Yes         | Nome dell'associazione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** associazione quando le chiavi esterne non sono state esposte sul **cliente** e  **Ordine** i tipi di entità. Il **molteplicità** i valori per ogni **finali** dell'associazione indicano che molte **ordini** può essere associato un **cliente**, ma un solo **cliente** può essere associato un **ordine**. Inoltre, il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e sono stati caricati in ObjectContext verrà eliminato Se il **cliente** viene eliminato.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** associazione quando le chiavi esterne sono state esposte sul **cliente** e  **Ordine** i tipi di entità. Con le chiavi esterne esposte, la relazione tra l'entità viene gestita con un **ReferentialConstraint** elemento. Un elemento AssociationSetMapping corrispondente non è necessario eseguire il mapping di questa associazione all'origine dati.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>Elemento AssociationSet (CSDL)

Il **AssociationSet** elemento schema conceptual schema definition Language (CSDL) è un contenitore logico per le istanze dell'associazione dello stesso tipo. Un set di associazioni fornisce una definizione per il raggruppamento di istanze di associazioni in modo che possano essere mappate a un'origine dati.  

Il **AssociationSet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento consentito)
-   End (esattamente due elementi richiesti)
-   Elementi Annotation (zero o più elementi consentiti)

Il **Association** attributo specifica il tipo di associazione che contiene un set di associazioni. I set di entità che costituiscono le entità finali di un set di associazioni vengono specificati con esattamente due figlio **End** elementi.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSet** elemento.

| Nome attributo  | È obbligatorio | Valore                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | Yes         | Nome del set di entità. Il valore del **Name** non può essere identico al valore dell'attributo il **Association** attributo.                                 |
| **Associazione** | Yes         | Il nome completo dell'associazione le cui istanze sono contenute nel set di associazioni. L'associazione deve essere nello stesso spazio dei nomi del set di associazioni. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **AssociationSet** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento con due **AssociationSet** elementi:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>Elemento CollectionType (CSDL)

Il **CollectionType** elemento schema conceptual schema definition Language (CSDL) specifica che una funzione o un parametro il tipo restituito è una raccolta. Il **CollectionType** elemento può essere un figlio dell'elemento di parametro o l'elemento ReturnType (funzione). Il tipo di raccolta può essere specificato usando il **tipo** attributo o uno degli elementi figlio seguenti:

-   **Elemento CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Un modello non eseguirà la convalida se viene specificato il tipo di una raccolta con entrambe le **tipo** attributo e un elemento figlio.

 

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **CollectionType** elemento. Si noti che il **DefaultValue**, **MaxLength**, **FixedLength**, **precisione**, **scalabilità**,  **Unicode**, e **regole di confronto** gli attributi sono applicabili solo alle raccolte di **semplici EDM**.

| Nome attributo                                                          | È obbligatorio | Valore                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | No          | Tipo della raccolta.                                                                                                                                                                                                      |
| **Ammette valori null**                                                            | No          | **True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null. <br/> [!NOTE]                                                                                                                 |
| > Nella versione 1 di CSDL, una proprietà di tipo complesso deve avere `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | No          | Valore predefinito della proprietà.                                                                                                                                                                                               |
| **MaxLength**                                                           | No          | Lunghezza massima del valore della proprietà.                                                                                                                                                                                        |
| **FixedLength**                                                         | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.                                                                                                                           |
| **Precisione**                                                           | No          | Precisione del valore della proprietà.                                                                                                                                                                                             |
| **Scala**                                                               | No          | Scala del valore della proprietà.                                                                                                                                                                                                 |
| **SRID**                                                                | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali.   Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.                                                                                                                                |
| **regole di confronto**                                                           | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                    |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente illustra una funzione definita dal modello che utilizza un **CollectionType** elemento per specificare che la funzione restituisce una raccolta di **persona** i tipi di entità (come specificato con il **ElementType** attributo).

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

L'esempio seguente illustra una funzione definita dal modello che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

L'esempio seguente illustra una funzione definita dal modello che utilizza il **CollectionType** elemento per specificare che la funzione accetta come parametro una raccolta di **reparto** i tipi di entità.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>Elemento ComplexType (CSDL)

Oggetto **ComplexType** elemento definisce una struttura di dati composta **EdmSimpleType** proprietà o altri tipi complessi.  Un tipo complesso può essere una proprietà di un tipo di entità o un altro tipo complesso. Un tipo complesso è simile a un tipo di entità in quanto il tipo complesso definisce i dati. Tuttavia esistono alcune differenze importanti tra i tipi complessi e i tipi di entità:

-   I tipi complessi non dispongono di identità o chiavi e pertanto non possono esistere indipendentemente. I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.
-   I tipi complessi non possono partecipare nelle associazioni. Né finale di un'associazione può essere un tipo complesso, e pertanto non è possibile definire le proprietà di navigazione per i tipi complessi.
-   Una proprietà di tipo complesso non può avere un valore null, sebbene ogni proprietà scalare di un tipo complesso possa essere impostata su Null.

Oggetto **ComplexType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Proprietà (zero o più elementi)
-   Elementi Annotation (zero o più elementi)

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ComplexType** elemento.

| Nome attributo                                                                                                 | È obbligatorio | Valore                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nome                                                                                                           | Yes         | Nome del tipo complesso. Il nome di un tipo complesso non può essere uguale a quello di un altro tipo complesso, di un tipo di entità o di un'associazione che si trova entro l'ambito del modello. |
| BaseType                                                                                                       | No          | Nome di un altro tipo complesso che è il tipo di base del tipo complesso definito. <br/> [!NOTE]                                                                     |
| > Questo attributo non è applicabile nella versione 1 CSDL. L'ereditarietà per i tipi complessi non è supportata in quella versione. |             |                                                                                                                                                                                     |
| Classe astratta                                                                                                       | No          | **True** oppure **False** (valore predefinito) a seconda se il tipo complesso è un tipo astratto. <br/> [!NOTE]                                                                  |
| > Questo attributo non è applicabile nella versione 1 CSDL. I tipi complessi in quella versione non possono essere tipi astratti.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ComplexType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un tipo complesso, **indirizzi**, con la **EdmSimpleType** delle proprietà **StreetAddress**, **Città**,  **StateOrProvince**, **paese**, e **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Per definire il tipo complesso **indirizzo** (vedere sopra) come una proprietà di un tipo di entità, è necessario dichiarare il tipo della proprietà nella definizione del tipo di entità. L'esempio seguente mostra le **indirizzi** proprietà come un tipo complesso di un tipo di entità (**server di pubblicazione**):

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>Elemento DefiningExpression (CSDL)

Il **DefiningExpression** elemento schema conceptual schema definition Language (CSDL) contiene un'espressione Entity SQL che definisce una funzione nel modello concettuale.  

> [!NOTE]
> Ai fini della convalida, un **DefiningExpression** elemento può contenere contenuto arbitrario. Tuttavia, Entity Framework genererà un'eccezione in fase di esecuzione se un **DefiningExpression** elemento neobsahuje Entity SQL valido.

 

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **DefiningExpression** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente usa un' **DefiningExpression** elemento per definire una funzione che restituisce il numero di anni, poiché è stato pubblicato un libro. Il contenuto di **DefiningExpression** elemento viene scritto in Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Elemento Dependent (CSDL)

Il **dipendenti** conceptual schema definition Language (CSDL) è un elemento figlio all'elemento ReferentialConstraint e definisce l'entità finale dipendente di un vincolo referenziale. Oggetto **ReferentialConstraint** elemento definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale. Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità. Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo. Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo. **PropertyRef** gli elementi vengono usati per specificare quali chiavi fanno riferimento l'entità finale principale.

Il **dipendenti** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   PropertyRef (uno o più elementi)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **dipendenti** elemento.

| Nome attributo | È obbligatorio | Valore                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Ruolo**       | Yes         | Nome del tipo di entità in un'entità finale dipendente dell'associazione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **dipendenti** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra una **ReferentialConstraint** elemento utilizzato come parte della definizione del **PublishedBy** association. Il **PublisherId** proprietà delle **libro** tipo di entità costituisce l'entità finale dipendente del vincolo referenziale.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Elemento Documentation (CSDL)

Il **documentazione** elemento schema conceptual schema definition Language (CSDL) può essere usato per fornire informazioni su un oggetto definito in un elemento padre. In un file con estensione edmx, quando la **documentazione** elemento è figlio di un elemento che appaia come un oggetto nell'area di progettazione della finestra di progettazione di Entity Framework (ad esempio un'entità, associazioni o proprietà), il contenuto del **documentazione**  elemento verrà visualizzato in Visual Studio **proprietà** finestra per l'oggetto.

Il **documentazione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   **Riepilogo**: una breve descrizione dell'elemento padre. Zero o un elemento.
-   **LongDescription**: descrizione Luna dell'elemento padre. Zero o un elemento.
-   Elementi di annotazione. Zero o più elementi.

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **documentazione** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra le **documentazione** elemento come elemento figlio di un elemento EntityType. Se il frammento di codice seguente nel file CSDL del contenuto di un'estensione edmx file, il contenuto del **Summary** e **LongDescription** elementi apparirebbe in Visual Studio **proprietà** finestra dei comandi quando si fa clic su di `Customer` tipo di entità.

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>Elemento End (CSDL)

Il **End** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento di associazione o l'elemento AssociationSet. In ogni caso, il ruolo del **End** elemento è diverso e i relativi attributi applicabili sono diversi.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento End come figlio dell'elemento Association

Un' **finali** elemento (come figlio del **Association** elemento) identifica il tipo di entità in un'estremità di un'associazione e il numero di istanze del tipo di entità che possono essere presenti in tale entità finale di un'associazione. Le entità finali dell'associazione sono definite come parte di un'associazione; un'associazione deve disporre esattamente di due entità finali. Le istanze del tipo di entità in corrispondenza di un'entità finale di un'associazione sono accessibili attraverso proprietà di navigazione o chiavi esterne se sono esposte in un tipo di entità.  

Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   OnDelete (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **associazione** elemento.

| Nome attributo   | È obbligatorio | Valore                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Yes         | Nome del tipo di entità in una entità finale dell'associazione.                                                                                                                                                                                                                                                                                                                                                         |
| **Ruolo**         | No          | Nome per l'entità finale dell'associazione. Se non è fornito alcun nome, verrà utilizzato il nome del tipo di entità nell'entità finale dell'associazione.                                                                                                                                                                                                                                                                                           |
| **Molteplicità** | Yes         | **1**, **0..1**, o **\*** a seconda del numero di istanze del tipo di entità che possono essere alla fine dell'associazione. <br/> **1** indica tale istanza del tipo esattamente un'entità è presente l'estremità dell'associazione. <br/> **0..1** indica che all'entità finale dell'associazione sono presenti zero o più istanze del tipo di entità. <br/> **\*** indica che da zero, uno o più istanze del tipo di entità sono presenti entità finale dell'associazione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** association. Il **molteplicità** i valori per ogni **finali** dell'associazione indicano che molte **ordini** può essere associato un **cliente**, ma un solo **cliente** può essere associato un **ordine**. Inoltre, il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e che sono stati caricati in ObjectContext sarà Se eliminato il **cliente** viene eliminato.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento End come figlio dell'elemento AssociationSet

Il **End** elemento specifica un'entità finale di un set di associazioni. Il **AssociationSet** l'elemento deve contenere due **End** elementi. Le informazioni contenute in un' **End** elemento viene usato per eseguire il mapping di un set di associazioni a un'origine dati.

Un' **End** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

> [!NOTE]
> Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio. Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.

 

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **finali** elemento quando è figlio di un **AssociationSet** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Elemento EntitySet**  | Yes         | Il nome del **EntitySet** elemento che definisce un'entità finale dell'elemento padre **AssociationSet** elemento. Il **EntitySet** elemento deve essere definito nello stesso contenitore di entità come elemento padre **AssociationSet** elemento. |
| **Ruolo**       | No          | Nome dell'entità finale del set di associazioni. Se il **ruolo** attributo non viene utilizzato, il nome del set di entità finale dell'associazione sarà il nome del set di entità.                                                                   |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **End** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento con due **AssociationSet** elementi, ognuno con due **End** elementi:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>Elemento EntityContainer (CSDL)

Il **EntityContainer** elemento schema conceptual schema definition Language (CSDL) è un contenitore logico per set di entità, set di associazioni e importazioni di funzioni. Un contenitore di entità del modello concettuale viene mappato a un contenitore di entità modello di archiviazione tramite l'elemento EntityContainerMapping. Un contenitore di entità del modello di archiviazione descrive la struttura del database: i set di entità descrivono le tabelle, i set di associazioni descrivono i vincoli delle chiavi esterne e le importazioni di funzioni descrivono le stored procedure in un database.

Un' **EntityContainer** elemento può avere zero o più elementi di documentazione. Se un **documentazione** l'elemento è presente, deve precedere tutti **EntitySet**, **AssociationSet**, e **FunctionImport** elementi.

Un' **EntityContainer** elemento può includere zero o più degli elementi figlio seguenti (nell'ordine riportato):

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Elementi Annotation

È possibile estendere un **EntityContainer** per includere il contenuto di un altro elemento **EntityContainer** che si trova all'interno dello stesso spazio dei nomi. Per includere il contenuto di un altro **EntityContainer**, il tipo di riferimento **EntityContainer** elemento, impostare il valore della **Extends** attributo sul nome del  **EntityContainer** elemento che si desidera includere. Tutti gli elementi figlio di include **EntityContainer** elemento verrà considerato come elementi figlio di fare riferimento alle **EntityContainer** elemento.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Using** elemento.

| Nome attributo | È obbligatorio | Valore                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Yes         | Nome del contenitore di entità.                               |
| **Estende**    | No          | Nome di un altro contenitore di entità all'interno dello stesso spazio dei nomi. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityContainer** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento che definisce tre set di entità e due set di associazioni.

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>Elemento EntitySet (CSDL)

Il **EntitySet** elemento di linguaggio conceptual schema definition language è un contenitore logico per istanze di un tipo di entità e le istanze di qualsiasi tipo derivato da tale tipo di entità. La relazione tra un tipo di entità e un set di entità è analoga alla relazione tra una riga e una tabella in un database relazionale. Analogamente a una riga, un tipo di entità definisce un set di dati correlati e, analogamente a una tabella, un set di entità contiene istanze di quella definizione. Un set di entità fornisce un construct per il raggruppamento di istanze del tipo di entità in modo che se ne possa eseguire il mapping alle strutture dei dati correlati in un'origine dati.  

È possibile definire più set di entità per un particolare tipo di entità.

> [!NOTE]
> La finestra di progettazione di Entity Framework non supporta i modelli concettuali che contengono più set di entità per tipo.

 

Il **EntitySet** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Elemento documentation (zero o un elemento consentito)
-   Elementi Annotation (zero o più elementi consentiti)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySet** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del set di entità.                                                              |
| **Elemento EntityType** | Yes         | Nome completo del tipo di entità per il quale il set di entità contiene delle istanze. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntitySet** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityContainer** elemento con tre **EntitySet** elementi:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

È possibile definire più set di entità per tipo (MEST). L'esempio seguente definisce un contenitore di entità con due set di entità per il **libro** tipo di entità:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>Elemento EntityType (CSDL)

Il **EntityType** elemento rappresenta la struttura di un concetto di livello superiore, ad esempio un cliente o un ordine, in un modello concettuale. Un tipo di entità è un modello per istanze di tipi di entità in un'applicazione. Ogni modello contiene le informazioni seguenti:

-   Un nome univoco. Obbligatorio.
-   Una chiave di entità che è definita da una o più proprietà. Obbligatorio.
-   Proprietà per contenere dati. (Facoltative)
-   Proprietà di navigazione che consentono di navigare da un'entità finale di un'associazione all'altra. (Facoltative)

In un'applicazione, un'istanza di un tipo di entità rappresenta un oggetto specifico, quale ad esempio un cliente o un ordine specifico. Ogni istanza di un tipo di entità deve avere una chiave di entità univoca all'interno di un set di entità.

Due istanze di tipi di entità sono considerate uguali solo se sono dello stesso tipo e se i valori delle rispettive chiavi di entità sono uguali.

Un' **EntityType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Chiave (zero o un elemento)
-   Proprietà (zero o più elementi)
-   NavigationProperty (zero o più elementi)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityType** elemento.

| Nome attributo                                                                                                                                  | È obbligatorio | Valore                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | Yes         | Nome del tipo di entità.                                                                     |
| **BaseType**                                                                                                                                    | No          | Nome di un altro tipo di entità che è il tipo di base del tipo di entità definito.  |
| **Classe astratta**                                                                                                                                    | No          | **True** oppure **False**, a seconda che il tipo di entità sia un tipo astratto.                 |
| **OpenType**                                                                                                                                    | No          | **True** oppure **False** a seconda che il tipo di entità sia un tipo di entità aperto. <br/> [!NOTE] |
| > Il **OpenType** attributo è applicabile solo ai tipi di entità definiti nei modelli concettuali utilizzati con ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EntityType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con tre **proprietà** elementi e due **NavigationProperty** elementi:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>Elemento EnumType (CSDL)

Il **EnumType** elemento rappresenta un tipo enumerato.

Un' **EnumType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Membro (zero o più elementi)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EnumType** elemento.

| Nome attributo     | È obbligatorio | Valore                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | Yes         | Nome del tipo di entità.                                                                                                                                                                  |
| **IsFlags**        | No          | **True** oppure **False**, a seconda se il tipo di enumerazione può essere utilizzato come un set di flag. Il valore predefinito è **False.**.                                                                     |
| **UnderlyingType** | No          | **Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** oppure **Edm.SByte** che definisce l'intervallo di valori del tipo.   È il tipo sottostante predefinito degli elementi di enumerazioni **Edm.Int32.**. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **EnumType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EnumType** elemento con tre **membro** elementi:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Elemento Function (CSDL)

Il **funzione** elemento schema conceptual schema definition Language (CSDL) viene usato per definire o dichiarare funzioni nel modello concettuale. Una funzione viene definita utilizzando un elemento DefiningExpression.  

Oggetto **funzione** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Parametro (zero o più elementi)
-   DefiningExpression (zero o un elemento)
-   ReturnType (funzione) (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** (Function) (elemento) o il **ReturnType** attributo (vedere sotto), ma non entrambi. I tipi restituiti possibili sono il edmSimpleType, il tipo di entità, il tipo complesso, il tipo di riga o il tipo di riferimento o una raccolta di uno di questi tipi.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **funzione** elemento.

| Nome attributo | È obbligatorio | Valore                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | Yes         | Nome della funzione.          |
| **ReturnType** | No          | Tipo restituito dalla funzione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **funzione** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente usa un' **funzione** elemento per definire una funzione che restituisce il numero di anni dopo è stato assunto un docente.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Elemento FunctionImport (CSDL)

Il **FunctionImport** elemento schema conceptual schema definition Language (CSDL) rappresenta una funzione che viene definito nell'origine dati, ma è disponibile per gli oggetti tramite il modello concettuale. Ad esempio, un elemento di funzione nel modello di archiviazione è utilizzabile per rappresentare una stored procedure in un database. Oggetto **FunctionImport** elemento nel modello concettuale rappresenta la funzione corrispondente in un'applicazione Entity Framework e viene eseguito il mapping alla funzione del modello di archiviazione usando l'elemento FunctionImportMapping. Quando la funzione viene chiamata nell'applicazione, la stored procedure corrispondente viene eseguita nel database.

Il **FunctionImport** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento consentito)
-   Parametro (zero o più elementi consentiti)
-   Elementi Annotation (zero o più elementi consentiti)
-   ReturnType (FunctionImport) (zero o più elementi consentiti)

Uno **parametro** elemento deve essere definito per ogni parametro accettati dalla funzione.

Restituisce un tipo per una funzione deve essere specificato mediante il **ReturnType** elemento (FunctionImport) o il **ReturnType** attributo (vedere sotto), ma non entrambi. Il valore del tipo restituito deve essere una raccolta di ComplexType, EntityType o EdmSimpleType.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **FunctionImport** elemento.

| Nome attributo   | È obbligatorio | Valore                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Yes         | Nome della funzione importata.                                                                                                                                                                    |
| **ReturnType**   | No          | Tipo restituito dalla funzione. Non utilizzare questo attributo se la funzione non restituisce un valore. In caso contrario, il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.        |
| **Elemento EntitySet**    | No          | Se la funzione restituisce una raccolta di entità dei tipi, il valore della **EntitySet** deve essere il set di entità a cui appartiene la raccolta. In caso contrario, il **EntitySet** attributo non deve essere usato. |
| **IsComposable** | No          | Se il valore è impostato su true, la funzione è componibile (Table-valued Function) e può essere usata in una query LINQ.  Il valore predefinito è **false**.                                                           |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **FunctionImport** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra una **FunctionImport** elemento che accetta un parametro e restituisce una raccolta di tipi di entità:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Elemento Key (CSDL)

Il **Key** è un elemento figlio dell'elemento EntityType e definisce un *chiave di entità* (una proprietà o un set di proprietà di un tipo di entità che determinano l'identità). Le proprietà che costituiscono una chiave di entità vengono scelte in fase di progettazione. I valori delle proprietà della chiave di entità devono identificare in modo univoco in fase di esecuzione un'istanza del tipo di entità all'interno di un set di entità. Le proprietà che costituiscono una chiave di entità devono essere scelte per garantire univocità delle istanze in un set di entità. Il **chiave** elemento definisce una chiave di entità facendo riferimento a uno o più delle proprietà di un tipo di entità.

Il **chiave** elemento può avere elementi figlio seguenti:

-   PropertyRef (uno o più elementi)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **chiave** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente definisce un tipo di entità denominato **libro**. La chiave di entità viene definita facendo il **ISBN** proprietà del tipo di entità.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

Il **ISBN** proprietà è una buona scelta per la chiave di entità perché un libro numero ISBN (International Standard) che identifica in modo univoco un libro.

L'esempio seguente mostra un tipo di entità (**Author**) che ha una chiave di entità costituita da due proprietà, **Name** e **indirizzo**.

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

Usando **Name** e **indirizzo** per l'entità chiave è una scelta ragionevole, perché due autori con lo stesso nome vivano allo stesso indirizzo. Questa scelta per una chiave di entità non garantisce tuttavia in modo assoluto chiavi di entità univoche in un set di entità. Aggiunta di una proprietà, ad esempio **AuthorId**, che può essere utilizzato per identificare in modo univoco un autore è consigliato in questo caso.

 

## <a name="member-element-csdl"></a>Elemento member (CSDL)

Il **membro** elemento è un elemento figlio dell'elemento EnumType e definisce un membro del tipo enumerato.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **FunctionImport** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del membro.                                                                                                                                                                  |
| **Valore**      | No          | Valore del membro. Per impostazione predefinita, il primo membro è il valore 0 e il valore di ogni enumeratore successivo viene incrementato di 1. Possono esistere più membri con gli stessi valori. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **FunctionImport** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EnumType** elemento con tre **membro** elementi:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Elemento NavigationProperty (CSDL)

Oggetto **NavigationProperty** elemento definisce una proprietà di navigazione, che fornisce un riferimento a altra estremità di un'associazione. A differenza delle proprietà definite con l'elemento proprietà, le proprietà di navigazione non definiscono la forma e le caratteristiche dei dati. Forniscono un modo per navigare in un'associazione tra due tipi di entità.

Si noti che le proprietà di navigazione sono facoltative in entrambi tipi di entità nelle entità finali di un'associazione. Se si definisce una proprietà di navigazione su un tipo di entità nell'entità finale di un'associazione, non è necessario definire una proprietà di navigazione sul tipo di entità nell'altra entità finale dell'associazione.

Il tipo di dati restituito da una proprietà di navigazione è determinato dalla molteplicità della relativa entità finale di associazione remota. Ad esempio, si supponga che una proprietà di navigazione **OrdersNavProp**, esiste in un **cliente** tipo di entità e si sposta un'associazione uno-a-molti tra **cliente** e  **Ordine**. Poiché l'estremità dell'associazione remota per la proprietà di navigazione con molteplicità molti (\*), il tipo di dati è una raccolta (di **ordine**). Allo stesso modo, se una proprietà di navigazione **CustomerNavProp**, sia disponibile la **ordine** tipo di entità, il tipo di dati sarebbe **cliente** poiché la molteplicità dell'entità finale remota è uno (1).

Oggetto **NavigationProperty** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **NavigationProperty** elemento.

| Nome attributo   | È obbligatorio | Valore                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Yes         | Nome della proprietà di navigazione.                                                                                                                                                                                                             |
| **Relazione** | Yes         | Nome di un'associazione che è all'interno dell'ambito del modello.                                                                                                                                                                                |
| **ToRole**       | Yes         | Entità finale dell'associazione in corrispondenza della quale termina la navigazione. Il valore della **ToRole** attributo deve essere identico al valore di uno delle **ruolo** attributi definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd).       |
| **FromRole**     | Yes         | Entità finale dell'associazione dalla quale ha inizio la navigazione. Il valore della **FromRole** attributo deve essere identico al valore di uno delle **ruolo** attributi definiti in una delle entità finali dell'associazione (definite nell'elemento AssociationEnd). |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **NavigationProperty** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente definisce un tipo di entità (**libro**) con due proprietà di navigazione (**PublishedBy** e **WrittenBy**):

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>Elemento OnDelete (CSDL)

Il **OnDelete** elemento schema conceptual schema definition Language (CSDL) definisce il comportamento che è connesso a un'associazione. Se il **azione** attributo è impostato su **Cascade** sul uno lato di un'associazione, relativi tipi di entità in altra estremità dell'associazione vengono eliminati quando viene eliminato il tipo di entità nella prima entità finale. Se l'associazione tra due tipi di entità è una relazione di chiave primaria di chiave-a-primary, allora un oggetto dipendente caricato viene eliminato quando l'oggetto principale in altra estremità dell'associazione viene eliminato indipendentemente i **OnDelete** specifica.  

> [!NOTE]
> Il **OnDelete** elemento interessa solo il comportamento di runtime di un'applicazione, e non influisce sul comportamento nell'origine dati. Il comportamento definito nell'origine dati deve essere uguale al comportamento definito nell'applicazione.

 

Un' **OnDelete** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **OnDelete** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Azione**     | Yes         | **CASCADE** oppure **None**. Se **Cascade**, tipi di entità dipendenti verranno eliminati quando il tipo di entità principale verrà eliminato. Se **None**, tipi di entità dipendenti non verranno eliminati quando il tipo di entità principale verrà eliminato. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Association** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Association** elemento che definisce il **CustomerOrders** association. Il **OnDelete** elemento indica che tutte le **ordini** che sono correlati a un particolare **cliente** e sono stati caricati in ObjectContext saranno eliminati quando il  **Cliente** viene eliminato.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Elemento Parameter (CSDL)

Il **parametro** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento FunctionImport o l'elemento di funzione.

### <a name="functionimport-element-application"></a>Applicazione dell'elemento FunctionImport

Oggetto **parametro** elemento (come figlio delle **FunctionImport** elemento) viene usato per definire i parametri di input e outpui per le importazioni di funzioni dichiarate in CSDL.

Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento consentito)
-   Elementi Annotation (zero o più elementi consentiti)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome del parametro.                                                                                                                                                                                                      |
| **Type**       | Yes         | Tipo del parametro. Il valore deve essere un' **EDMSimpleType** o un tipo complesso che rientra nell'ambito del modello.                                                                                                             |
| **Modalità**       | No          | **Nelle**, **Out**, o **InOut** a seconda che il parametro sia un input, output o parametro di input/output.                                                                                                                |
| **MaxLength**  | No          | Lunghezza massima consentita del parametro.                                                                                                                                                                                    |
| **Precisione**  | No          | Precisione del parametro.                                                                                                                                                                                                 |
| **Scala**      | No          | Scala del parametro.                                                                                                                                                                                                     |
| **SRID**       | No          | Identificatore di riferimento spaziale del sistema. Valido solo per i parametri di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente mostra una **FunctionImport** elemento con uno **parametro** elemento figlio. La funzione accetta un parametro di input e restituisce una raccolta di tipi di entità.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Applicazione dell'elemento Function

Oggetto **parametro** elemento (come figlio del **funzione** elemento) Definisce parametri per le funzioni che sono definite o dichiarate in un modello concettuale.

Il **parametro** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o più elementi)
-   Elemento CollectionType (zero o più elementi)
-   ReferenceType (zero o più elementi)
-   RowType (zero o più elementi)

> [!NOTE]
> Uno solo dei **CollectionType**, **ReferenceType**, o **RowType** gli elementi possono essere un elemento figlio di un **proprietà** elemento.

 

-   Elementi Annotation (zero o più elementi consentiti)

> [!NOTE]
> Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio. Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.

 

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **parametro** elemento.

| Nome attributo   | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Yes         | Nome del parametro.                                                                                                                                                                                                      |
| **Type**         | No          | Tipo del parametro. Un parametro può essere uno qualsiasi dei seguenti tipi (o raccolte di questi tipi): <br/> **EdmSimpleType** <br/> tipo di entità <br/> tipo complesso <br/> tipo di riga <br/> tipo di riferimento                             |
| **Ammette valori null**     | No          | **True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un **null** valore.                                                                                                                          |
| **DefaultValue** | No          | Valore predefinito della proprietà.                                                                                                                                                                                              |
| **MaxLength**    | No          | Lunghezza massima del valore della proprietà.                                                                                                                                                                                       |
| **FixedLength**  | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.                                                                                                                          |
| **Precisione**    | No          | Precisione del valore della proprietà.                                                                                                                                                                                            |
| **Scala**        | No          | Scala del valore della proprietà.                                                                                                                                                                                                |
| **SRID**         | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.                                                                                                                               |
| **regole di confronto**    | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                   |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **parametro** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente mostra una **funzione** elemento che utilizza uno **parametro** elemento figlio per definire un parametro di funzione.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a>Elemento Principal (CSDL)

Il **Principal** conceptual schema definition Language (CSDL) è un elemento figlio all'elemento ReferentialConstraint che definisce l'entità finale di un vincolo referenziale. Oggetto **ReferentialConstraint** elemento definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale. Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità. Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo. Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo. **PropertyRef** gli elementi vengono usati per specificare quali chiavi vengono fatto riferimento dall'entità finale dipendente.

Il **Principal** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   PropertyRef (uno o più elementi)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Principal** elemento.

| Nome attributo | È obbligatorio | Valore                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Ruolo**       | Yes         | Nome del tipo di entità in un'entità finale principale dell'associazione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Principal** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra una **ReferentialConstraint** elemento che fa parte della definizione delle **PublishedBy** association. Il **Id** proprietà delle **server di pubblicazione** tipo di entità costituisce l'entità finale principale del vincolo referenziale.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Elemento Property (CSDL)

Il **proprietà** elemento schema conceptual schema definition Language (CSDL) può essere un figlio dell'elemento EntityType, ComplexType (elemento) o l'elemento RowType.

### <a name="entitytype-and-complextype-element-applications"></a>Applicazione degli elementi EntityType e ComplexType

**Proprietà** elementi (come figli del **EntityType** oppure **ComplexType** elementi) definiscono la forma e le caratteristiche dei dati che sarà contenuti in un'istanza del tipo di entità o istanza del tipo complesso . Le proprietà in un modello concettuale sono analoghe alle proprietà definite su una classe. Nello stesso modo in cui le proprietà su una classe definiscono la forma della classe e forniscono informazioni su oggetti, le proprietà in un modello concettuale definiscono la forma di un tipo di entità e forniscono informazioni su istanze del tipo di entità.

Il **proprietà** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Elemento documentation (zero o un elemento consentito)
-   Elementi Annotation (zero o più elementi consentiti)

I facet seguenti possono essere applicati a un **proprietà** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisione**, **scalabilità**, **Unicode**, **delle regole di confronto**,  **ConcurrencyMode**. I facet sono attributi XML che forniscono informazioni sul modo in cui i valori di proprietà vengono archiviati nell'archivio dati.

> [!NOTE]
> I facet possono essere applicati solo alle proprietà di tipo **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.

| Nome attributo                                                         | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | Yes         | Nome della proprietà.                                                                                                                                                                                                       |
| **Type**                                                               | Yes         | Tipo del valore della proprietà. Il tipo di valore della proprietà deve essere un' **EDMSimpleType** o un tipo complesso (indicato da un nome completo) nell'ambito del modello.                                                 |
| **Ammette valori null**                                                           | No          | **True** (valore predefinito) o <strong>False</strong> a seconda del fatto che la proprietà può avere un valore null. <br/> [!NOTE]                                                                                                   |
| > Nella versione 1 di CSDL deve avere una proprietà del tipo complesso `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | No          | Valore predefinito della proprietà.                                                                                                                                                                                              |
| **MaxLength**                                                          | No          | Lunghezza massima del valore della proprietà.                                                                                                                                                                                       |
| **FixedLength**                                                        | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.                                                                                                                          |
| **Precisione**                                                          | No          | Precisione del valore della proprietà.                                                                                                                                                                                            |
| **Scala**                                                              | No          | Scala del valore della proprietà.                                                                                                                                                                                                |
| **SRID**                                                               | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.                                                                                                                               |
| **regole di confronto**                                                          | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | No          | **None** (valore predefinito) o **Fixed**. Se il valore è impostato su **Fixed**, verrà utilizzato il valore della proprietà nei controlli di concorrenza ottimistica.                                                                                  |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con tre **proprietà** elementi:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

L'esempio seguente mostra una **ComplexType** elemento con cinque **proprietà** elementi:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Applicazione dell'elemento RowType

**Proprietà** elementi (come figli di un **RowType** elemento) definiscono la forma e le caratteristiche dei dati che possono essere passati a o restituiti da una funzione definita dal modello.  

Il **proprietà** elemento può includere esattamente uno degli elementi figlio seguenti:

-   CollectionType
-   ReferenceType
-   RowType

Il **proprietà** elemento può avere qualsiasi numero figlio annotazione di elementi.

> [!NOTE]
> Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.

 

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **proprietà** elemento.

| Nome attributo                                                     | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | Yes         | Nome della proprietà.                                                                                                                                                                                                       |
| **Type**                                                           | Yes         | Tipo del valore della proprietà.                                                                                                                                                                                                 |
| **Ammette valori null**                                                       | No          | **True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null. <br/> [!NOTE]                                                                                                                |
| > Nella versione 1 CSDL deve avere una proprietà del tipo complesso `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | No          | Valore predefinito della proprietà.                                                                                                                                                                                              |
| **MaxLength**                                                      | No          | Lunghezza massima del valore della proprietà.                                                                                                                                                                                       |
| **FixedLength**                                                    | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.                                                                                                                          |
| **Precisione**                                                      | No          | Precisione del valore della proprietà.                                                                                                                                                                                            |
| **Scala**                                                          | No          | Scala del valore della proprietà.                                                                                                                                                                                                |
| **SRID**                                                           | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.                                                                                                                               |
| **regole di confronto**                                                      | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                   |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **proprietà** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

#### <a name="example"></a>Esempio

L'esempio seguente illustra **proprietà** elementi usati per definire la forma del tipo restituito di una funzione definita dal modello.

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>Elemento PropertyRef (CSDL)

Il **PropertyRef** elemento schema conceptual schema definition Language (CSDL) fa riferimento a una proprietà di un tipo di entità per indicare che la proprietà eseguirà uno dei seguenti ruoli:

-   Parte della chiave di entità (una proprietà o un set di proprietà di un tipo di entità che determinano l'identità). Uno o più **PropertyRef** elementi possono essere utilizzati per definire una chiave di entità.
-   L'entità finale dipendente o principale di un vincolo referenziale.

Il **PropertyRef** elemento può avere solo elementi di annotazione (zero o più) come elementi figlio.

> [!NOTE]
> Elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.

 

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **PropertyRef** elemento.

| Nome attributo | È obbligatorio | Valore                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Yes         | Nome della proprietà alla quale viene fatto riferimento. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **PropertyRef** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente definisce un tipo di entità (**libro**). La chiave di entità viene definita facendo il **ISBN** proprietà del tipo di entità.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

Nell'esempio seguente, due **PropertyRef** gli elementi vengono usati per indicare che due proprietà (**Id** e **PublisherId**) sono le entità finali principale e dipendente di un referenziale vincolo.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>Elemento ReferenceType (CSDL)

Il **ReferenceType** elemento schema conceptual schema definition Language (CSDL) specifica un riferimento a un tipo di entità. Il **ReferenceType** elemento può essere un elemento figlio degli elementi seguenti:

-   ReturnType (funzione)
-   Parametro
-   CollectionType

Il **ReferenceType** elemento viene usato quando si definisce un parametro o tipo restituito per una funzione.

Oggetto **ReferenceType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReferenceType** elemento.

| Nome attributo | È obbligatorio | Valore                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | Yes         | Nome del tipo di entità a cui viene fatto riferimento. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReferenceType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

Nell'esempio seguente il **ReferenceType** elemento utilizzato come elemento figlio di un **parametro** elemento in una funzione definita dal modello che accetta un riferimento a una **persona** entità tipo:

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

Nell'esempio seguente il **ReferenceType** elemento utilizzato come elemento figlio di un **ReturnType** elemento (funzione) in una funzione definita dal modello che restituisce un riferimento a un **persona**tipo di entità:

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>Elemento ReferentialConstraint (CSDL)

Oggetto **ReferentialConstraint** elemento schema conceptual schema definition Language (CSDL) definisce la funzionalità che è simile a un vincolo di integrità referenziale in un database relazionale. Nello stesso modo in cui una colonna (o più colonne) da una tabella di database può fare riferimento alla chiave primaria di un'altra tabella, una proprietà (o più proprietà) di un tipo di entità può fare riferimento alla chiave di entità di un altro tipo di entità. Il tipo di entità cui viene fatto riferimento viene chiamato il *entità finale principale* del vincolo. Il tipo di entità che fa riferimento l'entità finale principale viene chiamato il *entità finale dipendente* del vincolo.

Se una chiave esterna che viene esposto in un tipo di entità fa riferimento a una proprietà in un altro tipo di entità, il **ReferentialConstraint** elemento definisce un'associazione tra i due tipi di entità. Poiché il **ReferentialConstraint** elemento fornisce informazioni sul modo in cui due tipi di entità sono correlate, nessun elemento AssociationSetMapping corrispondente è necessario lo schema mapping specification language (MSL). Un'associazione tra due tipi di entità che non dispongono di chiavi esterne esposte deve disporre di un oggetto corrispondente **AssociationSetMapping** elemento per eseguire il mapping di informazioni sull'associazione all'origine dati.

Se una chiave esterna non viene esposto in un tipo di entità, il **ReferentialConstraint** elemento può definire solo un vincolo primary key-a-primary key tra il tipo di entità e un altro tipo di entità.

Oggetto **ReferentialConstraint** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Entità (esattamente un elemento)
-   Dipendente (esattamente un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Il **ReferentialConstraint** elemento può avere qualsiasi numero di attributi di annotazione (attributi XML personalizzati). Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente mostra una **ReferentialConstraint** elemento utilizzato come parte della definizione del **PublishedBy** association.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>Elemento ReturnType (funzione) (CSDL)

Il **ReturnType** (funzione) elemento schema conceptual schema definition Language (CSDL) specifica il tipo restituito per una funzione che viene definito in un elemento di funzione. Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.

Tipi restituiti possono essere qualsiasi **EdmSimpleType**, tipo di entità, tipo complesso, tipo di riga, tipo di riferimento o una raccolta di uno di questi tipi.

Il tipo restituito di una funzione può essere specificato mediante il **tipo** attributo del **ReturnType** elemento (funzione), o con uno degli elementi figlio seguenti:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Un modello non eseguirà la convalida se si specifica un restituito dalla funzione tipo con entrambe le **tipo** attributo del **ReturnType** (Function) (elemento) e uno degli elementi figlio.

 

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReturnType** (Function) (elemento).

| Nome attributo | È obbligatorio | Valore                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | No          | Tipo restituito dalla funzione. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** (Function) (elemento). Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente usa un' **funzione** elemento per definire una funzione che restituisce il numero di anni in un libro è stato in stampa. Si noti che il tipo restituito è specificato per il **tipo** attributo di un **ReturnType** (Function) (elemento).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>Elemento ReturnType (FunctionImport) (CSDL)

Il **ReturnType** (FunctionImport) elemento schema conceptual schema definition Language (CSDL) specifica il tipo restituito per una funzione che viene definito in un elemento FunctionImport. Una funzione di tipo restituito può essere specificato anche con un **ReturnType** attributo.

Tipi restituiti possono essere qualsiasi raccolta di tipo di entità, tipo complesso, oppure **EdmSimpleType**,

Viene specificato il tipo restituito di una funzione con il **tipo** attributo il **ReturnType** elemento (FunctionImport).

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **ReturnType** elemento (FunctionImport).

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | No          | Tipo restituito dalla funzione. Il valore deve essere una raccolta di ComplexType, EntityType o EDMSimpleType.                                                                                      |
| **Elemento EntitySet**  | No          | Se la funzione restituisce una raccolta di entità dei tipi, il valore della **EntitySet** deve essere il set di entità a cui appartiene la raccolta. In caso contrario, il **EntitySet** attributo non deve essere usato. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **ReturnType** elemento (FunctionImport). Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente usa un' **FunctionImport** che restituisce libri e i server di pubblicazione. Si noti che la funzione restituisce due set di risultati e quindi due **ReturnType** vengono specificati gli elementi (FunctionImport).

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Elemento RowType (CSDL)

Oggetto **RowType** elemento schema conceptual schema definition Language (CSDL) definisce una struttura senza nome come parametro o tipo restituito per una funzione definita nel modello concettuale.

Oggetto **RowType** elemento può essere l'elemento figlio degli elementi seguenti:

-   CollectionType
-   Parametro
-   ReturnType (funzione)

Oggetto **RowType** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Proprietà (una o più)
-   Elementi Annotation (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **RowType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

### <a name="example"></a>Esempio

L'esempio seguente illustra una funzione definita dal modello che usa un' **CollectionType** elemento per specificare che la funzione restituisce una raccolta di righe (come specificato nella **RowType** elemento).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>Elemento Schema (CSDL)

Il **Schema** elemento è l'elemento radice di una definizione del modello concettuale. Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello concettuale.

Il **Schema** elemento può contenere zero o più degli elementi figlio seguenti:

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   Associazione
-   ComplexType
-   Funzione

Oggetto **Schema** elemento può contenere zero o un elemento di annotazione.

> [!NOTE]
> Il **funzione** elemento e gli elementi di annotazione sono consentiti solo nella versione 2 CSDL e versioni successive.

 

Il **Schema** elemento utilizza le **Namespace** attributo per definire lo spazio dei nomi per il tipo di entità, tipo complesso e gli oggetti di associazione in un modello concettuale. All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome. Gli spazi dei nomi possono estendersi a più **Schema** elementi e più file con estensione CSDL.

Uno spazio dei nomi del modello concettuale è diverso dallo spazio dei nomi XML del **Schema** elemento. Uno spazio dei nomi del modello concettuale (come definito per il **Namespace** attributi) è un contenitore logico per tipi di entità, tipi complessi e tipi di associazione. Lo spazio dei nomi XML (indicato dal **xmlns** attributo) di un **Schema** elemento è lo spazio dei nomi predefinito per gli elementi figlio e attributi del **Schema** elemento. Spazi dei nomi XML nel formato http://schemas.microsoft.com/ado/YYYY/MM/edm (dove YYYY e MM rappresentano un anno e mese rispettivamente) sono riservati a CSDL. Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Schema** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spazio dei nomi**  | Yes         | Spazio dei nomi del modello concettuale. Il valore della **Namespace** viene usato in modo da formare il nome completo di un tipo. Ad esempio, se un **EntityType** denominata *cliente* trova nello spazio dei nomi si, specificare il nome completo del **EntityType** è Simpleexamplemodel. <br/> Le stringhe seguenti non possono essere usate come valore per il **Namespace** attributo: **System**, **temporaneo**, oppure **Edm**. Il valore per il **Namespace** attributo non può essere identico al valore per il **Namespace** attributo nell'elemento di Schema SSDL. |
| **Alias**      | No          | Identificatore utilizzato al posto del nome dello spazio dei nomi. Ad esempio, se un **EntityType** denominata *cliente* è lo spazio dei nomi si e il valore del **Alias** attributo è *Model*, è possibile utilizzare Model. Customer come nome completo del **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Schema** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente mostra una **Schema** elemento che contiene un' **EntityContainer** elemento, due **EntityType** elementi e uno **associazione** elemento.

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>Elemento TypeRef (CSDL)

Il **TypeRef** elemento schema conceptual schema definition Language (CSDL) fornisce un riferimento a un tipo con nome esistente. Il **TypeRef** elemento può essere un figlio dell'elemento CollectionType, che consente di specificare che una funzione dispone di una raccolta come un parametro o tipo restituito.

Oggetto **TypeRef** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   Documentazione (zero o un elemento)
-   Elementi Annotation (zero o più elementi)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **TypeRef** elemento. Si noti che il **DefaultValue**, **MaxLength**, **FixedLength**, **precisione**, **scalabilità**,  **Unicode**, e **regole di confronto** gli attributi sono applicabili solo alle **semplici EDM**.

| Nome attributo                                                     | È obbligatorio | Valore                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | No          | Nome del tipo a cui si fa riferimento.                                                                                                                                                                                          |
| **Ammette valori null**                                                       | No          | **True** (valore predefinito) o **False** a seconda del fatto che la proprietà può avere un valore null. <br/> [!NOTE]                                                                                                                |
| > Nella versione 1 CSDL deve avere una proprietà del tipo complesso `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | No          | Valore predefinito della proprietà.                                                                                                                                                                                              |
| **MaxLength**                                                      | No          | Lunghezza massima del valore della proprietà.                                                                                                                                                                                       |
| **FixedLength**                                                    | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come una stringa di lunghezza fissa.                                                                                                                          |
| **Precisione**                                                      | No          | Precisione del valore della proprietà.                                                                                                                                                                                            |
| **Scala**                                                          | No          | Scala del valore della proprietà.                                                                                                                                                                                                |
| **SRID**                                                           | No          | Identificatore di riferimento spaziale del sistema. Valido solo per le proprietà di tipi spaziali. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | No          | **True** oppure **False** a seconda se il valore della proprietà essere archiviato come stringa Unicode.                                                                                                                               |
| **regole di confronto**                                                      | No          | Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.                                                                                                                                                   |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **CollectionType** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

L'esempio seguente illustra una funzione definita dal modello che utilizza il **TypeRef** elemento (come figlio di un **CollectionType** elemento) per specificare che la funzione accetta una raccolta di  **Reparto** i tipi di entità.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>Elemento Using (CSDL)

Il **Using** elemento schema conceptual schema definition Language (CSDL) Importa il contenuto di un modello concettuale che esiste in uno spazio dei nomi diversi. Impostando il valore della **Namespace** attributo, è possibile fare riferimento a tipi di entità, tipi complessi e tipi di associazione definiti in un altro modello concettuale. Più di un **Using** elemento può essere un figlio di un **Schema** elemento.

> [!NOTE]
> Il **Using** elemento in CSDL non funziona esattamente come una **usando** istruzione in un linguaggio di programmazione. Tramite l'importazione di uno spazio dei nomi con un **usando** istruzione in un linguaggio di programmazione, si influisce sugli oggetti nello spazio dei nomi originale. In CSDL, uno spazio dei nomi importato può contenere un tipo di entità derivato da un tipo di entità nello spazio dei nomi originale. Ciò può influire sui set di entità dichiarati nello spazio dei nomi originale.

 

Il **Using** elemento può avere elementi figlio seguenti:

-   Documentazione (zero o un elemento consentito)
-   Elementi Annotation (zero o più elementi consentiti)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi possono essere applicati per il **Using** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spazio dei nomi**  | Yes         | Nome dello spazio dei nomi importato.                                                                                                                                                |
| **Alias**      | Yes         | Identificatore utilizzato al posto del nome dello spazio dei nomi. Anche se questo attributo è obbligatorio, non è necessario utilizzarlo al posto del nome dello spazio dei nomi per qualificare nomi di oggetti. |

 

> [!NOTE]
> Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato per il **Using** elemento. Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL. I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.

 

### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato il **Using** elemento utilizzato per importare uno spazio dei nomi definito altrove. Si noti che lo spazio dei nomi per il **Schema** elemento mostrato è `BooksModel`. Il `Address` proprietà il `Publisher` **EntityType** è un tipo complesso definito nel `ExtendedBooksModel` dello spazio dei nomi (importati con il **Using** elemento).

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>Attributi di annotazione (CSDL)

Gli attributi di annotazione in Conceptual Schema Definition Language (CSDL) sono attributi XML personalizzati nel modello concettuale. Oltre ad avere una struttura XML valida, per gli attributi di annotazione hanno le caratteristiche seguenti:

-   Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a CSDL.
-   È possibile applicare più attributi di annotazione a un determinato elemento CSDL.
-   I nomi completi di due attributi di annotazione non devono essere uguali.

Gli attributi di annotazione possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale. Metadati contenuti negli elementi di annotazione sono accessibili in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con un attributo di annotazione (**CustomAttribute**). Nell'esempio viene inoltre mostrato un elemento di annotazione applicato all'elemento del tipo di entità.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Il codice seguente recupera i metadati dell'attributo di annotazione e li scrive nella console:

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Nel codice riportato in precedenza si presuppone che il file `School.csdl` si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementi di annotazione (CSDL)

Gli elementi Annotation in Conceptual Schema Definition Language (CSDL) sono elementi XML personalizzati nel modello concettuale. Oltre ad avere una struttura XML valida, gli elementi Annotation hanno le caratteristiche seguenti:

-   Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a CSDL.
-   Più elementi Annotation possono essere figli di un dato elemento CSDL.
-   I nomi completi di due elementi Annotation non devono essere uguali.
-   Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento CSDL.

Gli elementi Annotation possono essere utilizzati per fornire metadati aggiuntivi sugli elementi in un modello concettuale. A partire da .NET Framework versione 4, i metadati contenuti in elementi annotation sono accessibile in fase di esecuzione usando le classi nello spazio dei nomi System.Data.Metadata.Edm.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntityType** elemento con un elemento annotation (**CustomElement**). Nell'esempio viene inoltre mostrato un attributo di annotazione applicato all'elemento del tipo di entità.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Il codice seguente recupera i metadati dell'elemento Annotation e li scrive nella console:

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Nel codice riportato in precedenza si presuppone che il file School.csdl si trovi nella directory di output del progetto e che al progetto siano state aggiunte le istruzioni `Imports` e `Using` seguenti:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Tipi del modello concettuale (CSDL)

Linguaggio di definizione schema concettuale (CSDL) supporta un set di tipi di dati primitivi astratti, chiamato **semplici EDM**, che definiscono le proprietà in un modello concettuale. **Semplici EDM** sono proxy per i tipi di dati primitivi supportati in ambiente di hosting o nell'archivio.

Nella tabella seguente vengono elencati i tipi di dati primitivi supportati da CSDL. Vengono inoltre elencati i facet che possono essere applicati a ciascun **EDMSimpleType**.

| EDMSimpleType                    | Descrizione                                                | Facet applicabili                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM. Binary**                   | Contiene dati binari.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Boolean**                  | Contiene il valore **true** oppure **false**.                  | Nullable, Default                                                        |
| **Edm.Byte**                     | Contiene un Unsigned Integer a 8 bit.                  | Precision, Nullable, Default                                             |
| **EDM**                 | Rappresenta una data e un'ora.                                | Precision, Nullable, Default                                             |
| **DateTimeOffset**           | Contiene una data e un'ora come offset in minuti rispetto all'ora GMT. | Precision, Nullable, Default                                             |
| **Decimal**                  | Contiene un valore numerico con scala e precisione fisse.   | Precision, Nullable, Default                                             |
| **EDM. Double**                   | Contiene un numero con precisione a 15 cifre a virgola mobile   | Precision, Nullable, Default                                             |
| **Edm.Float**                    | Contiene un numero a virgola mobile con precisione a 7 cifre.   | Precision, Nullable, Default                                             |
| **EDM. GUID**                     | Contiene un identificatore univoco a 16 byte.                      | Precision, Nullable, Default                                             |
| **Edm.Int16**                    | Contiene un Signed Integer a 16 bit.                    | Precision, Nullable, Default                                             |
| **Edm.Int32**                    | Contiene un Signed Integer a 32 bit.                    | Precision, Nullable, Default                                             |
| **Edm.Int64**                    | Contiene un Signed Integer a 64 bit.                    | Precision, Nullable, Default                                             |
| **Edm.SByte**                    | Contiene un Signed Integer a 8 bit.                     | Precision, Nullable, Default                                             |
| **EDM. String**                   | Contiene dati di tipo carattere.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **EDM**                     | Contiene un'ora del giorno.                                    | Precision, Nullable, Default                                             |
| **Geography**                |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **EDM. geographypoint**           |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Geographypolygon**         |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **EDM**                 |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Ammette valori null, impostazione predefinita, l'identificatore SRID                                                  |

## <a name="facets-csdl"></a>Facet (CSDL)

I facet in Conceptual Schema Definition Language (CSDL) rappresentano vincoli sulle proprietà di tipi di entità e di tipi complessi. I facet appaiono come attributi XML negli elementi CSDL seguenti:

-   Proprietà
-   TypeRef
-   Parametro

Nella tabella seguente vengono descritti i facet supportati in CSDL. Tutti i facet sono facoltativi. Alcuni facet elencati di seguito vengono utilizzate da Entity Framework durante la generazione di un database da un modello concettuale.

> [!NOTE]
> Per informazioni sui tipi di dati in un modello concettuale, vedere i tipi di modello concettuale (CSDL).

| Facet               | Descrizione                                                                                                                                                                                                                                                   | Si applica a                                                                                                                                                                                                                                                                                                                                                                           | Utilizzato per la generazione di database. | Utilizzato dal runtime |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **regole di confronto**       | Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.                                                                                                               | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Yes                              | No                  |
| **ConcurrencyMode** | Indica che il valore della proprietà deve essere usato per le verifiche della concorrenza ottimistica.                                                                                                                                                                    | Tutti i **EDMSimpleType** proprietà                                                                                                                                                                                                                                                                                                                                                     | No                               | Yes                 |
| **Default**         | Specifica il valore predefinito della proprietà se durante la creazione di istanze non viene fornito alcun valore.                                                                                                                                                                       | Tutti i **EDMSimpleType** proprietà                                                                                                                                                                                                                                                                                                                                                     | Yes                              | Yes                 |
| **FixedLength**     | Specifica se la lunghezza del valore della proprietà può variare.                                                                                                                                                                                                  | **EDM. Binary**, **Edm. String**                                                                                                                                                                                                                                                                                                                                                       | Yes                              | No                  |
| **MaxLength**       | Specifica la lunghezza massima del valore della proprietà.                                                                                                                                                                                                           | **EDM. Binary**, **Edm. String**                                                                                                                                                                                                                                                                                                                                                       | Yes                              | No                  |
| **Ammette valori null**        | Specifica se la proprietà può avere una **null** valore.                                                                                                                                                                                                     | Tutti i **EDMSimpleType** proprietà                                                                                                                                                                                                                                                                                                                                                     | Yes                              | Yes                 |
| **Precisione**       | Per le proprietà di tipo **decimale**, specifica il numero di cifre può avere un valore della proprietà. Per le proprietà di tipo **tempo**, **DateTime**, e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria di secondi del valore della proprietà. | **EDM**, **DateTimeOffset**, **decimal**, **EDM**                                                                                                                                                                                                                                                                                                              | Yes                              | No                  |
| **Scala**           | Specifica il numero di cifre a destra del separatore decimale per il valore della proprietà.                                                                                                                                                                      | **Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Yes                              | No                  |
| **SRID**            | Specifica l'ID sistema spaziale riferimento del sistema. Per altre informazioni, vedere [SRID](http://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Geography, EDM. geographypoint, Edm.GeographyLineString, geographypolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, EDM, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | No                               | Yes                 |
| **Unicode**         | Viene indicato se il valore della proprietà viene archiviato come Unicode.                                                                                                                                                                                                    | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Yes                              | Yes                 |

>[!NOTE]
> Durante la generazione di un database da un modello concettuale, la procedura guidata Crea Database riconosceranno il valore della **StoreGeneratedPattern** dell'attributo su un **proprietà** elemento se è nel seguente spazio dei nomi: http://schemas.microsoft.com/ado/2009/02/edm/annotation . I valori supportati per l'attributo vengono **Identity** e **calcolata**. Un valore pari **identità** produrrà una colonna di database con un valore identity generato nel database. Un valore pari **calcolata** produrrà una colonna con un valore calcolato nel database.

### <a name="example"></a>Esempio

Nell'esempio seguente vengono mostrati i facet applicati alle proprietà di un tipo di entità:

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
