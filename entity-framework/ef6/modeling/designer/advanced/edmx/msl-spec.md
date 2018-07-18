---
title: Specifica MSL - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121161"
---
# <a name="msl-specification"></a><span data-ttu-id="b848e-102">Specifica MSL</span><span class="sxs-lookup"><span data-stu-id="b848e-102">MSL Specification</span></span>
<span data-ttu-id="b848e-103">Linguaggio di specifica di mapping (MSL) è un linguaggio basato su XML che descrive il mapping tra il modello concettuale e il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b848e-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="b848e-104">In un'applicazione Entity Framework, i metadati di mapping vengono caricati da un file con estensione MSL (scritto nel linguaggio MSL) in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="b848e-105">Entity Framework Usa i metadati di mapping in fase di esecuzione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="b848e-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="b848e-106">Entity Framework Designer (Entity Framework Designer) archivia le informazioni di mapping in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="b848e-107">In fase di compilazione, Entity Designer utilizza le informazioni in un file con estensione edmx per creare il file con estensione MSL che serve da Entity Framework in fase di esecuzione</span><span class="sxs-lookup"><span data-stu-id="b848e-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="b848e-108">I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b848e-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="b848e-109">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere [specifica CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="b848e-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="b848e-110">Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="b848e-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="b848e-111">Le versioni di MSL si differenziano dagli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="b848e-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="b848e-112">Versione MSL</span><span class="sxs-lookup"><span data-stu-id="b848e-112">MSL Version</span></span> | <span data-ttu-id="b848e-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="b848e-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="b848e-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="b848e-114">MSL v1</span></span>      | <span data-ttu-id="b848e-115">urn: schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="b848e-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="b848e-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="b848e-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="b848e-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="b848e-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="b848e-118">Elemento Alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-118">Alias Element (MSL)</span></span>

<span data-ttu-id="b848e-119">Il **Alias** elemento specifica linguaggio MSL (mapping) è un figlio dell'elemento di Mapping che viene usato per definire gli alias spazi dei nomi del modello concettuali di modello e di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="b848e-120">I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b848e-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="b848e-121">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere l'elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="b848e-122">Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere l'elemento Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="b848e-123">Il **Alias** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-124">Applicable Attributes</span></span>

<span data-ttu-id="b848e-125">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Alias** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="b848e-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-126">Attribute Name</span></span> | <span data-ttu-id="b848e-127">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-127">Is Required</span></span> | <span data-ttu-id="b848e-128">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="b848e-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="b848e-129">**Key**</span></span>        | <span data-ttu-id="b848e-130">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-130">Yes</span></span>         | <span data-ttu-id="b848e-131">L'alias dello spazio dei nomi specificato per il **valore** attributo.</span><span class="sxs-lookup"><span data-stu-id="b848e-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="b848e-132">**Valore**</span><span class="sxs-lookup"><span data-stu-id="b848e-132">**Value**</span></span>      | <span data-ttu-id="b848e-133">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-133">Yes</span></span>         | <span data-ttu-id="b848e-134">Lo spazio dei nomi per il quale il valore della **chiave** elemento è un alias.</span><span class="sxs-lookup"><span data-stu-id="b848e-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="b848e-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-135">Example</span></span>

<span data-ttu-id="b848e-136">L'esempio seguente mostra un' **Alias** elemento che definisce un alias, `c`, per i tipi definiti nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="b848e-137">Elemento AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="b848e-138">Il **AssociationEnd** elemento in specifica linguaggio MSL (mapping) viene usato quando le funzioni di modifica di un tipo di entità nel modello concettuale vengono mappate alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="b848e-139">Se una modifica stored procedure accetta un parametro il cui valore è contenuto in una proprietà di associazione, il **AssociationEnd** elemento viene mappato il valore della proprietà al parametro.</span><span class="sxs-lookup"><span data-stu-id="b848e-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="b848e-140">Per ulteriori informazioni, vedere l'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="b848e-140">For more information, see the example below.</span></span>

<span data-ttu-id="b848e-141">Per altre informazioni sull'esecuzione del mapping delle funzioni di modifica dei tipi di entità alle stored procedure, vedere l'elemento ModificationFunctionMapping (MSL) e la procedura dettagliata: Mapping di un'entità alle Stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="b848e-142">Il **AssociationEnd** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-144">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-144">Applicable Attributes</span></span>

<span data-ttu-id="b848e-145">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **AssociationEnd** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="b848e-146">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-146">Attribute Name</span></span>     | <span data-ttu-id="b848e-147">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-147">Is Required</span></span> | <span data-ttu-id="b848e-148">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-149">**Elemento AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="b848e-149">**AssociationSet**</span></span> | <span data-ttu-id="b848e-150">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-150">Yes</span></span>         | <span data-ttu-id="b848e-151">Nome dell'associazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="b848e-152">**From**</span><span class="sxs-lookup"><span data-stu-id="b848e-152">**From**</span></span>           | <span data-ttu-id="b848e-153">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-153">Yes</span></span>         | <span data-ttu-id="b848e-154">Il valore della **FromRole** attributo della proprietà di navigazione che corrisponde all'associazione in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="b848e-155">Per altre informazioni, vedere l'elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="b848e-156">**Per**</span><span class="sxs-lookup"><span data-stu-id="b848e-156">**To**</span></span>             | <span data-ttu-id="b848e-157">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-157">Yes</span></span>         | <span data-ttu-id="b848e-158">Il valore della **ToRole** attributo della proprietà di navigazione che corrisponde all'associazione in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="b848e-159">Per altre informazioni, vedere l'elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="b848e-160">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-160">Example</span></span>

