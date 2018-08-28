---
title: La definizione di Query - finestra di progettazione di Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 60d5310589bb9bc3fdb971673422e80537357e55
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996307"
---
# <a name="defining-query---ef-designer"></a>Definizione di Query - finestra di progettazione di Entity Framework
Questa procedura dettagliata illustra come aggiungere una definizione di un tipo di query e un'entità corrispondente a un modello usando la finestra di progettazione di Entity Framework. Una query di definizione viene comunemente utilizzata per fornire funzionalità simili a quelle fornite da una vista di database, ma la vista viene definita nel modello, non il database. Una query di definizione consente di eseguire un'istruzione SQL specificata nella **DefiningQuery** elemento di un file con estensione edmx. Per altre informazioni, vedere **DefiningQuery** nel [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Quando si usano query di definizione, è necessario anche definire un tipo di entità nel modello. Il tipo di entità viene utilizzato per far emergere i dati esposti dalla query di definizione. Si noti che i dati emersi tramite questo tipo di entità sono di sola lettura.

Le query con parametri non possono essere eseguite come query di definizione. Tuttavia, è possibile aggiornare i dati eseguendo il mapping delle funzioni di inserimento, aggiornamento ed eliminazione del tipo di entità che fa emergere i dati nelle stored procedure. Per altre informazioni, vedere [Insert, Update e Delete con Stored procedure](~/ef6/modeling/designer/stored-procedures/cud.md).

In questo argomento viene illustrato come eseguire le attività seguenti.

-   Aggiungere una Query di definizione
-   Aggiungere un tipo di entità al modello
-   Eseguire il mapping di Query di definizione per il tipo di entità

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

Questa procedura dettagliata Usa Visual Studio 2012 o versioni successiva.

-   Aprire Visual Studio.
-   Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **applicazione Console** modello.
-   Immettere **DefiningQuerySample** come nome del progetto e fare clic su **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Creare un modello basato sul Database School

-   Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **DefiningQueryModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.
-   Fare clic su nuova connessione. Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella finestra di dialogo Scegli oggetti di Database, verificare i **tabelle** nodo. Verrà aggiunto tutte le tabelle per il **School** modello.
-   Scegliere **Fine**.
-   In Esplora soluzioni fare doppio clic il **DefiningQueryModel.edmx** del file e selezionare **Apri con...** .
-   Selezionare **Editor XML (testo)**.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   Fare clic su **Sì** se viene richiesto con il messaggio seguente:

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Aggiungere una Query di definizione

In questo passaggio si userà l'Editor XML per aggiungere una definizione di query e un tipo di entità alla sezione SSDL del file con estensione edmx. 

-   Aggiungere un **EntitySet** elemento alla sezione SSDL del file con estensione edmx (riga 5 a 13). Specificare quanto segue:
    -   Solo le **Name** e **EntityType** gli attributi del **EntitySet** elemento vengono specificati.
    -   Il nome completo del tipo di entità viene utilizzato nel **EntityType** attributo.
    -   L'istruzione SQL da eseguire è specificata nel **DefiningQuery** elemento.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Aggiungere il **EntityType** elemento alla sezione SSDL del file con estensione. file come mostrato di seguito. Tenere presente quanto segue:
    -   Il valore della **nome** attributo corrisponde al valore del **EntityType** attributo il **EntitySet** elemento precedente, anche se il nome completo del tipo di entità viene utilizzato il **EntityType** attributo.
    -   I nomi di proprietà corrispondono ai nomi delle colonne restituiti dall'istruzione SQL nel **DefiningQuery** elemento (sopra).
    -   In questo esempio la chiave di entità è composta da tre proprietà, per assicurare un valore di chiave univoco.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Se in seguito si esegue la **procedura guidata Aggiorna modello** finestra di dialogo, tutte le modifiche apportate al modello di archiviazione, inclusa la definizione delle query, verrà sovrascritto.

 

## <a name="add-an-entity-type-to-the-model"></a>Aggiungere un tipo di entità al modello

In questo passaggio si aggiungerà il tipo di entità al modello concettuale usando la finestra di progettazione di Entity Framework.  Tenere presente quanto segue:

-   Il **Name** dell'entità corrispondente al valore del **EntityType** attributo il **EntitySet** elemento precedente.
-   I nomi di proprietà corrispondono ai nomi delle colonne restituiti dall'istruzione SQL nel **DefiningQuery** elemento precedente.
-   In questo esempio la chiave di entità è composta da tre proprietà, per assicurare un valore di chiave univoco.

Aprire il modello nella finestra di progettazione di Entity Framework.

-   Fare doppio clic il DefiningQueryModel.edmx.
-   Pronunciare **Sì** per il messaggio seguente:

    ![Warning2](~/ef6/media/warning2.png)

 

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.

-   Fare doppio clic su area di designer e seleziona **Aggiungi nuovo**-&gt;**Entity...** .
-   Specificare **GradeReport** per il nome dell'entità e **CourseID** per il **proprietà Key**.
-   Fare doppio clic il **GradeReport** entità e selezionare **Aggiungi nuovo** - &gt; **proprietà scalare**.
-   Modificare il nome predefinito della proprietà **FirstName**.
-   Aggiungere un'altra proprietà scalare e specificare **LastName** per il nome.
-   Aggiungere un'altra proprietà scalare e specificare **Grade** per il nome.
-   Nel **proprietà** finestra Modifica il **Grade**del **tipo** proprietà **Decimal**.
-   Selezionare il **FirstName** e **LastName** proprietà.
-   Nel **delle proprietà** finestra Modifica il **EntityKey** valore della proprietà **True**.

Di conseguenza, gli elementi seguenti sono stati aggiunti per il **CSDL** sezione del file con estensione edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Eseguire il mapping di Query di definizione per il tipo di entità

In questo passaggio si userà la finestra Dettagli Mapping per eseguire il mapping concettuale e i tipi di entità di archiviazione.

-   Fare doppio clic il **GradeReport** entità di un'area di progettazione e seleziona **Mapping tabella**.  
    Il **Dettagli Mapping** viene visualizzata la finestra.
-   Selezionare **GradeReport** dalle **&lt;aggiungere una tabella o vista&gt;** nell'elenco a discesa (sotto **tabella**s).  
    Mapping tra concettuale predefinito e di archiviazione **GradeReport** vengono visualizzati il tipo di entità.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

Di conseguenza, il **EntitySetMapping** elemento viene aggiunto alla sezione del mapping del file con estensione edmx. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Compilare l'applicazione.

 

## <a name="call-the-defining-query-in-your-code"></a>Chiamare la Query di definizione nel codice

È ora possibile eseguire la query di definizione usando il **GradeReport** tipo di entità. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
