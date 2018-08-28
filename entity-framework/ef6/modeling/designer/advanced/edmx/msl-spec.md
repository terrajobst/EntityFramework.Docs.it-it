---
title: Specifica MSL - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996031"
---
# <a name="msl-specification"></a>Specifica MSL
Linguaggio di specifica di mapping (MSL) è un linguaggio basato su XML che descrive il mapping tra il modello concettuale e il modello di archiviazione di un'applicazione Entity Framework.

In un'applicazione Entity Framework, i metadati di mapping vengono caricati da un file con estensione MSL (scritto nel linguaggio MSL) in fase di compilazione. Entity Framework Usa i metadati di mapping in fase di esecuzione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.

Entity Framework Designer (Entity Framework Designer) archivia le informazioni di mapping in un file con estensione edmx in fase di progettazione. In fase di compilazione, Entity Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione MSL che serve da Entity Framework in fase di esecuzione

I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi. Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere [specifica CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Le versioni di MSL si differenziano dagli spazi dei nomi XML.

| Versione MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Elemento Alias (MSL)

Il **Alias** elemento specifica linguaggio MSL (mapping) è un figlio dell'elemento di Mapping che viene usato per definire gli alias spazi dei nomi del modello concettuali di modello e di archiviazione. I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi. Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere l'elemento Schema (CSDL). Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere l'elemento Schema (SSDL).

Il **Alias** elemento non può avere elementi figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Alias** elemento.

| Nome attributo | È obbligatorio | Valore                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Yes         | L'alias dello spazio dei nomi specificato per il **valore** attributo. |
| **Valore**      | Yes         | Lo spazio dei nomi per il quale il valore della **chiave** elemento è un alias.     |

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **Alias** elemento che definisce un alias, `c`, per i tipi definiti nel modello concettuale.

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a>Elemento AssociationEnd (MSL)

Il **AssociationEnd** elemento in specifica linguaggio MSL (mapping) viene usato quando le funzioni di modifica di un tipo di entità nel modello concettuale vengono mappate alle stored procedure nel database sottostante. Se una modifica stored procedure accetta un parametro il cui valore è contenuto in una proprietà di associazione, il **AssociationEnd** elemento viene mappato il valore della proprietà al parametro. Per ulteriori informazioni, vedere l'esempio seguente.

Per altre informazioni sull'esecuzione del mapping delle funzioni di modifica dei tipi di entità alle stored procedure, vedere l'elemento ModificationFunctionMapping (MSL) e la procedura dettagliata: Mapping di un'entità alle Stored procedure.

Il **AssociationEnd** elemento può avere elementi figlio seguenti:

-   ScalarProperty

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **AssociationEnd** elemento.

| Nome attributo     | È obbligatorio | Valore                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Elemento AssociationSet** | Yes         | Nome dell'associazione di cui è in corso il mapping.                                                                                                                                 |
| **From**           | Yes         | Il valore della **FromRole** attributo della proprietà di navigazione che corrisponde all'associazione in corso il mapping. Per altre informazioni, vedere l'elemento NavigationProperty (CSDL). |
| **Per**             | Yes         | Il valore della **ToRole** attributo della proprietà di navigazione che corrisponde all'associazione in corso il mapping. Per altre informazioni, vedere l'elemento NavigationProperty (CSDL).   |

### <a name="example"></a>Esempio

Si consideri il tipo di entità del modello concettuale seguente:

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

Si consideri inoltre la seguente stored procedure:

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

Per eseguire il mapping della funzione di aggiornamento della `Course` entità a questa stored procedure, è necessario specificare un valore per il **DepartmentID** parametro. Il valore di `DepartmentID` non corrisponde a una proprietà sul tipo di entità. È contenuto in un'associazione indipendente il cui mapping è illustrato di seguito:

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

Il codice seguente illustra il **AssociationEnd** elemento usato per eseguire il mapping di **DepartmentID** proprietà del **FK\_corso\_reparto** associazione per il **UpdateCourse** stored procedure di (a cui la funzione di aggiornamento del **corso** viene mappato il tipo di entità):

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a>Elemento AssociationSetMapping (MSL)

Il **AssociationSetMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'associazione concettuale colonne modello e una tabella nel database sottostante.

Le associazioni del modello concettuale sono tipi le cui proprietà rappresentano colonne di chiavi primarie ed esterne nel database sottostante. Il **AssociationSetMapping** elemento utilizza due elementi EndProperty per definire i mapping tra le proprietà di tipo di associazione e le colonne nel database. È possibile impostare condizioni su questi mapping con l'elemento Condition. Eseguire il mapping di insert, update e delete funzioni per le associazioni alle stored procedure nel database con l'elemento ModificationFunctionMapping. Definire i mapping di sola lettura tra associazioni e le colonne della tabella utilizzando una stringa Entity SQL in un elemento QueryView.

> [!NOTE]
> Se per un'associazione nel modello concettuale viene definito un vincolo referenziale, l'associazione non è necessario eseguire il mapping con un **AssociationSetMapping** elemento. Se un' **AssociationSetMapping** elemento è presente un'associazione con un vincolo referenziale, i mapping definiti nel **AssociationSetMapping** elemento verrà ignorato. Per altre informazioni, vedere l'elemento ReferentialConstraint (CSDL).

Il **AssociationSetMapping** elemento può avere i seguenti elementi figlio

-   Elemento QueryView (zero o più)
-   EndProperty (zero o due)
-   Condizione (zero o più)
-   ModificationFunctionMapping (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSetMapping** elemento.

| Nome attributo     | È obbligatorio | Valore                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Yes         | Nome del set di associazioni del modello concettuale di cui è in corso il mapping.                      |
| **TypeName**       | No          | Nome completo dello spazio dei nomi del tipo di associazione del modello concettuale di cui è in corso il mapping. |
| **StoreEntitySet** | No          | Nome della tabella di cui è in corso il mapping.                                                 |

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **AssociationSetMapping** elemento in cui il **FK\_corso\_reparto** set di associazioni nel modello concettuale sono mappato al  **Corso** tabella nel database. I mapping tra le colonne della tabella e proprietà del tipo di associazione vengono specificati in figlio **EndProperty** elementi.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
``` 

## <a name="complexproperty-element-msl"></a>Elemento ComplexProperty (MSL)

Oggetto **ComplexProperty** elemento specifica MSL (mapping language) definisce il mapping tra una proprietà di tipo complesso in un modello concettuale entità tabella e tipo di colonne nel database sottostante. I mapping di colonne delle proprietà sono specificati negli elementi ScalarProperty figlio.

Il **ComplexType** property (elemento) può avere elementi figlio seguenti:

-   ScalarProperty (zero o più)
-   **Elemento ComplexProperty** (zero o più)
-   ComplextTypeMapping (zero o più)
-   Condizione (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ComplexProperty** elemento:

| Nome attributo | È obbligatorio | Valore                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome della proprietà complessa di un tipo di entità nel modello concettuale di cui è in corso il mapping. |
| **TypeName**   | No          | Nome qualificato di spazio dei nomi del tipo di proprietà del modello concettuale.                              |

### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School. Il tipo complesso seguente è stato aggiunto al modello concettuale:

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

Il **LastName** e **FirstName** le proprietà del **persona** tipo di entità sono stati sostituiti con una proprietà complessa, **nome**:

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

MSL seguente il **ComplexProperty** elemento usato per eseguire il mapping di **nome** proprietà alle colonne nel database sottostante:

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a>Elemento ComplexTypeMapping (MSL)

Il **ComplexTypeMapping** elemento in specifica linguaggio MSL (mapping) è un figlio dell'elemento ResultMapping e definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nell'oggetto sottostante database quando vengono soddisfatte le seguenti:

-   Un tipo complesso concettuale viene restituito dall'importazione di funzioni.
-   I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo complesso.

Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo complesso è basato sui nomi delle colonne e delle proprietà. Se i nomi delle colonne non corrispondono esattamente i nomi delle proprietà, è necessario usare il **ComplexTypeMapping** elemento per definire il mapping. Per un esempio del mapping predefinito, vedere l'elemento FunctionImportMapping (MSL).

Il **ComplexTypeMapping** elemento può avere elementi figlio seguenti:

-   ScalarProperty (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ComplexTypeMapping** elemento.

| Nome attributo | È obbligatorio | Valore                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | Yes         | Nome completo dello spazio dei nomi del tipo complesso di cui è in corso il mapping. |

### <a name="example"></a>Esempio

Si consideri la seguente stored procedure:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Si consideri anche il tipo complesso del modello concettuale seguente:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Per creare un'importazione di funzioni che restituisce istanze del tipo complesso precedente, il mapping tra le colonne restituite dalla stored procedure e il tipo di entità deve essere definito un **ComplexTypeMapping** elemento:

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a>Elemento Condition (MSL)

Il **condizione** elemento (MSL) mapping specification language stabilire le condizioni per i mapping tra il modello concettuale e il database sottostante. Il mapping definito all'interno di un nodo XML non è valide se tutte le condizioni, come specificato nell'elemento figlio **condizione** elementi, siano soddisfatti. In caso contrario, il mapping non è valido. Ad esempio, se un elemento MappingFragment contiene uno o più **condizione** gli elementi figlio, il mapping definito all'interno la **MappingFragment** nodo sarà valido solo se tutte le condizioni dell'elemento figlio  **Condizione** elementi siano soddisfatti.

Ogni condizione può essere applicata a una delle due una **Name** (il nome di una proprietà di entità del modello concettuale, specificato dal **nome** attributo), o un **ColumnName** (il nome di una colonna in il database specificato tramite il **ColumnName** attributo). Quando la **nome** attributo è impostato, la condizione viene confrontata con un valore di proprietà di entità. Quando la **ColumnName** attributo è impostato, la condizione viene confrontata con un valore di colonna. Uno solo dei **nome** oppure **ColumnName** attributo può essere specificato un **condizione** elemento.

> [!NOTE]
> Quando la **Condition** elemento viene usato all'interno di un elemento FunctionImportMapping, solo le **nome** attributo non è applicabile.

Il **condizione** elemento può essere un elemento figlio degli elementi seguenti:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

Il **condizione** elemento non può avere nessun elemento figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **condizione** elemento:

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | No          | Nome della colonna della tabella il cui valore viene utilizzato per valutare la condizione.                                                                                                                                                                                                                   |
| **IsNull**     | No          | **True** oppure **False**. Se il valore è **True** e il valore della colonna **null**, o se il valore **False** e il valore della colonna non è **null**, la condizione è true . In caso contrario, la condizione è false. <br/> Il **IsNull** e **valore** attributi non possono essere utilizzati allo stesso tempo. |
| **Valore**      | No          | Valore con cui viene confrontato il valore della colonna. Se i valori sono uguali, la condizione è true. In caso contrario, la condizione è false. <br/> Il **IsNull** e **valore** attributi non possono essere utilizzati allo stesso tempo.                                                                       |
| **Name**       | No          | Nome della proprietà dell'entità del modello concettuale il cui valore viene utilizzato per valutare la condizione. <br/> Questo attributo non è applicabile se il **condizione** elemento viene usato all'interno di un elemento FunctionImportMapping.                                                                           |

### <a name="example"></a>Esempio

L'esempio seguente illustra **Condition** gli elementi come elementi figlio del **MappingFragment** elementi. Quando **HireDate** non è null e **EnrollmentDate** è null, i dati vengono mappati tra il **SchoolModel.Instructor** tipo e il **PersonID**e **HireDate** colonne del **Person** tabella. Quando **EnrollmentDate** non è null e **HireDate** è null, i dati vengono mappati tra il **SchoolModel.Student** tipo e il **PersonID** e **Enrollment** le colonne delle **persona** tabella.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a>Elemento DeleteFunction (MSL)

Il **DeleteFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di eliminazione di un tipo di entità o associazione nel modello concettuale a una stored procedure nel database sottostante. Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione. Per altre informazioni, vedere la funzione elemento (SSDL).

> [!NOTE]
> Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction applicato a EntityTypeMapping

Quando applicato all'elemento EntityTypeMapping, il **DeleteFunction** elemento esegue il mapping della funzione di eliminazione di un tipo di entità nel modello concettuale a una stored procedure.

Il **DeleteFunction** elemento può avere i seguenti elementi figlio quando applicato a un **EntityTypeMapping** elemento:

-   AssociationEnd (zero o più)
-   Elemento ComplexProperty (zero o più)
-   ScarlarProperty (zero o più)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **DeleteFunction** elemento quando viene applicato a un **EntityTypeMapping** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nomefunzione**          | Yes         | Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata. La stored procedure deve essere dichiarata nel modello di archiviazione. |
| **RowsAffectedParameter** | No          | Nome del parametro di output che restituisce il numero di righe interessate.                                                                               |

#### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e viene illustrato il **DeleteFunction** mapping della funzione di eliminazione dell'elemento il **persona** tipo di entità per il **DeletePerson** stored procedure. Il **DeletePerson** stored procedure viene dichiarata nel modello di archiviazione.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction applicato ad AssociationSetMapping

Quando applicato all'elemento AssociationSetMapping, il **DeleteFunction** elemento esegue il mapping della funzione di eliminazione di un'associazione nel modello concettuale a una stored procedure.

Il **DeleteFunction** elemento può avere i seguenti elementi figlio quando applicato al **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **DeleteFunction** elemento quando viene applicato per il **AssociationSetMapping** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nomefunzione**          | Yes         | Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata. La stored procedure deve essere dichiarata nel modello di archiviazione. |
| **RowsAffectedParameter** | No          | Nome del parametro di output che restituisce il numero di righe interessate.                                                                               |

#### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e Mostra le **DeleteFunction** elemento usato per eseguire il mapping di funzione di eliminazione del **CourseInstructor** associazione per il  **DeleteCourseInstructor** stored procedure. Il **DeleteCourseInstructor** stored procedure viene dichiarata nel modello di archiviazione.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a>Elemento EndProperty (MSL)

Il **EndProperty** elemento specifica MSL (mapping language) definisce il mapping tra un'entità finale o una funzione di modifica di un'associazione del modello concettuale e il database sottostante. Il mapping di colonne delle proprietà viene specificato in un elemento ScalarProperty figlio.

Quando un **EndProperty** elemento viene usato per definire il mapping per l'entità finale di un'associazione del modello concettuale, è un figlio di un elemento AssociationSetMapping. Quando la **EndProperty** elemento viene usato per definire il mapping per una funzione di modifica di un'associazione del modello concettuale, è un figlio di un elemento InsertFunction o elemento DeleteFunction.

Il **EndProperty** elemento può avere elementi figlio seguenti:

-   ScalarProperty (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **EndProperty** elemento:

| Nome attributo | È obbligatorio | Valore                                                 |
|:---------------|:------------|:------------------------------------------------------|
| nome           | Yes         | Nome dell'entità finale dell'associazione di cui è in corso il mapping. |

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **AssociationSetMapping** elemento in cui il **FK\_corso\_reparto** associazione nel modello concettuale è mappato al **Course** tabella nel database. I mapping tra le colonne della tabella e proprietà del tipo di associazione vengono specificati in figlio **EndProperty** elementi.

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a>Esempio

L'esempio seguente mostra le **EndProperty** mapping di funzioni insert e delete di un'associazione tramite l'elemento (**CourseInstructor**) alle stored procedure nel database sottostante. Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a>Elemento EntityContainerMapping (MSL)

Il **EntityContainerMapping** elemento in specifica linguaggio MSL (mapping) viene mappato il contenitore di entità nel modello concettuale al contenitore di entità nel modello di archiviazione. Il **EntityContainerMapping** elemento è figlio dell'elemento di Mapping.

Il **EntityContainerMapping** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):

-   EntitySetMapping (zero o più)
-   AssociationSetMapping (zero o più)
-   FunctionImportMapping (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityContainerMapping** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Yes         | Nome del contenitore di entità del modello di archiviazione di cui è in corso il mapping.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Yes         | Nome del contenitore di entità del modello concettuale di cui è in corso il mapping.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | No          | **True** oppure **False**. Se **False**, non vengono generati alcuna visualizzazione di aggiornamento. Questo attributo deve essere impostato su **False** quando si dispone di un mapping di sola lettura che sarebbe non valido perché i dati potrebbero non eseguire il round trip correttamente. <br/> Il valore predefinito è **True**. |

### <a name="example"></a>Esempio

L'esempio seguente mostra un **EntityContainerMapping** elemento che viene eseguito il mapping di **SchoolModelEntities** contenitore (il contenitore di entità del modello concettuale) per il  **SchoolModelStoreContainer** contenitore (il contenitore di entità del modello di archiviazione):

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a>Elemento EntitySetMapping (MSL)

Il **EntitySetMapping** elemento tutti i tipi in un'entità del modello concettuale è impostati su entità di eseguire il mapping specification language (MSL) imposta il modello di archiviazione. Una set di entità nel modello concettuale è un contenitore logico per le istanze delle entità dello stesso tipo (e i tipi derivati). Una set di entità nel modello di archiviazione rappresenta una tabella o vista nel database sottostante. Il set di entità del modello concettuale è specificato dal valore della **Name** attributo del **EntitySetMapping** elemento. Il mapping alla tabella o vista specificato dal **StoreEntitySet** attributo in ogni elemento MappingFragment figlio o nel **EntitySetMapping** elemento stesso.

Il **EntitySetMapping** elemento può avere elementi figlio seguenti:

-   EntityTypeMapping (zero o più)
-   Elemento QueryView (zero o più)
-   MappingFragment (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySetMapping** elemento.

| Nome attributo           | È obbligatorio | Valore                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Yes         | Nome del set di entità del modello concettuale di cui è in corso il mapping.                                                                                                                                                             |
| **TypeName** **1**       | No          | Nome del tipo di entità del modello concettuale di cui è in corso il mapping.                                                                                                                                                            |
| **StoreEntitySet** **1** | No          | Nome del set di entità del modello di archiviazione a cui viene eseguito il mapping.                                                                                                                                                             |
| **Makecolumnsdistinct impostato**  | No          | **True** oppure **False** a seconda se vengono restituite solo righe distinte. <br/> Se questo attributo è impostato su **True**, il **GenerateUpdateViews** attributo dell'elemento EntityContainerMapping deve essere impostata su **False**. |

 

**1** il **TypeName** e **StoreEntitySet** attributi possono essere utilizzati al posto di elementi figlio EntityTypeMapping e MappingFragment per eseguire il mapping un singolo tipo di entità a una singola tabella.

### <a name="example"></a>Esempio

L'esempio seguente mostra un' **EntitySetMapping** elemento che esegue il mapping di tre tipi (un tipo di base e due tipi derivati) nel **corsi** set di entità del modello concettuale a tre tabelle diverse nel database sottostante. Le tabelle sono specificate per il **StoreEntitySet** attributo in ogni **MappingFragment** elemento.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a>Elemento EntityTypeMapping (MSL)

Il **EntityTypeMapping** elemento specifica MSL (mapping language) definisce il mapping tra un tipo di entità nel modello concettuale e tabelle o viste nel database sottostante. Per informazioni sui tipi di entità del modello concettuale e tabelle di database o viste sottostanti, vedere l'elemento EntityType (CSDL) e l'elemento EntitySet (SSDL). Viene specificato il tipo di entità del modello concettuale che viene eseguito il mapping dal **nomeTipo** attributo delle **EntityTypeMapping** elemento. La tabella o vista in cui viene eseguito il mapping viene specificato per il **StoreEntitySet** attributo dell'elemento MappingFragment figlio.

Il ModificationFunctionMapping elemento figlio può essere usato per eseguire il mapping di inserimento, aggiornamento o eliminazione di funzioni di tipi di entità alle stored procedure nel database.

Il **EntityTypeMapping** elemento può avere elementi figlio seguenti:

-   MappingFragment (zero o più)
-   ModificationFunctionMapping (zero o più)
-   ScalarProperty
-   Condizione

> [!NOTE]
> **MappingFragment** e **ModificationFunctionMapping** non può contenere elementi figlio di elementi di **EntityTypeMapping** elemento allo stesso tempo.


> [!NOTE]
> Il **ScalarProperty** e **condizione** elementi possono essere solo elementi figlio del **EntityTypeMapping** elemento quando viene usata all'interno di un elemento FunctionImportMapping.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityTypeMapping** elemento.

| Nome attributo | È obbligatorio | Valore                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | Yes         | Nome completo dello spazio dei nomi del tipo di entità del modello concettuale di cui è in corso il mapping. <br/> Se il tipo è astratto o un tipo derivato, il valore deve essere `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Esempio

L'esempio seguente illustra un elemento EntitySetMapping con due figlio **EntityTypeMapping** elementi. Nel primo **EntityTypeMapping** elemento, il **SchoolModel.Person** viene eseguito il mapping di tipo di entità per il **persona** tabella. Nella seconda **EntityTypeMapping** dell'elemento, la funzionalità di aggiornamento il **SchoolModel.Person** tipo viene mappato a una stored procedure **UpdatePerson**, nel database .

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Esempio

Nell'esempio successivo viene mostrato il mapping di una gerarchia di tipo in cui il tipo radice è astratto. Si noti l'uso del `IsOfType` sintassi per il **TypeName** attributi.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a>Elemento FunctionImportMapping (MSL)

Il **FunctionImportMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure o funzione nel database sottostante. Le importazioni di funzioni devono essere dichiarate nel modello concettuale mentre le stored procedure nel modello di archiviazione. Per altre informazioni, vedere l'elemento FunctionImport (CSDL) e funzione elemento (SSDL).

> [!NOTE]
> Per impostazione predefinita, se un'importazione di funzioni restituisce un tipo di entità del modello concettuale o un tipo complesso, i nomi delle colonne restituiti dalla stored procedure sottostante devono corrispondere esattamente ai nomi delle proprietà nel tipo di modello concettuale. Se i nomi delle colonne non corrispondono esattamente ai nomi delle proprietà, il mapping deve essere definito in un elemento ResultMapping.

Il **FunctionImportMapping** elemento può avere elementi figlio seguenti:

-   ResultMapping (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **FunctionImportMapping** elemento:

| Nome attributo         | È obbligatorio | Valore                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Yes         | Nome dell'importazione di funzioni nel modello concettuale di cui è in corso il mapping.           |
| **Nomefunzione**       | Yes         | Nome completo dello spazio dei nomi della funzione nel modello di archiviazione di cui è in corso il mapping. |

### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School. Si consideri la funzione seguente nel modello di archiviazione:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Si consideri anche questa importazione di funzioni nel modello concettuale:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

L'esempio seguente viene mostrato un **FunctionImportMapping** elemento usato per eseguire il mapping di funzione e importazione sopra tra loro di funzioni:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Elemento InsertFunction (MSL)

Il **InsertFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di inserimento di un tipo di entità o associazione nel modello concettuale a una stored procedure nel database sottostante. Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione. Per altre informazioni, vedere la funzione elemento (SSDL).

> [!NOTE]
> Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.

Il **InsertFunction** elemento può essere un figlio dell'elemento ModificationFunctionMapping e applicato a EntityTypeMapping (elemento) o l'elemento AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction applicato a EntityTypeMapping

Quando applicato all'elemento EntityTypeMapping, il **InsertFunction** elemento esegue il mapping della funzione di inserimento di un tipo di entità nel modello concettuale a una stored procedure.

Il **InsertFunction** elemento può avere i seguenti elementi figlio quando applicato a un **EntityTypeMapping** elemento:

-   AssociationEnd (zero o più)
-   Elemento ComplexProperty (zero o più)
-   ResultBinding (zero o più)
-   ScarlarProperty (zero o più)

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **InsertFunction** elemento quando viene applicato a un **EntityTypeMapping** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nomefunzione**          | Yes         | Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata. La stored procedure deve essere dichiarata nel modello di archiviazione. |
| **RowsAffectedParameter** | No          | Nome del parametro di output che restituisce il numero di righe interessate.                                                                               |

#### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e viene illustrato il **InsertFunction** elemento usato per eseguire il mapping di funzione di inserimento del tipo di entità Person al **InsertPerson** stored procedure. Il **InsertPerson** stored procedure viene dichiarata nel modello di archiviazione.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction applicato ad AssociationSetMapping

Quando applicato all'elemento AssociationSetMapping, il **InsertFunction** elemento esegue il mapping della funzione di inserimento di un'associazione nel modello concettuale a una stored procedure.

Il **InsertFunction** elemento può avere i seguenti elementi figlio quando applicato al **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **InsertFunction** elemento quando viene applicato per il **AssociationSetMapping** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nomefunzione**          | Yes         | Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata. La stored procedure deve essere dichiarata nel modello di archiviazione. |
| **RowsAffectedParameter** | No          | Nome del parametro di output che restituisce il numero di righe interessate.                                                                               |

#### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e viene illustrato il **InsertFunction** elemento usato per eseguire il mapping di funzione di inserimento del **CourseInstructor** associazione per il  **InsertCourseInstructor** stored procedure. Il **InsertCourseInstructor** stored procedure viene dichiarata nel modello di archiviazione.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a>Elemento Mapping (MSL)

Il **Mapping** elemento in specifica linguaggio MSL (mapping) contiene informazioni per il mapping tra gli oggetti definiti in un modello concettuale a un database (come descritto in un modello di archiviazione). Per altre informazioni, vedere Specifica CSDL e SSDL Specification.

Il **Mapping** è l'elemento radice per una specifica di mapping. Lo spazio dei nomi XML per il mapping di specifiche http://schemas.microsoft.com/ado/2009/11/mapping/cs .

L'elemento Mapping può includere i seguenti elementi figlio (nell'ordine elencato):

-   Alias (zero o più)
-   EntityContainerMapping (esattamente un elemento)

I nomi di tipi di modelli concettuale e di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi. Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere l'elemento Schema (CSDL). Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere l'elemento Schema (SSDL). Gli alias degli spazi dei nomi utilizzati in MSL possono essere definiti con l'elemento Alias.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Mapping** elemento.

| Nome attributo | È obbligatorio | Valore                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Barra spaziatrice**      | Yes         | **C-S**. Si tratta di un valore fisso e non può essere modificato. |

### <a name="example"></a>Esempio

L'esempio seguente mostra una **Mapping** elemento basato su parte del modello School. Per altre informazioni sul modello School, vedere Guida introduttiva (Entity Framework):

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a>Elemento MappingFragment (MSL)

Il **MappingFragment** elemento specifica MSL (mapping language) definisce il mapping tra le proprietà di un tipo di entità del modello concettuale e una tabella o vista nel database. Per informazioni sui tipi di entità del modello concettuale e tabelle di database o viste sottostanti, vedere l'elemento EntityType (CSDL) e l'elemento EntitySet (SSDL). Il **MappingFragment** può essere un elemento figlio dell'elemento EntityTypeMapping o l'elemento EntitySetMapping.

Il **MappingFragment** elemento può avere elementi figlio seguenti:

-   ComplexType (zero o più)
-   ScalarProperty (zero o più)
-   Condizione (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **MappingFragment** elemento.

| Nome attributo          | È obbligatorio | Valore                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Yes         | Nome della tabella o visualizzazione di cui è in corso il mapping.                                                                                                                                                                           |
| **Makecolumnsdistinct impostato** | No          | **True** oppure **False** a seconda se vengono restituite solo righe distinte. <br/> Se questo attributo è impostato su **True**, il **GenerateUpdateViews** attributo dell'elemento EntityContainerMapping deve essere impostata su **False**. |

### <a name="example"></a>Esempio

L'esempio seguente mostra una **MappingFragment** elemento come figlio di un **EntityTypeMapping** elemento. In questo esempio, le proprietà del **Course** tipo nel modello concettuale vengono mappate alle colonne delle **corso** tabella nel database.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Esempio

L'esempio seguente mostra una **MappingFragment** elemento come figlio di un **EntitySetMapping** elemento. Come illustrato nell'esempio precedente, le proprietà del **Course** tipo nel modello concettuale vengono mappate alle colonne delle **corso** tabella nel database.

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a>Elemento ModificationFunctionMapping (MSL)

Il **ModificationFunctionMapping** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping di inserimento, aggiornamento ed eliminazione di funzioni di un tipo di entità del modello concettuale alle stored procedure nel database sottostante. Il **ModificationFunctionMapping** elemento può anche eseguire il mapping di inserimento ed eliminare le funzioni per le associazioni molti-a-molti nel modello concettuale alle stored procedure nel database sottostante. Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione. Per altre informazioni, vedere la funzione elemento (SSDL).

> [!NOTE]
> Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.


> [!NOTE]
> Se esiste un mapping tra le funzioni di modifica per un'entità in una gerarchia di ereditarietà e stored procedure, è necessario eseguire il mapping delle funzioni di modifica per tutti i tipi nella gerarchia a stored procedure.

Il **ModificationFunctionMapping** elemento può essere un figlio dell'elemento EntityTypeMapping o AssociationSetMapping (elemento).

Il **ModificationFunctionMapping** elemento può avere elementi figlio seguenti:

-   DeleteFunction (zero o più)
-   InsertFunction (zero o più)
-   UpdateFunction (zero o più)

Attributi non sono applicabili per il **ModificationFunctionMapping** elemento.

### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato il mapping per il **persone** set di entità nel modello School. Oltre al mapping di colonna per il **Person** tipo di entità, il mapping dell'inserimento, aggiornamento ed eliminazione di funzioni delle **persona** tipo vengono visualizzati. Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato il mapping per il **CourseInstructor** set di associazioni nel modello School. Oltre al mapping di colonna per il **CourseInstructor** associazione, il mapping delle funzioni insert e delete delle **CourseInstructor** associazione vengono visualizzati. Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a>Elemento QueryView (MSL)

Il **QueryView** elemento specifica MSL (mapping language) definisce un mapping di sola lettura tra un tipo di entità o associazione nel modello concettuale e una tabella nel database sottostante. Il mapping viene definito con una query Entity SQL che viene valutata in base al modello di archiviazione e si esprime il risultato impostato in termini di entità o associazione nel modello concettuale. Poiché le visualizzazioni di query sono di sola lettura, non è possibile utilizzare comandi di aggiornamento standard per aggiornare i tipi definiti da tali visualizzazioni. A tale scopo è possibile utilizzare le funzioni di modifica. Per altre informazioni, vedere Procedura: mapping delle funzioni di modifica alle Stored procedure.

> [!NOTE]
> Nel **QueryView** dell'elemento, le espressioni Entity SQL che contengono **GroupBy**, aggregazioni di gruppo o le proprietà di navigazione non sono supportati.

 

Il **QueryView** elemento può essere un figlio dell'elemento EntitySetMapping o AssociationSetMapping (elemento). Nel primo caso, la visualizzazione di query definisce un mapping di sola lettura per un'entità nel modello concettuale. Nel secondo caso, la visualizzazione di query definisce un mapping di sola lettura per un'associazione nel modello concettuale.

> [!NOTE]
> Se il **AssociationSetMapping** elemento è un'associazione con un vincolo referenziale, il **AssociationSetMapping** elemento viene ignorato. Per altre informazioni, vedere l'elemento ReferentialConstraint (CSDL).

Il **QueryView** elemento non può avere elementi figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **QueryView** elemento.

| Nome attributo | È obbligatorio | Valore                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | No          | Nome del tipo di modello concettuale mappato dalla visualizzazione di query. |

### <a name="example"></a>Esempio

Nell'esempio seguente il **QueryView** come figlio dell'elemento il **EntitySetMapping** elemento e viene definito un mapping di visualizzazione di query per il **reparto** tipo di entità nel Modello School.

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

Poiché la query restituisce solo un sottoinsieme dei membri del **reparto** tipo nel modello di archiviazione, il **reparto** tipo nel modello School è stato modificato base al mapping come indicato di seguito:

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a>Esempio

L'esempio seguente mostra la **QueryView** elemento come figlio di un **AssociationSetMapping** elemento e viene definito un mapping di sola lettura per il `FK_Course_Department` associazione nel modello School.

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a>Commenti

È possibile definire le visualizzazioni di query per consentire gli scenari seguenti:

-   Definire un'entità nel modello concettuale che non includa tutte le proprietà dell'entità nel modello di archiviazione. Sono incluse proprietà che non dispongono di valori predefiniti e non supportano **null** valori.
-   Eseguire il mapping delle colonne calcolate nel modello di archiviazione alle proprietà dei tipi di entità nel modello concettuale.
-   Definire un mapping in cui le condizioni utilizzate per partizionare le entità nel modello concettuale non siano basate sull'uguaglianza. Quando si specifica un mapping condizionale utilizzando il **condizione** elemento, la condizione fornita deve essere uguale al valore specificato. Per altre informazioni, vedere l'elemento Condition (MSL).
-   Eseguire il mapping della stessa colonna nel modello di archiviazione a più tipi nel modello concettuale.
-   Eseguire il mapping di più tipi alla stessa tabella.
-   Definire associazioni nel modello concettuale che non siano basate sulle chiavi esterne dello schema relazionale.
-   Utilizzare la logica di business personalizzata per impostare il valore delle proprietà nel modello concettuale. Ad esempio, è possibile mappare il valore di stringa "T" nell'origine dati su un valore di **true**, un valore booleano, nel modello concettuale.
-   Definire filtri condizionali per i risultati della query.
-   Applicare ai dati del modello concettuale un numero di restrizioni minore di quelle applicate al modello di archiviazione. Ad esempio, è possibile impostare una proprietà nel modello concettuale che ammette valori null anche se la colonna a cui viene mappata non supporta **null**valori.

Le considerazioni seguenti riguardano la definizione delle visualizzazioni di query per le entità:

-   Le visualizzazioni di query sono di sola lettura. È possibile applicare aggiornamenti alle entità solo utilizzando le funzioni di modifica.
-   Quando viene definito un tipo di entità da una visualizzazione di query, è necessario definire anche tutte le entità correlate dalle visualizzazioni di query.
-   Quando si esegue il mapping di un'associazione molti-a-molti a un'entità nel modello di archiviazione che rappresenta una tabella dei collegamenti nello schema relazionale, è necessario definire un **QueryView** elemento le **AssociationSetMapping** elemento per la tabella.
-   È necessario definire le visualizzazioni di query per tutti i tipi di una gerarchia di tipi. È possibile eseguire questa operazione nei modi seguenti:
-   -   Con un unico **QueryView** elemento che specifica una singola query Entity SQL che restituisce l'unione di tutti i tipi di entità nella gerarchia.
    -   Con un unico **QueryView** elemento che specifica una singola query Entity SQL che usa l'operatore CASE per restituire un tipo di entità specifico nella gerarchia basata su una condizione specifica.
    -   Con un'ulteriore **QueryView** (elemento) per un tipo specifico nella gerarchia. In questo caso, usare il **nomeTipo** attributo delle **QueryView** elemento per specificare il tipo di entità per ogni visualizzazione.
-   Quando si definisce una visualizzazione di query, è possibile specificare il **StorageSetName** attributo il **EntitySetMapping** elemento.
-   Quando si definisce una visualizzazione di query, il **EntitySetMapping**elemento non può contenere anche **proprietà** mapping.

## <a name="resultbinding-element-msl"></a>Elemento ResultBinding (MSL)

Il **ResultBinding** elemento in specifica linguaggio MSL (mapping) esegue il mapping di valori di colonna restituiti dalle stored procedure alle proprietà dell'entità nel modello concettuale quando le funzioni di modifica tipo di entità vengono eseguito il mapping alla stored procedure nel database sottostante. Ad esempio, quando il valore di una colonna identity viene restituito da un'istruzione insert stored procedure, il **ResultBinding** elemento esegue il mapping del valore restituito a una proprietà del tipo di entità nel modello concettuale.

Il **ResultBinding** elemento può essere figlio dell'elemento InsertFunction o l'elemento UpdateFunction.

Il **ResultBinding** elemento non può avere elementi figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ResultBinding** elemento:

| Nome attributo | È obbligatorio | Valore                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Yes         | Nome della proprietà di entità nel modello concettuale di cui è in corso il mapping. |
| **ColumnName** | Yes         | Nome della colonna di cui viene eseguito il mapping.                                          |

### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e Mostra un **InsertFunction** elemento usato per eseguire il mapping della funzione di inserimento delle **persona** tipo di entità per il **InsertPerson** stored procedure. (Il **InsertPerson** stored procedure viene illustrata di seguito e viene dichiarata nel modello di archiviazione.) Oggetto **ResultBinding** elemento viene usato per eseguire il mapping di un valore di colonna che viene restituito dalla stored procedure (**NewPersonID**) a una proprietà di tipo di entità (**PersonID**).

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

L'istruzione Transact-SQL seguente viene descritto il **InsertPerson** stored procedure:

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a>Elemento ResultMapping (MSL)

Il **ResultMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nel database sottostante quando vengono soddisfatte le seguenti:

-   L'operazione di importazione di funzioni restituisce un tipo di entità del modello concettuale.
-   I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo di entità o del tipo complesso.

Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo di entità o un tipo complesso è basato sui nomi delle colonne e delle proprietà. Se i nomi delle colonne non corrispondono esattamente i nomi delle proprietà, è necessario usare il **ResultMapping** elemento per definire il mapping. Per un esempio del mapping predefinito, vedere l'elemento FunctionImportMapping (MSL).

Il **ResultMapping** è un elemento figlio dell'elemento FunctionImportMapping.

Il **ResultMapping** elemento può avere elementi figlio seguenti:

-   EntityTypeMapping (zero o più)
-   ComplexTypeMapping

Attributi non sono applicabili per il **ResultMapping** elemento.

### <a name="example"></a>Esempio

Si consideri la seguente stored procedure:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Si consideri inoltre il tipo di entità del modello concettuale seguente:

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

Per creare un'importazione di funzioni che restituisce istanze del tipo di entità precedente, il mapping tra le colonne restituite dalla stored procedure e il tipo di entità deve essere definito un **ResultMapping** elemento:

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a>Elemento ScalarProperty (MSL)

Il **ScalarProperty** elemento in specifica linguaggio MSL (mapping) esegue il mapping di una proprietà di un tipo di entità del modello concettuale, al tipo complesso o associazione a una colonna della tabella o un parametro della stored procedure nel database sottostante.

> [!NOTE]
> Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione. Per altre informazioni, vedere la funzione elemento (SSDL).

Il **ScalarProperty** elemento può essere un elemento figlio degli elementi seguenti:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Come elemento figlio di **MappingFragment**, **ComplexProperty**, o **EndProperty** elemento, il **ScalarProperty** elemento esegue il mapping di una proprietà nel modello concettuale a una colonna nel database. Come elemento figlio di **InsertFunction**, **UpdateFunction**, o **DeleteFunction** elemento, il **ScalarProperty** elemento esegue il mapping di una proprietà nel modello concettuale a un parametro della stored procedure.

Il **ScalarProperty** elemento non può avere elementi figlio.

### <a name="applicable-attributes"></a>Attributi applicabili

Gli attributi che si applicano per la **ScalarProperty** elemento differiscono in base al ruolo dell'elemento.

Nella tabella seguente vengono descritti gli attributi che sono applicabili quando la **ScalarProperty** elemento viene usato per eseguire il mapping di una proprietà del modello concettuale a una colonna nel database:

| Nome attributo | È obbligatorio | Valore                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Yes         | Nome della proprietà del modello concettuale di cui è in corso il mapping. |
| **ColumnName** | Yes         | Nome della colonna della tabella di cui è in corso il mapping.              |

Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ScalarProperty** elemento quando viene usato per eseguire il mapping di una proprietà del modello concettuale a un parametro della stored procedure:

| Nome attributo    | È obbligatorio | Valore                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Yes         | Nome della proprietà del modello concettuale di cui è in corso il mapping.                                                                                 |
| **ParameterName** | Yes         | Nome del parametro di cui è in corso il mapping.                                                                                                 |
| **Version**       | No          | **Corrente** oppure **originale** a seconda che il valore corrente o il valore originale della proprietà deve essere usato per le verifiche della concorrenza. |

### <a name="example"></a>Esempio

L'esempio seguente mostra le **ScalarProperty** elemento usato in due modi:

-   Per mappare le proprietà del **Person** tipo di entità alle colonne della **persona**tabella.
-   Per mappare le proprietà del **persona** ai parametri del tipo di entità la **UpdatePerson** stored procedure. Le stored procedure vengono dichiarate nel modello di archiviazione.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Esempio

L'esempio seguente mostra le **ScalarProperty** elemento usato per eseguire il mapping di inserimento ed eliminazione di funzioni di un'associazione del modello concettuale alle stored procedure nel database. Le stored procedure vengono dichiarate nel modello di archiviazione.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a>Elemento UpdateFunction (MSL)

Il **UpdateFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di aggiornamento di un tipo di entità nel modello concettuale a una stored procedure nel database sottostante. Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione. Per altre informazioni, vedere la funzione elemento (SSDL).

> [!NOTE]
>  Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.

Il **UpdateFunction** elemento può essere un figlio dell'elemento ModificationFunctionMapping e applicata all'elemento EntityTypeMapping.

Il **UpdateFunction** elemento può avere elementi figlio seguenti:

-   AssociationEnd (zero o più)
-   Elemento ComplexProperty (zero o più)
-   ResultBinding (zero o più)
-   ScarlarProperty (zero o più)

### <a name="applicable-attributes"></a>Attributi applicabili

Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **UpdateFunction** elemento.

| Nome attributo            | È obbligatorio | Valore                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nomefunzione**          | Yes         | Nome completo dello spazio dei nomi della stored procedure a cui la funzione di aggiornamento viene mappata. La stored procedure deve essere dichiarata nel modello di archiviazione. |
| **RowsAffectedParameter** | No          | Nome del parametro di output che restituisce il numero di righe interessate.                                                                               |

### <a name="example"></a>Esempio

Nell'esempio seguente si basa sul modello School e viene illustrato il **UpdateFunction** elemento usato per eseguire il mapping di funzione di aggiornamento del **persona** tipo di entità per il **UpdatePerson** stored procedure. Il **UpdatePerson** stored procedure viene dichiarata nel modello di archiviazione.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