<span data-ttu-id="b848e-161">Si consideri il tipo di entità del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="b848e-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="b848e-162">Si consideri inoltre la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="b848e-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="b848e-163">Per eseguire il mapping della funzione di aggiornamento della `Course` entità a questa stored procedure, è necessario specificare un valore per il **DepartmentID** parametro.</span><span class="sxs-lookup"><span data-stu-id="b848e-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="b848e-164">Il valore di `DepartmentID` non corrisponde a una proprietà sul tipo di entità. È contenuto in un'associazione indipendente il cui mapping è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b848e-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="b848e-165">Il codice seguente illustra il **AssociationEnd** elemento usato per eseguire il mapping di **DepartmentID** proprietà del **FK\_corso\_reparto** associazione per il **UpdateCourse** stored procedure di (a cui la funzione di aggiornamento del **corso** viene mappato il tipo di entità):</span><span class="sxs-lookup"><span data-stu-id="b848e-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="b848e-166">Elemento AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-167">Il **AssociationSetMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'associazione concettuale colonne modello e una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="b848e-168">Le associazioni del modello concettuale sono tipi le cui proprietà rappresentano colonne di chiavi primarie ed esterne nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="b848e-169">Il **AssociationSetMapping** elemento utilizza due elementi EndProperty per definire i mapping tra le proprietà di tipo di associazione e le colonne nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="b848e-170">È possibile impostare condizioni su questi mapping con l'elemento Condition.</span><span class="sxs-lookup"><span data-stu-id="b848e-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="b848e-171">Eseguire il mapping di insert, update e delete funzioni per le associazioni alle stored procedure nel database con l'elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="b848e-172">Definire i mapping di sola lettura tra associazioni e le colonne della tabella utilizzando una stringa Entity SQL in un elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="b848e-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-173">Se per un'associazione nel modello concettuale viene definito un vincolo referenziale, l'associazione non è necessario eseguire il mapping con un **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="b848e-174">Se un' **AssociationSetMapping** elemento è presente un'associazione con un vincolo referenziale, i mapping definiti nel **AssociationSetMapping** elemento verrà ignorato.</span><span class="sxs-lookup"><span data-stu-id="b848e-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="b848e-175">Per altre informazioni, vedere l'elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="b848e-176">Il **AssociationSetMapping** elemento può avere i seguenti elementi figlio</span><span class="sxs-lookup"><span data-stu-id="b848e-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="b848e-177">Elemento QueryView (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="b848e-178">EndProperty (zero o due)</span><span class="sxs-lookup"><span data-stu-id="b848e-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="b848e-179">Condizione (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="b848e-180">ModificationFunctionMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-181">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-181">Applicable Attributes</span></span>

<span data-ttu-id="b848e-182">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="b848e-183">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-183">Attribute Name</span></span>     | <span data-ttu-id="b848e-184">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-184">Is Required</span></span> | <span data-ttu-id="b848e-185">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-186">**Name**</span></span>           | <span data-ttu-id="b848e-187">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-187">Yes</span></span>         | <span data-ttu-id="b848e-188">Nome del set di associazioni del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="b848e-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="b848e-189">**TypeName**</span></span>       | <span data-ttu-id="b848e-190">No</span><span class="sxs-lookup"><span data-stu-id="b848e-190">No</span></span>          | <span data-ttu-id="b848e-191">Nome completo dello spazio dei nomi del tipo di associazione del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="b848e-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="b848e-192">**StoreEntitySet**</span></span> | <span data-ttu-id="b848e-193">No</span><span class="sxs-lookup"><span data-stu-id="b848e-193">No</span></span>          | <span data-ttu-id="b848e-194">Nome della tabella di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="b848e-195">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-195">Example</span></span>

<span data-ttu-id="b848e-196">L'esempio seguente mostra un' **AssociationSetMapping** elemento in cui il **FK\_corso\_reparto** set di associazioni nel modello concettuale sono mappato al  **Corso** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="b848e-197">I mapping tra le colonne della tabella e proprietà del tipo di associazione vengono specificati in figlio **EndProperty** elementi.</span><span class="sxs-lookup"><span data-stu-id="b848e-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="b848e-198">Elemento ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="b848e-199">Oggetto **ComplexProperty** elemento specifica MSL (mapping language) definisce il mapping tra una proprietà di tipo complesso in un modello concettuale entità tabella e tipo di colonne nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="b848e-200">I mapping di colonne delle proprietà sono specificati negli elementi ScalarProperty figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="b848e-201">Il **ComplexType** property (elemento) può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-202">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="b848e-203">**Elemento ComplexProperty** (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="b848e-204">ComplextTypeMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="b848e-205">Condizione (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-206">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-206">Applicable Attributes</span></span>

<span data-ttu-id="b848e-207">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ComplexProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="b848e-208">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-208">Attribute Name</span></span> | <span data-ttu-id="b848e-209">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-209">Is Required</span></span> | <span data-ttu-id="b848e-210">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-211">**Name**</span></span>       | <span data-ttu-id="b848e-212">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-212">Yes</span></span>         | <span data-ttu-id="b848e-213">Nome della proprietà complessa di un tipo di entità nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="b848e-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="b848e-214">**TypeName**</span></span>   | <span data-ttu-id="b848e-215">No</span><span class="sxs-lookup"><span data-stu-id="b848e-215">No</span></span>          | <span data-ttu-id="b848e-216">Nome qualificato di spazio dei nomi del tipo di proprietà del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="b848e-217">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-217">Example</span></span>

<span data-ttu-id="b848e-218">Nell'esempio seguente si basa sul modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-218">The following example is based on the School model.</span></span> <span data-ttu-id="b848e-219">Il tipo complesso seguente è stato aggiunto al modello concettuale:</span><span class="sxs-lookup"><span data-stu-id="b848e-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="b848e-220">Il **LastName** e **FirstName** le proprietà del **persona** tipo di entità sono stati sostituiti con una proprietà complessa, **nome**:</span><span class="sxs-lookup"><span data-stu-id="b848e-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="b848e-221">MSL seguente il **ComplexProperty** elemento usato per eseguire il mapping di **nome** proprietà alle colonne nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="b848e-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="b848e-222">Elemento ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-223">Il **ComplexTypeMapping** elemento in specifica linguaggio MSL (mapping) è un figlio dell'elemento ResultMapping e definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nell'oggetto sottostante database quando vengono soddisfatte le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="b848e-224">Un tipo complesso concettuale viene restituito dall'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="b848e-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="b848e-225">I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="b848e-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="b848e-226">Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo complesso è basato sui nomi delle colonne e delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="b848e-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="b848e-227">Se i nomi delle colonne non corrispondono esattamente i nomi delle proprietà, è necessario usare il **ComplexTypeMapping** elemento per definire il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="b848e-228">Per un esempio del mapping predefinito, vedere l'elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="b848e-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="b848e-229">Il **ComplexTypeMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-230">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-231">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-231">Applicable Attributes</span></span>

<span data-ttu-id="b848e-232">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ComplexTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="b848e-233">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-233">Attribute Name</span></span> | <span data-ttu-id="b848e-234">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-234">Is Required</span></span> | <span data-ttu-id="b848e-235">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="b848e-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="b848e-236">**TypeName**</span></span>   | <span data-ttu-id="b848e-237">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-237">Yes</span></span>         | <span data-ttu-id="b848e-238">Nome completo dello spazio dei nomi del tipo complesso di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-239">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-239">Example</span></span>

<span data-ttu-id="b848e-240">Si consideri la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="b848e-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="b848e-241">Si consideri anche il tipo complesso del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="b848e-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="b848e-242">Per creare un'importazione di funzioni che restituisce istanze del tipo complesso precedente, il mapping tra le colonne restituite dalla stored procedure e il tipo di entità deve essere definito un **ComplexTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="b848e-243">Elemento Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-243">Condition Element (MSL)</span></span>

<span data-ttu-id="b848e-244">Il **condizione** elemento (MSL) mapping specification language stabilire le condizioni per i mapping tra il modello concettuale e il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="b848e-245">Il mapping definito all'interno di un nodo XML non è valide se tutte le condizioni, come specificato nell'elemento figlio **condizione** elementi, siano soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="b848e-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="b848e-246">In caso contrario, il mapping non è valido.</span><span class="sxs-lookup"><span data-stu-id="b848e-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="b848e-247">Ad esempio, se un elemento MappingFragment contiene uno o più **condizione** gli elementi figlio, il mapping definito all'interno la **MappingFragment** nodo sarà valido solo se tutte le condizioni dell'elemento figlio  **Condizione** elementi siano soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="b848e-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="b848e-248">Ogni condizione può essere applicata a una delle due una **Name** (il nome di una proprietà di entità del modello concettuale, specificato dal **nome** attributo), o un **ColumnName** (il nome di una colonna in il database specificato tramite il **ColumnName** attributo).</span><span class="sxs-lookup"><span data-stu-id="b848e-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="b848e-249">Quando la **nome** attributo è impostato, la condizione viene confrontata con un valore di proprietà di entità.</span><span class="sxs-lookup"><span data-stu-id="b848e-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="b848e-250">Quando la **ColumnName** attributo è impostato, la condizione viene confrontata con un valore di colonna.</span><span class="sxs-lookup"><span data-stu-id="b848e-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="b848e-251">Uno solo dei **nome** oppure **ColumnName** attributo può essere specificato un **condizione** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-252">Quando la **Condition** elemento viene usato all'interno di un elemento FunctionImportMapping, solo le **nome** attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="b848e-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="b848e-253">Il **condizione** elemento può essere un elemento figlio degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="b848e-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="b848e-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-255">ComplexProperty</span></span>
-   <span data-ttu-id="b848e-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-256">EntitySetMapping</span></span>
-   <span data-ttu-id="b848e-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="b848e-257">MappingFragment</span></span>
-   <span data-ttu-id="b848e-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-258">EntityTypeMapping</span></span>

<span data-ttu-id="b848e-259">Il **condizione** elemento non può avere nessun elemento figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-260">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-260">Applicable Attributes</span></span>

<span data-ttu-id="b848e-261">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **condizione** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="b848e-262">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-262">Attribute Name</span></span> | <span data-ttu-id="b848e-263">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-263">Is Required</span></span> | <span data-ttu-id="b848e-264">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="b848e-265">**ColumnName**</span></span> | <span data-ttu-id="b848e-266">No</span><span class="sxs-lookup"><span data-stu-id="b848e-266">No</span></span>          | <span data-ttu-id="b848e-267">Nome della colonna della tabella il cui valore viene utilizzato per valutare la condizione.</span><span class="sxs-lookup"><span data-stu-id="b848e-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="b848e-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="b848e-268">**IsNull**</span></span>     | <span data-ttu-id="b848e-269">No</span><span class="sxs-lookup"><span data-stu-id="b848e-269">No</span></span>          | <span data-ttu-id="b848e-270">**True** oppure **False**.</span><span class="sxs-lookup"><span data-stu-id="b848e-270">**True** or **False**.</span></span> <span data-ttu-id="b848e-271">Se il valore è **True** e il valore della colonna **null**, o se il valore **False** e il valore della colonna non è **null**, la condizione è true .</span><span class="sxs-lookup"><span data-stu-id="b848e-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="b848e-272">In caso contrario, la condizione è false.</span><span class="sxs-lookup"><span data-stu-id="b848e-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="b848e-273">Il **IsNull** e **valore** attributi non possono essere utilizzati allo stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="b848e-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="b848e-274">**Valore**</span><span class="sxs-lookup"><span data-stu-id="b848e-274">**Value**</span></span>      | <span data-ttu-id="b848e-275">No</span><span class="sxs-lookup"><span data-stu-id="b848e-275">No</span></span>          | <span data-ttu-id="b848e-276">Valore con cui viene confrontato il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="b848e-276">The value with which the column value is compared.</span></span> <span data-ttu-id="b848e-277">Se i valori sono uguali, la condizione è true.</span><span class="sxs-lookup"><span data-stu-id="b848e-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="b848e-278">In caso contrario, la condizione è false.</span><span class="sxs-lookup"><span data-stu-id="b848e-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="b848e-279">Il **IsNull** e **valore** attributi non possono essere utilizzati allo stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="b848e-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="b848e-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-280">**Name**</span></span>       | <span data-ttu-id="b848e-281">No</span><span class="sxs-lookup"><span data-stu-id="b848e-281">No</span></span>          | <span data-ttu-id="b848e-282">Nome della proprietà dell'entità del modello concettuale il cui valore viene utilizzato per valutare la condizione.</span><span class="sxs-lookup"><span data-stu-id="b848e-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="b848e-283">Questo attributo non è applicabile se il **condizione** elemento viene usato all'interno di un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="b848e-284">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-284">Example</span></span>

<span data-ttu-id="b848e-285">L'esempio seguente illustra **Condition** gli elementi come elementi figlio del **MappingFragment** elementi.</span><span class="sxs-lookup"><span data-stu-id="b848e-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="b848e-286">Quando **HireDate** non è null e **EnrollmentDate** è null, i dati vengono mappati tra il **SchoolModel.Instructor** tipo e il **PersonID**e **HireDate** colonne del **Person** tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="b848e-287">Quando **EnrollmentDate** non è null e **HireDate** è null, i dati vengono mappati tra il **SchoolModel.Student** tipo e il **PersonID** e **Enrollment** le colonne delle **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="b848e-288">Elemento DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="b848e-289">Il **DeleteFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di eliminazione di un tipo di entità o associazione nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="b848e-290">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="b848e-291">Per altre informazioni, vedere la funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-292">Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.</span><span class="sxs-lookup"><span data-stu-id="b848e-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="b848e-293">DeleteFunction applicato a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="b848e-294">Quando applicato all'elemento EntityTypeMapping, il **DeleteFunction** elemento esegue il mapping della funzione di eliminazione di un tipo di entità nel modello concettuale a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="b848e-295">Il **DeleteFunction** elemento può avere i seguenti elementi figlio quando applicato a un **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="b848e-296">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="b848e-297">Elemento ComplexProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="b848e-298">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b848e-299">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-299">Applicable Attributes</span></span>

<span data-ttu-id="b848e-300">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **DeleteFunction** elemento quando viene applicato a un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="b848e-301">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-301">Attribute Name</span></span>            | <span data-ttu-id="b848e-302">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-302">Is Required</span></span> | <span data-ttu-id="b848e-303">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-304">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-304">**FunctionName**</span></span>          | <span data-ttu-id="b848e-305">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-305">Yes</span></span>         | <span data-ttu-id="b848e-306">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata.</span><span class="sxs-lookup"><span data-stu-id="b848e-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="b848e-307">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="b848e-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="b848e-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="b848e-309">No</span><span class="sxs-lookup"><span data-stu-id="b848e-309">No</span></span>          | <span data-ttu-id="b848e-310">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="b848e-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="b848e-311">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-311">Example</span></span>

<span data-ttu-id="b848e-312">Nell'esempio seguente si basa sul modello School e viene illustrato il **DeleteFunction** mapping della funzione di eliminazione dell'elemento il **persona** tipo di entità per il **DeletePerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="b848e-313">Il **DeletePerson** stored procedure viene dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="b848e-314">DeleteFunction applicato ad AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="b848e-315">Quando applicato all'elemento AssociationSetMapping, il **DeleteFunction** elemento esegue il mapping della funzione di eliminazione di un'associazione nel modello concettuale a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="b848e-316">Il **DeleteFunction** elemento può avere i seguenti elementi figlio quando applicato al **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="b848e-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b848e-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-318">Applicable Attributes</span></span>

<span data-ttu-id="b848e-319">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **DeleteFunction** elemento quando viene applicato per il **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="b848e-320">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-320">Attribute Name</span></span>            | <span data-ttu-id="b848e-321">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-321">Is Required</span></span> | <span data-ttu-id="b848e-322">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-323">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-323">**FunctionName**</span></span>          | <span data-ttu-id="b848e-324">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-324">Yes</span></span>         | <span data-ttu-id="b848e-325">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata.</span><span class="sxs-lookup"><span data-stu-id="b848e-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="b848e-326">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="b848e-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="b848e-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="b848e-328">No</span><span class="sxs-lookup"><span data-stu-id="b848e-328">No</span></span>          | <span data-ttu-id="b848e-329">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="b848e-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="b848e-330">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-330">Example</span></span>

<span data-ttu-id="b848e-331">Nell'esempio seguente si basa sul modello School e Mostra le **DeleteFunction** elemento usato per eseguire il mapping di funzione di eliminazione del **CourseInstructor** associazione per il  **DeleteCourseInstructor** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="b848e-332">Il **DeleteCourseInstructor** stored procedure viene dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="b848e-333">Elemento EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="b848e-334">Il **EndProperty** elemento specifica MSL (mapping language) definisce il mapping tra un'entità finale o una funzione di modifica di un'associazione del modello concettuale e il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="b848e-335">Il mapping di colonne delle proprietà viene specificato in un elemento ScalarProperty figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="b848e-336">Quando un **EndProperty** elemento viene usato per definire il mapping per l'entità finale di un'associazione del modello concettuale, è un figlio di un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="b848e-337">Quando la **EndProperty** elemento viene usato per definire il mapping per una funzione di modifica di un'associazione del modello concettuale, è un figlio di un elemento InsertFunction o elemento DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="b848e-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="b848e-338">Il **EndProperty** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-339">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-340">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-340">Applicable Attributes</span></span>

<span data-ttu-id="b848e-341">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **EndProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="b848e-342">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-342">Attribute Name</span></span> | <span data-ttu-id="b848e-343">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-343">Is Required</span></span> | <span data-ttu-id="b848e-344">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="b848e-345">nome</span><span class="sxs-lookup"><span data-stu-id="b848e-345">Name</span></span>           | <span data-ttu-id="b848e-346">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-346">Yes</span></span>         | <span data-ttu-id="b848e-347">Nome dell'entità finale dell'associazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-348">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-348">Example</span></span>

<span data-ttu-id="b848e-349">L'esempio seguente mostra un' **AssociationSetMapping** elemento in cui il **FK\_corso\_reparto** associazione nel modello concettuale è mappato al **Course** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="b848e-350">I mapping tra le colonne della tabella e proprietà del tipo di associazione vengono specificati in figlio **EndProperty** elementi.</span><span class="sxs-lookup"><span data-stu-id="b848e-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-351">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-351">Example</span></span>

<span data-ttu-id="b848e-352">L'esempio seguente mostra le **EndProperty** mapping di funzioni insert e delete di un'associazione tramite l'elemento (**CourseInstructor**) alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="b848e-353">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="b848e-354">Elemento EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-355">Il **EntityContainerMapping** elemento in specifica linguaggio MSL (mapping) viene mappato il contenitore di entità nel modello concettuale al contenitore di entità nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="b848e-356">Il **EntityContainerMapping** elemento è figlio dell'elemento di Mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="b848e-357">Il **EntityContainerMapping** elemento può presentare gli elementi figlio seguenti (nell'ordine riportato):</span><span class="sxs-lookup"><span data-stu-id="b848e-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b848e-358">EntitySetMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="b848e-359">AssociationSetMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="b848e-360">FunctionImportMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-361">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-361">Applicable Attributes</span></span>

<span data-ttu-id="b848e-362">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityContainerMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="b848e-363">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-363">Attribute Name</span></span>            | <span data-ttu-id="b848e-364">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-364">Is Required</span></span> | <span data-ttu-id="b848e-365">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="b848e-366">**StorageModelContainer**</span></span> | <span data-ttu-id="b848e-367">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-367">Yes</span></span>         | <span data-ttu-id="b848e-368">Nome del contenitore di entità del modello di archiviazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="b848e-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="b848e-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="b848e-370">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-370">Yes</span></span>         | <span data-ttu-id="b848e-371">Nome del contenitore di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="b848e-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="b848e-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="b848e-373">No</span><span class="sxs-lookup"><span data-stu-id="b848e-373">No</span></span>          | <span data-ttu-id="b848e-374">**True** oppure **False**.</span><span class="sxs-lookup"><span data-stu-id="b848e-374">**True** or **False**.</span></span> <span data-ttu-id="b848e-375">Se **False**, non vengono generati alcuna visualizzazione di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="b848e-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="b848e-376">Questo attributo deve essere impostato su **False** quando si dispone di un mapping di sola lettura che sarebbe non valido perché i dati potrebbero non eseguire il round trip correttamente.</span><span class="sxs-lookup"><span data-stu-id="b848e-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="b848e-377">Il valore predefinito è **True**.</span><span class="sxs-lookup"><span data-stu-id="b848e-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-378">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-378">Example</span></span>

<span data-ttu-id="b848e-379">L'esempio seguente mostra un **EntityContainerMapping** elemento che viene eseguito il mapping di **SchoolModelEntities** contenitore (il contenitore di entità del modello concettuale) per il  **SchoolModelStoreContainer** contenitore (il contenitore di entità del modello di archiviazione):</span><span class="sxs-lookup"><span data-stu-id="b848e-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="b848e-380">Elemento EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-381">Il **EntitySetMapping** elemento tutti i tipi in un'entità del modello concettuale è impostati su entità di eseguire il mapping specification language (MSL) imposta il modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="b848e-382">Una set di entità nel modello concettuale è un contenitore logico per le istanze delle entità dello stesso tipo (e i tipi derivati).</span><span class="sxs-lookup"><span data-stu-id="b848e-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="b848e-383">Una set di entità nel modello di archiviazione rappresenta una tabella o vista nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="b848e-384">Il set di entità del modello concettuale è specificato dal valore della **Name** attributo del **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="b848e-385">Il mapping alla tabella o vista specificato dal **StoreEntitySet** attributo in ogni elemento MappingFragment figlio o nel **EntitySetMapping** elemento stesso.</span><span class="sxs-lookup"><span data-stu-id="b848e-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="b848e-386">Il **EntitySetMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-387">EntityTypeMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="b848e-388">Elemento QueryView (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="b848e-389">MappingFragment (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-390">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-390">Applicable Attributes</span></span>

<span data-ttu-id="b848e-391">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="b848e-392">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-392">Attribute Name</span></span>           | <span data-ttu-id="b848e-393">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-393">Is Required</span></span> | <span data-ttu-id="b848e-394">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-395">**Name**</span></span>                 | <span data-ttu-id="b848e-396">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-396">Yes</span></span>         | <span data-ttu-id="b848e-397">Nome del set di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="b848e-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="b848e-398">**TypeName** **1**</span></span>       | <span data-ttu-id="b848e-399">No</span><span class="sxs-lookup"><span data-stu-id="b848e-399">No</span></span>          | <span data-ttu-id="b848e-400">Nome del tipo di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="b848e-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="b848e-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="b848e-402">No</span><span class="sxs-lookup"><span data-stu-id="b848e-402">No</span></span>          | <span data-ttu-id="b848e-403">Nome del set di entità del modello di archiviazione a cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="b848e-404">**Makecolumnsdistinct impostato**</span><span class="sxs-lookup"><span data-stu-id="b848e-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="b848e-405">No</span><span class="sxs-lookup"><span data-stu-id="b848e-405">No</span></span>          | <span data-ttu-id="b848e-406">**True** oppure **False** a seconda se vengono restituite solo righe distinte.</span><span class="sxs-lookup"><span data-stu-id="b848e-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="b848e-407">Se questo attributo è impostato su **True**, il **GenerateUpdateViews** attributo dell'elemento EntityContainerMapping deve essere impostata su **False**.</span><span class="sxs-lookup"><span data-stu-id="b848e-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="b848e-408">**1** il **TypeName** e **StoreEntitySet** attributi possono essere utilizzati al posto di elementi figlio EntityTypeMapping e MappingFragment per eseguire il mapping un singolo tipo di entità a una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="b848e-409">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-409">Example</span></span>

<span data-ttu-id="b848e-410">L'esempio seguente mostra un' **EntitySetMapping** elemento che esegue il mapping di tre tipi (un tipo di base e due tipi derivati) nel **corsi** set di entità del modello concettuale a tre tabelle diverse nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="b848e-411">Le tabelle sono specificate per il **StoreEntitySet** attributo in ogni **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="b848e-412">Elemento EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-413">Il **EntityTypeMapping** elemento specifica MSL (mapping language) definisce il mapping tra un tipo di entità nel modello concettuale e tabelle o viste nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="b848e-414">Per informazioni sui tipi di entità del modello concettuale e tabelle di database o viste sottostanti, vedere l'elemento EntityType (CSDL) e l'elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="b848e-415">Viene specificato il tipo di entità del modello concettuale che viene eseguito il mapping dal **nomeTipo** attributo delle **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="b848e-416">La tabella o vista in cui viene eseguito il mapping viene specificato per il **StoreEntitySet** attributo dell'elemento MappingFragment figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="b848e-417">Il ModificationFunctionMapping elemento figlio può essere usato per eseguire il mapping di inserimento, aggiornamento o eliminazione di funzioni di tipi di entità alle stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="b848e-418">Il **EntityTypeMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-419">MappingFragment (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="b848e-420">ModificationFunctionMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="b848e-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-421">ScalarProperty</span></span>
-   <span data-ttu-id="b848e-422">Condizione</span><span class="sxs-lookup"><span data-stu-id="b848e-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-423">**MappingFragment** e **ModificationFunctionMapping** non può contenere elementi figlio di elementi di **EntityTypeMapping** elemento allo stesso tempo.</span><span class="sxs-lookup"><span data-stu-id="b848e-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="b848e-424">Il **ScalarProperty** e **condizione** elementi possono essere solo elementi figlio del **EntityTypeMapping** elemento quando viene usata all'interno di un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-425">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-425">Applicable Attributes</span></span>

<span data-ttu-id="b848e-426">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="b848e-427">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-427">Attribute Name</span></span> | <span data-ttu-id="b848e-428">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-428">Is Required</span></span> | <span data-ttu-id="b848e-429">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="b848e-430">**TypeName**</span></span>   | <span data-ttu-id="b848e-431">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-431">Yes</span></span>         | <span data-ttu-id="b848e-432">Nome completo dello spazio dei nomi del tipo di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="b848e-433">Se il tipo è astratto o un tipo derivato, il valore deve essere `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="b848e-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-434">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-434">Example</span></span>

<span data-ttu-id="b848e-435">L'esempio seguente illustra un elemento EntitySetMapping con due figlio **EntityTypeMapping** elementi.</span><span class="sxs-lookup"><span data-stu-id="b848e-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="b848e-436">Nel primo **EntityTypeMapping** elemento, il **SchoolModel.Person** viene eseguito il mapping di tipo di entità per il **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="b848e-437">Nella seconda **EntityTypeMapping** dell'elemento, la funzionalità di aggiornamento il **SchoolModel.Person** tipo viene mappato a una stored procedure **UpdatePerson**, nel database .</span><span class="sxs-lookup"><span data-stu-id="b848e-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-438">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-438">Example</span></span>

<span data-ttu-id="b848e-439">Nell'esempio successivo viene mostrato il mapping di una gerarchia di tipo in cui il tipo radice è astratto.</span><span class="sxs-lookup"><span data-stu-id="b848e-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="b848e-440">Si noti l'uso del `IsOfType` sintassi per il **TypeName** attributi.</span><span class="sxs-lookup"><span data-stu-id="b848e-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="b848e-441">Elemento FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-442">Il **FunctionImportMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure o funzione nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="b848e-443">Le importazioni di funzioni devono essere dichiarate nel modello concettuale mentre le stored procedure nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="b848e-444">Per altre informazioni, vedere l'elemento FunctionImport (CSDL) e funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-445">Per impostazione predefinita, se un'importazione di funzioni restituisce un tipo di entità del modello concettuale o un tipo complesso, i nomi delle colonne restituiti dalla stored procedure sottostante devono corrispondere esattamente ai nomi delle proprietà nel tipo di modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="b848e-446">Se i nomi delle colonne non corrispondono esattamente ai nomi delle proprietà, il mapping deve essere definito in un elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="b848e-447">Il **FunctionImportMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-448">ResultMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-449">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-449">Applicable Attributes</span></span>

<span data-ttu-id="b848e-450">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **FunctionImportMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="b848e-451">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-451">Attribute Name</span></span>         | <span data-ttu-id="b848e-452">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-452">Is Required</span></span> | <span data-ttu-id="b848e-453">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="b848e-454">**FunctionImportName**</span></span> | <span data-ttu-id="b848e-455">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-455">Yes</span></span>         | <span data-ttu-id="b848e-456">Nome dell'importazione di funzioni nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="b848e-457">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-457">**FunctionName**</span></span>       | <span data-ttu-id="b848e-458">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-458">Yes</span></span>         | <span data-ttu-id="b848e-459">Nome completo dello spazio dei nomi della funzione nel modello di archiviazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-460">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-460">Example</span></span>

<span data-ttu-id="b848e-461">Nell'esempio seguente si basa sul modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-461">The following example is based on the School model.</span></span> <span data-ttu-id="b848e-462">Si consideri la funzione seguente nel modello di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="b848e-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="b848e-463">Si consideri anche questa importazione di funzioni nel modello concettuale:</span><span class="sxs-lookup"><span data-stu-id="b848e-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="b848e-464">L'esempio seguente viene mostrato un **FunctionImportMapping** elemento usato per eseguire il mapping di funzione e importazione sopra tra loro di funzioni:</span><span class="sxs-lookup"><span data-stu-id="b848e-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="b848e-465">Elemento InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="b848e-466">Il **InsertFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di inserimento di un tipo di entità o associazione nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="b848e-467">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="b848e-468">Per altre informazioni, vedere la funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-469">Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.</span><span class="sxs-lookup"><span data-stu-id="b848e-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="b848e-470">Il **InsertFunction** elemento può essere un figlio dell'elemento ModificationFunctionMapping e applicato a EntityTypeMapping (elemento) o l'elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="b848e-471">InsertFunction applicato a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="b848e-472">Quando applicato all'elemento EntityTypeMapping, il **InsertFunction** elemento esegue il mapping della funzione di inserimento di un tipo di entità nel modello concettuale a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="b848e-473">Il **InsertFunction** elemento può avere i seguenti elementi figlio quando applicato a un **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="b848e-474">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="b848e-475">Elemento ComplexProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="b848e-476">ResultBinding (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="b848e-477">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b848e-478">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-478">Applicable Attributes</span></span>

<span data-ttu-id="b848e-479">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **InsertFunction** elemento quando viene applicato a un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="b848e-480">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-480">Attribute Name</span></span>            | <span data-ttu-id="b848e-481">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-481">Is Required</span></span> | <span data-ttu-id="b848e-482">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-483">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-483">**FunctionName**</span></span>          | <span data-ttu-id="b848e-484">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-484">Yes</span></span>         | <span data-ttu-id="b848e-485">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="b848e-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="b848e-486">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="b848e-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="b848e-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="b848e-488">No</span><span class="sxs-lookup"><span data-stu-id="b848e-488">No</span></span>          | <span data-ttu-id="b848e-489">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="b848e-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="b848e-490">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-490">Example</span></span>

<span data-ttu-id="b848e-491">Nell'esempio seguente si basa sul modello School e viene illustrato il **InsertFunction** elemento usato per eseguire il mapping di funzione di inserimento del tipo di entità Person al **InsertPerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="b848e-492">Il **InsertPerson** stored procedure viene dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="b848e-493">InsertFunction applicato ad AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="b848e-494">Quando applicato all'elemento AssociationSetMapping, il **InsertFunction** elemento esegue il mapping della funzione di inserimento di un'associazione nel modello concettuale a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="b848e-495">Il **InsertFunction** elemento può avere i seguenti elementi figlio quando applicato al **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="b848e-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="b848e-497">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-497">Applicable Attributes</span></span>

<span data-ttu-id="b848e-498">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **InsertFunction** elemento quando viene applicato per il **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="b848e-499">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-499">Attribute Name</span></span>            | <span data-ttu-id="b848e-500">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-500">Is Required</span></span> | <span data-ttu-id="b848e-501">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-502">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-502">**FunctionName**</span></span>          | <span data-ttu-id="b848e-503">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-503">Yes</span></span>         | <span data-ttu-id="b848e-504">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="b848e-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="b848e-505">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="b848e-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="b848e-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="b848e-507">No</span><span class="sxs-lookup"><span data-stu-id="b848e-507">No</span></span>          | <span data-ttu-id="b848e-508">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="b848e-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="b848e-509">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-509">Example</span></span>

<span data-ttu-id="b848e-510">Nell'esempio seguente si basa sul modello School e viene illustrato il **InsertFunction** elemento usato per eseguire il mapping di funzione di inserimento del **CourseInstructor** associazione per il  **InsertCourseInstructor** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="b848e-511">Il **InsertCourseInstructor** stored procedure viene dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="b848e-512">Elemento Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="b848e-513">Il **Mapping** elemento in specifica linguaggio MSL (mapping) contiene informazioni per il mapping tra gli oggetti definiti in un modello concettuale a un database (come descritto in un modello di archiviazione).</span><span class="sxs-lookup"><span data-stu-id="b848e-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="b848e-514">Per altre informazioni, vedere Specifica CSDL e SSDL Specification.</span><span class="sxs-lookup"><span data-stu-id="b848e-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="b848e-515">Il **Mapping** è l'elemento radice per una specifica di mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="b848e-516">Lo spazio dei nomi XML per il mapping di specifiche http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="b848e-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="b848e-517">L'elemento Mapping può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="b848e-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="b848e-518">Alias (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="b848e-519">EntityContainerMapping (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="b848e-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="b848e-520">I nomi di tipi di modelli concettuale e di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b848e-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="b848e-521">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere l'elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="b848e-522">Per informazioni sul nome dello spazio dei nomi di modello archiviazione, vedere l'elemento Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="b848e-523">Gli alias degli spazi dei nomi utilizzati in MSL possono essere definiti con l'elemento Alias.</span><span class="sxs-lookup"><span data-stu-id="b848e-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-524">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-524">Applicable Attributes</span></span>

<span data-ttu-id="b848e-525">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **Mapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="b848e-526">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-526">Attribute Name</span></span> | <span data-ttu-id="b848e-527">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-527">Is Required</span></span> | <span data-ttu-id="b848e-528">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="b848e-529">**Barra spaziatrice**</span><span class="sxs-lookup"><span data-stu-id="b848e-529">**Space**</span></span>      | <span data-ttu-id="b848e-530">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-530">Yes</span></span>         | <span data-ttu-id="b848e-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="b848e-531">**C-S**.</span></span> <span data-ttu-id="b848e-532">Si tratta di un valore fisso e non può essere modificato.</span><span class="sxs-lookup"><span data-stu-id="b848e-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-533">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-533">Example</span></span>

<span data-ttu-id="b848e-534">L'esempio seguente mostra una **Mapping** elemento basato su parte del modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="b848e-535">Per altre informazioni sul modello School, vedere Guida introduttiva (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="b848e-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="b848e-536">Elemento MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="b848e-537">Il **MappingFragment** elemento specifica MSL (mapping language) definisce il mapping tra le proprietà di un tipo di entità del modello concettuale e una tabella o vista nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="b848e-538">Per informazioni sui tipi di entità del modello concettuale e tabelle di database o viste sottostanti, vedere l'elemento EntityType (CSDL) e l'elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="b848e-539">Il **MappingFragment** può essere un elemento figlio dell'elemento EntityTypeMapping o l'elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="b848e-540">Il **MappingFragment** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-541">ComplexType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="b848e-542">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="b848e-543">Condizione (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-544">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-544">Applicable Attributes</span></span>

<span data-ttu-id="b848e-545">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="b848e-546">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-546">Attribute Name</span></span>          | <span data-ttu-id="b848e-547">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-547">Is Required</span></span> | <span data-ttu-id="b848e-548">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="b848e-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="b848e-550">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-550">Yes</span></span>         | <span data-ttu-id="b848e-551">Nome della tabella o visualizzazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="b848e-552">**Makecolumnsdistinct impostato**</span><span class="sxs-lookup"><span data-stu-id="b848e-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="b848e-553">No</span><span class="sxs-lookup"><span data-stu-id="b848e-553">No</span></span>          | <span data-ttu-id="b848e-554">**True** oppure **False** a seconda se vengono restituite solo righe distinte.</span><span class="sxs-lookup"><span data-stu-id="b848e-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="b848e-555">Se questo attributo è impostato su **True**, il **GenerateUpdateViews** attributo dell'elemento EntityContainerMapping deve essere impostata su **False**.</span><span class="sxs-lookup"><span data-stu-id="b848e-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-556">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-556">Example</span></span>

<span data-ttu-id="b848e-557">L'esempio seguente mostra una **MappingFragment** elemento come figlio di un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="b848e-558">In questo esempio, le proprietà del **Course** tipo nel modello concettuale vengono mappate alle colonne delle **corso** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-559">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-559">Example</span></span>

<span data-ttu-id="b848e-560">L'esempio seguente mostra una **MappingFragment** elemento come figlio di un **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="b848e-561">Come illustrato nell'esempio precedente, le proprietà del **Course** tipo nel modello concettuale vengono mappate alle colonne delle **corso** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="b848e-562">Elemento ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-563">Il **ModificationFunctionMapping** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping di inserimento, aggiornamento ed eliminazione di funzioni di un tipo di entità del modello concettuale alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="b848e-564">Il **ModificationFunctionMapping** elemento può anche eseguire il mapping di inserimento ed eliminare le funzioni per le associazioni molti-a-molti nel modello concettuale alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="b848e-565">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="b848e-566">Per altre informazioni, vedere la funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-567">Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.</span><span class="sxs-lookup"><span data-stu-id="b848e-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="b848e-568">Se esiste un mapping tra le funzioni di modifica per un'entità in una gerarchia di ereditarietà e stored procedure, è necessario eseguire il mapping delle funzioni di modifica per tutti i tipi nella gerarchia a stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="b848e-569">Il **ModificationFunctionMapping** elemento può essere un figlio dell'elemento EntityTypeMapping o AssociationSetMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="b848e-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="b848e-570">Il **ModificationFunctionMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-571">DeleteFunction (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="b848e-572">InsertFunction (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="b848e-573">UpdateFunction (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="b848e-574">Attributi non sono applicabili per il **ModificationFunctionMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="b848e-575">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-575">Example</span></span>

<span data-ttu-id="b848e-576">Nell'esempio seguente viene illustrato il mapping per il **persone** set di entità nel modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="b848e-577">Oltre al mapping di colonna per il **Person** tipo di entità, il mapping dell'inserimento, aggiornamento ed eliminazione di funzioni delle **persona** tipo vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="b848e-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="b848e-578">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-579">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-579">Example</span></span>

<span data-ttu-id="b848e-580">Nell'esempio seguente viene illustrato il mapping per il **CourseInstructor** set di associazioni nel modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="b848e-581">Oltre al mapping di colonna per il **CourseInstructor** associazione, il mapping delle funzioni insert e delete delle **CourseInstructor** associazione vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="b848e-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="b848e-582">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="b848e-583">Elemento QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="b848e-584">Il **QueryView** elemento specifica MSL (mapping language) definisce un mapping di sola lettura tra un tipo di entità o associazione nel modello concettuale e una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="b848e-585">Il mapping viene definito con una query Entity SQL che viene valutata in base al modello di archiviazione e si esprime il risultato impostato in termini di entità o associazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="b848e-586">Poiché le visualizzazioni di query sono di sola lettura, non è possibile utilizzare comandi di aggiornamento standard per aggiornare i tipi definiti da tali visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="b848e-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="b848e-587">A tale scopo è possibile utilizzare le funzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="b848e-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="b848e-588">Per altre informazioni, vedere Procedura: mapping delle funzioni di modifica alle Stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-589">Nel **QueryView** dell'elemento, le espressioni Entity SQL che contengono **GroupBy**, aggregazioni di gruppo o le proprietà di navigazione non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="b848e-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="b848e-590">Il **QueryView** elemento può essere un figlio dell'elemento EntitySetMapping o AssociationSetMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="b848e-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="b848e-591">Nel primo caso, la visualizzazione di query definisce un mapping di sola lettura per un'entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="b848e-592">Nel secondo caso, la visualizzazione di query definisce un mapping di sola lettura per un'associazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-593">Se il **AssociationSetMapping** elemento è un'associazione con un vincolo referenziale, il **AssociationSetMapping** elemento viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="b848e-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="b848e-594">Per altre informazioni, vedere l'elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="b848e-595">Il **QueryView** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-596">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-596">Applicable Attributes</span></span>

<span data-ttu-id="b848e-597">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **QueryView** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="b848e-598">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-598">Attribute Name</span></span> | <span data-ttu-id="b848e-599">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-599">Is Required</span></span> | <span data-ttu-id="b848e-600">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="b848e-601">**TypeName**</span></span>   | <span data-ttu-id="b848e-602">No</span><span class="sxs-lookup"><span data-stu-id="b848e-602">No</span></span>          | <span data-ttu-id="b848e-603">Nome del tipo di modello concettuale mappato dalla visualizzazione di query.</span><span class="sxs-lookup"><span data-stu-id="b848e-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-604">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-604">Example</span></span>

<span data-ttu-id="b848e-605">Nell'esempio seguente il **QueryView** come figlio dell'elemento il **EntitySetMapping** elemento e viene definito un mapping di visualizzazione di query per il **reparto** tipo di entità nel Modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="b848e-606">Poiché la query restituisce solo un sottoinsieme dei membri del **reparto** tipo nel modello di archiviazione, il **reparto** tipo nel modello School è stato modificato base al mapping come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b848e-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-607">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-607">Example</span></span>

<span data-ttu-id="b848e-608">L'esempio seguente mostra la **QueryView** elemento come figlio di un **AssociationSetMapping** elemento e viene definito un mapping di sola lettura per il `FK_Course_Department` associazione nel modello School.</span><span class="sxs-lookup"><span data-stu-id="b848e-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="b848e-609">Commenti</span><span class="sxs-lookup"><span data-stu-id="b848e-609">Comments</span></span>

<span data-ttu-id="b848e-610">È possibile definire le visualizzazioni di query per consentire gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="b848e-611">Definire un'entità nel modello concettuale che non includa tutte le proprietà dell'entità nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="b848e-612">Sono incluse proprietà che non dispongono di valori predefiniti e non supportano **null** valori.</span><span class="sxs-lookup"><span data-stu-id="b848e-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="b848e-613">Eseguire il mapping delle colonne calcolate nel modello di archiviazione alle proprietà dei tipi di entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="b848e-614">Definire un mapping in cui le condizioni utilizzate per partizionare le entità nel modello concettuale non siano basate sull'uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="b848e-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="b848e-615">Quando si specifica un mapping condizionale utilizzando il **condizione** elemento, la condizione fornita deve essere uguale al valore specificato.</span><span class="sxs-lookup"><span data-stu-id="b848e-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="b848e-616">Per altre informazioni, vedere l'elemento Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="b848e-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="b848e-617">Eseguire il mapping della stessa colonna nel modello di archiviazione a più tipi nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="b848e-618">Eseguire il mapping di più tipi alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="b848e-619">Definire associazioni nel modello concettuale che non siano basate sulle chiavi esterne dello schema relazionale.</span><span class="sxs-lookup"><span data-stu-id="b848e-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="b848e-620">Utilizzare la logica di business personalizzata per impostare il valore delle proprietà nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="b848e-621">Ad esempio, è possibile mappare il valore di stringa "T" nell'origine dati su un valore di **true**, un valore booleano, nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="b848e-622">Definire filtri condizionali per i risultati della query.</span><span class="sxs-lookup"><span data-stu-id="b848e-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="b848e-623">Applicare ai dati del modello concettuale un numero di restrizioni minore di quelle applicate al modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="b848e-624">Ad esempio, è possibile impostare una proprietà nel modello concettuale che ammette valori null anche se la colonna a cui viene mappata non supporta **null**valori.</span><span class="sxs-lookup"><span data-stu-id="b848e-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="b848e-625">Le considerazioni seguenti riguardano la definizione delle visualizzazioni di query per le entità:</span><span class="sxs-lookup"><span data-stu-id="b848e-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="b848e-626">Le visualizzazioni di query sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="b848e-626">Query views are read-only.</span></span> <span data-ttu-id="b848e-627">È possibile applicare aggiornamenti alle entità solo utilizzando le funzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="b848e-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="b848e-628">Quando viene definito un tipo di entità da una visualizzazione di query, è necessario definire anche tutte le entità correlate dalle visualizzazioni di query.</span><span class="sxs-lookup"><span data-stu-id="b848e-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="b848e-629">Quando si esegue il mapping di un'associazione molti-a-molti a un'entità nel modello di archiviazione che rappresenta una tabella dei collegamenti nello schema relazionale, è necessario definire un **QueryView** elemento le **AssociationSetMapping** elemento per la tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="b848e-630">È necessario definire le visualizzazioni di query per tutti i tipi di una gerarchia di tipi.</span><span class="sxs-lookup"><span data-stu-id="b848e-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="b848e-631">È possibile eseguire questa operazione nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="b848e-632">Con un unico **QueryView** elemento che specifica una singola query Entity SQL che restituisce l'unione di tutti i tipi di entità nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b848e-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="b848e-633">Con un unico **QueryView** elemento che specifica una singola query Entity SQL che usa l'operatore CASE per restituire un tipo di entità specifico nella gerarchia basata su una condizione specifica.</span><span class="sxs-lookup"><span data-stu-id="b848e-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="b848e-634">Con un'ulteriore **QueryView** (elemento) per un tipo specifico nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b848e-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="b848e-635">In questo caso, usare il **nomeTipo** attributo delle **QueryView** elemento per specificare il tipo di entità per ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="b848e-636">Quando si definisce una visualizzazione di query, è possibile specificare il **StorageSetName** attributo il **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="b848e-637">Quando si definisce una visualizzazione di query, il **EntitySetMapping**elemento non può contenere anche **proprietà** mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="b848e-638">Elemento ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="b848e-639">Il **ResultBinding** elemento in specifica linguaggio MSL (mapping) esegue il mapping di valori di colonna restituiti dalle stored procedure alle proprietà dell'entità nel modello concettuale quando le funzioni di modifica tipo di entità vengono eseguito il mapping alla stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="b848e-640">Ad esempio, quando il valore di una colonna identity viene restituito da un'istruzione insert stored procedure, il **ResultBinding** elemento esegue il mapping del valore restituito a una proprietà del tipo di entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="b848e-641">Il **ResultBinding** elemento può essere figlio dell'elemento InsertFunction o l'elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="b848e-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="b848e-642">Il **ResultBinding** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-643">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-643">Applicable Attributes</span></span>

<span data-ttu-id="b848e-644">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ResultBinding** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="b848e-645">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-645">Attribute Name</span></span> | <span data-ttu-id="b848e-646">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-646">Is Required</span></span> | <span data-ttu-id="b848e-647">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-648">**Name**</span></span>       | <span data-ttu-id="b848e-649">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-649">Yes</span></span>         | <span data-ttu-id="b848e-650">Nome della proprietà di entità nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="b848e-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="b848e-651">**ColumnName**</span></span> | <span data-ttu-id="b848e-652">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-652">Yes</span></span>         | <span data-ttu-id="b848e-653">Nome della colonna di cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="b848e-654">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-654">Example</span></span>

<span data-ttu-id="b848e-655">Nell'esempio seguente si basa sul modello School e Mostra un **InsertFunction** elemento usato per eseguire il mapping della funzione di inserimento delle **persona** tipo di entità per il **InsertPerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="b848e-656">(Il **InsertPerson** stored procedure viene illustrata di seguito e viene dichiarata nel modello di archiviazione.) Oggetto **ResultBinding** elemento viene usato per eseguire il mapping di un valore di colonna che viene restituito dalla stored procedure (**NewPersonID**) a una proprietà di tipo di entità (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="b848e-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="b848e-657">L'istruzione Transact-SQL seguente viene descritto il **InsertPerson** stored procedure:</span><span class="sxs-lookup"><span data-stu-id="b848e-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="b848e-658">Elemento ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="b848e-659">Il **ResultMapping** elemento specifica MSL (mapping language) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nel database sottostante quando vengono soddisfatte le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="b848e-660">L'operazione di importazione di funzioni restituisce un tipo di entità del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="b848e-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="b848e-661">I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo di entità o del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="b848e-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="b848e-662">Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo di entità o un tipo complesso è basato sui nomi delle colonne e delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="b848e-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="b848e-663">Se i nomi delle colonne non corrispondono esattamente i nomi delle proprietà, è necessario usare il **ResultMapping** elemento per definire il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="b848e-664">Per un esempio del mapping predefinito, vedere l'elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="b848e-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="b848e-665">Il **ResultMapping** è un elemento figlio dell'elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="b848e-666">Il **ResultMapping** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-667">EntityTypeMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="b848e-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-668">ComplexTypeMapping</span></span>

<span data-ttu-id="b848e-669">Attributi non sono applicabili per il **ResultMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="b848e-670">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-670">Example</span></span>

<span data-ttu-id="b848e-671">Si consideri la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="b848e-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="b848e-672">Si consideri inoltre il tipo di entità del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="b848e-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="b848e-673">Per creare un'importazione di funzioni che restituisce istanze del tipo di entità precedente, il mapping tra le colonne restituite dalla stored procedure e il tipo di entità deve essere definito un **ResultMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="b848e-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="b848e-674">Elemento ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="b848e-675">Il **ScalarProperty** elemento in specifica linguaggio MSL (mapping) esegue il mapping di una proprietà di un tipo di entità del modello concettuale, al tipo complesso o associazione a una colonna della tabella o un parametro della stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="b848e-676">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="b848e-677">Per altre informazioni, vedere la funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="b848e-678">Il **ScalarProperty** elemento può essere un elemento figlio degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="b848e-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="b848e-679">MappingFragment</span></span>
-   <span data-ttu-id="b848e-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="b848e-680">InsertFunction</span></span>
-   <span data-ttu-id="b848e-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="b848e-681">UpdateFunction</span></span>
-   <span data-ttu-id="b848e-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="b848e-682">DeleteFunction</span></span>
-   <span data-ttu-id="b848e-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-683">EndProperty</span></span>
-   <span data-ttu-id="b848e-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="b848e-684">ComplexProperty</span></span>
-   <span data-ttu-id="b848e-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="b848e-685">ResultMapping</span></span>

<span data-ttu-id="b848e-686">Come elemento figlio di **MappingFragment**, **ComplexProperty**, o **EndProperty** elemento, il **ScalarProperty** elemento esegue il mapping di una proprietà nel modello concettuale a una colonna nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="b848e-687">Come elemento figlio di **InsertFunction**, **UpdateFunction**, o **DeleteFunction** elemento, il **ScalarProperty** elemento esegue il mapping di una proprietà nel modello concettuale a un parametro della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="b848e-688">Il **ScalarProperty** elemento non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="b848e-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-689">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-689">Applicable Attributes</span></span>

<span data-ttu-id="b848e-690">Gli attributi che si applicano per la **ScalarProperty** elemento differiscono in base al ruolo dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="b848e-691">Nella tabella seguente vengono descritti gli attributi che sono applicabili quando la **ScalarProperty** elemento viene usato per eseguire il mapping di una proprietà del modello concettuale a una colonna nel database:</span><span class="sxs-lookup"><span data-stu-id="b848e-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="b848e-692">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-692">Attribute Name</span></span> | <span data-ttu-id="b848e-693">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-693">Is Required</span></span> | <span data-ttu-id="b848e-694">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="b848e-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-695">**Name**</span></span>       | <span data-ttu-id="b848e-696">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-696">Yes</span></span>         | <span data-ttu-id="b848e-697">Nome della proprietà del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="b848e-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="b848e-698">**ColumnName**</span></span> | <span data-ttu-id="b848e-699">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-699">Yes</span></span>         | <span data-ttu-id="b848e-700">Nome della colonna della tabella di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="b848e-701">Nella tabella seguente vengono descritti gli attributi che sono applicabili per il **ScalarProperty** elemento quando viene usato per eseguire il mapping di una proprietà del modello concettuale a un parametro della stored procedure:</span><span class="sxs-lookup"><span data-stu-id="b848e-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="b848e-702">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-702">Attribute Name</span></span>    | <span data-ttu-id="b848e-703">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-703">Is Required</span></span> | <span data-ttu-id="b848e-704">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="b848e-705">**Name**</span></span>          | <span data-ttu-id="b848e-706">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-706">Yes</span></span>         | <span data-ttu-id="b848e-707">Nome della proprietà del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="b848e-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="b848e-708">**ParameterName**</span></span> | <span data-ttu-id="b848e-709">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-709">Yes</span></span>         | <span data-ttu-id="b848e-710">Nome del parametro di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="b848e-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="b848e-711">**Version**</span></span>       | <span data-ttu-id="b848e-712">No</span><span class="sxs-lookup"><span data-stu-id="b848e-712">No</span></span>          | <span data-ttu-id="b848e-713">**Corrente** oppure **originale** a seconda che il valore corrente o il valore originale della proprietà deve essere usato per le verifiche della concorrenza.</span><span class="sxs-lookup"><span data-stu-id="b848e-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="b848e-714">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-714">Example</span></span>

<span data-ttu-id="b848e-715">L'esempio seguente mostra le **ScalarProperty** elemento usato in due modi:</span><span class="sxs-lookup"><span data-stu-id="b848e-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="b848e-716">Per mappare le proprietà del **Person** tipo di entità alle colonne della **persona**tabella.</span><span class="sxs-lookup"><span data-stu-id="b848e-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="b848e-717">Per mappare le proprietà del **persona** ai parametri del tipo di entità la **UpdatePerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="b848e-718">Le stored procedure vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="b848e-719">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-719">Example</span></span>

<span data-ttu-id="b848e-720">L'esempio seguente mostra le **ScalarProperty** elemento usato per eseguire il mapping di inserimento ed eliminazione di funzioni di un'associazione del modello concettuale alle stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="b848e-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="b848e-721">Le stored procedure vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="b848e-722">Elemento UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="b848e-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="b848e-723">Il **UpdateFunction** elemento in specifica linguaggio MSL (mapping) viene eseguito il mapping della funzione di aggiornamento di un tipo di entità nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="b848e-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="b848e-724">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="b848e-725">Per altre informazioni, vedere la funzione elemento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="b848e-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="b848e-726">Se non si esegue il mapping tutte e tre di inserimento, aggiornamento o eliminazione di operazioni di un tipo di entità alle stored procedure, le operazioni non mappate avrà esito negativo se eseguita in fase di esecuzione e viene generata un'UpdateException.</span><span class="sxs-lookup"><span data-stu-id="b848e-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="b848e-727">Il **UpdateFunction** elemento può essere un figlio dell'elemento ModificationFunctionMapping e applicata all'elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="b848e-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="b848e-728">Il **UpdateFunction** elemento può avere elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="b848e-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="b848e-729">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="b848e-730">Elemento ComplexProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="b848e-731">ResultBinding (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="b848e-732">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="b848e-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="b848e-733">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="b848e-733">Applicable Attributes</span></span>

<span data-ttu-id="b848e-734">Nella tabella seguente vengono descritti gli attributi che possono essere applicati per il **UpdateFunction** elemento.</span><span class="sxs-lookup"><span data-stu-id="b848e-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="b848e-735">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="b848e-735">Attribute Name</span></span>            | <span data-ttu-id="b848e-736">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b848e-736">Is Required</span></span> | <span data-ttu-id="b848e-737">Valore</span><span class="sxs-lookup"><span data-stu-id="b848e-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b848e-738">**Nomefunzione**</span><span class="sxs-lookup"><span data-stu-id="b848e-738">**FunctionName**</span></span>          | <span data-ttu-id="b848e-739">Yes</span><span class="sxs-lookup"><span data-stu-id="b848e-739">Yes</span></span>         | <span data-ttu-id="b848e-740">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di aggiornamento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="b848e-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="b848e-741">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="b848e-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="b848e-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="b848e-743">No</span><span class="sxs-lookup"><span data-stu-id="b848e-743">No</span></span>          | <span data-ttu-id="b848e-744">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="b848e-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="b848e-745">Esempio</span><span class="sxs-lookup"><span data-stu-id="b848e-745">Example</span></span>

<span data-ttu-id="b848e-746">Nell'esempio seguente si basa sul modello School e viene illustrato il **UpdateFunction** elemento usato per eseguire il mapping di funzione di aggiornamento del **persona** tipo di entità per il **UpdatePerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b848e-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="b848e-747">Il **UpdatePerson** stored procedure viene dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b848e-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
