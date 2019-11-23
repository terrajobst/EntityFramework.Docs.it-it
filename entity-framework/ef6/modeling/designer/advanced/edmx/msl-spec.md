---
title: Specifica MSL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182559"
---
# <a name="msl-specification"></a><span data-ttu-id="bb20b-102">Specifica MSL</span><span class="sxs-lookup"><span data-stu-id="bb20b-102">MSL Specification</span></span>
<span data-ttu-id="bb20b-103">MSL (Mapping Specification Language) è un linguaggio basato su XML che descrive il mapping tra il modello concettuale e il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bb20b-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="bb20b-104">In un'applicazione Entity Framework, i metadati di mapping vengono caricati da un file con estensione MSL (scritto in MSL) in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="bb20b-105">Entity Framework usa i metadati di mapping in fase di esecuzione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="bb20b-106">Il Entity Framework Designer (EF designer) archivia le informazioni di mapping in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="bb20b-107">In fase di compilazione, il Entity Designer utilizza le informazioni contenute in un file con estensione edmx per creare il file con estensione msl necessario per Entity Framework in fase di esecuzione</span><span class="sxs-lookup"><span data-stu-id="bb20b-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="bb20b-108">I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bb20b-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="bb20b-109">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere [specifica CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="bb20b-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="bb20b-110">Per informazioni sul nome dello spazio dei nomi del modello di archiviazione, vedere [specifica SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="bb20b-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="bb20b-111">Le versioni di MSL sono differenziate in base agli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="bb20b-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="bb20b-112">Versione MSL</span><span class="sxs-lookup"><span data-stu-id="bb20b-112">MSL Version</span></span> | <span data-ttu-id="bb20b-113">Spazio dei nomi XML</span><span class="sxs-lookup"><span data-stu-id="bb20b-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="bb20b-114">MSL V1</span><span class="sxs-lookup"><span data-stu-id="bb20b-114">MSL v1</span></span>      | <span data-ttu-id="bb20b-115">urn: schemas-microsoft-com: Windows: archiviazione: mapping: CS</span><span class="sxs-lookup"><span data-stu-id="bb20b-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="bb20b-116">MSL V2</span><span class="sxs-lookup"><span data-stu-id="bb20b-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="bb20b-117">MSL V3</span><span class="sxs-lookup"><span data-stu-id="bb20b-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="bb20b-118">Elemento Alias (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-118">Alias Element (MSL)</span></span>

<span data-ttu-id="bb20b-119">L'elemento **alias** in Mapping Specification Language (MSL) è un elemento figlio dell'elemento mapping utilizzato per definire gli alias per gli spazi dei nomi del modello concettuale e del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="bb20b-120">I nomi di tutti i tipi di modelli concettuale o di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bb20b-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="bb20b-121">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere elemento schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="bb20b-122">Per informazioni sul nome dello spazio dei nomi del modello di archiviazione, vedere elemento schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="bb20b-123">L'elemento **alias** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-124">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-125">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **alias** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="bb20b-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-126">Attribute Name</span></span> | <span data-ttu-id="bb20b-127">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-127">Is Required</span></span> | <span data-ttu-id="bb20b-128">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="bb20b-129">**Key**</span></span>        | <span data-ttu-id="bb20b-130">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-130">Yes</span></span>         | <span data-ttu-id="bb20b-131">Alias per lo spazio dei nomi specificato dall'attributo **value** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="bb20b-132">**Valore**</span><span class="sxs-lookup"><span data-stu-id="bb20b-132">**Value**</span></span>      | <span data-ttu-id="bb20b-133">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-133">Yes</span></span>         | <span data-ttu-id="bb20b-134">Spazio dei nomi per il quale il valore dell'elemento **chiave** è un alias.</span><span class="sxs-lookup"><span data-stu-id="bb20b-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="bb20b-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-135">Example</span></span>

<span data-ttu-id="bb20b-136">Nell'esempio seguente viene illustrato un elemento **alias** che definisce un alias, `c`, per i tipi definiti nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="bb20b-137">Elemento AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="bb20b-138">L'elemento **AssociationEnd** in Mapping Specification Language (MSL) viene utilizzato quando viene eseguito il mapping delle funzioni di modifica di un tipo di entità nel modello concettuale alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="bb20b-139">Se una modifica stored procedure accetta un parametro il cui valore è contenuto in una proprietà di associazione, l'elemento **AssociationEnd** esegue il mapping del valore della proprietà al parametro.</span><span class="sxs-lookup"><span data-stu-id="bb20b-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="bb20b-140">Per ulteriori informazioni, vedere l'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="bb20b-140">For more information, see the example below.</span></span>

<span data-ttu-id="bb20b-141">Per ulteriori informazioni sul mapping delle funzioni di modifica dei tipi di entità alle stored procedure, vedere elemento ModificationFunctionMapping (MSL) e procedura dettagliata: mapping di un'entità alle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="bb20b-142">L'elemento **AssociationEnd** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-144">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-144">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-145">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="bb20b-146">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-146">Attribute Name</span></span>     | <span data-ttu-id="bb20b-147">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-147">Is Required</span></span> | <span data-ttu-id="bb20b-148">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="bb20b-149">**AssociationSet**</span></span> | <span data-ttu-id="bb20b-150">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-150">Yes</span></span>         | <span data-ttu-id="bb20b-151">Nome dell'associazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="bb20b-152">**From**</span><span class="sxs-lookup"><span data-stu-id="bb20b-152">**From**</span></span>           | <span data-ttu-id="bb20b-153">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-153">Yes</span></span>         | <span data-ttu-id="bb20b-154">Valore dell'attributo **FromRole** della proprietà di navigazione che corrisponde all'associazione di cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="bb20b-155">Per ulteriori informazioni, vedere elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="bb20b-156">**Per**</span><span class="sxs-lookup"><span data-stu-id="bb20b-156">**To**</span></span>             | <span data-ttu-id="bb20b-157">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-157">Yes</span></span>         | <span data-ttu-id="bb20b-158">Valore dell'attributo **ToRole** della proprietà di navigazione che corrisponde all'associazione di cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="bb20b-159">Per ulteriori informazioni, vedere elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="bb20b-160">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-160">Example</span></span>

<span data-ttu-id="bb20b-161">Si consideri il tipo di entità del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="bb20b-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="bb20b-162">Si consideri inoltre la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="bb20b-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="bb20b-163">Per eseguire il mapping della funzione di aggiornamento dell'entità `Course` a questa stored procedure, è necessario specificare un valore per il parametro **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="bb20b-164">Il valore di `DepartmentID` non corrisponde a una proprietà sul tipo di entità. È contenuto in un'associazione indipendente il cui mapping è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bb20b-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="bb20b-165">Il codice seguente illustra l'elemento **AssociationEnd** usato per eseguire il mapping della proprietà **DepartmentID** dell'associazione del **reparto di\_FK\_Department** al stored procedure **UpdateCourse** (a cui viene mappata la funzione di aggiornamento del tipo di entità **Course** ):</span><span class="sxs-lookup"><span data-stu-id="bb20b-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="bb20b-166">Elemento AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-167">L'elemento **AssociationSetMapping** in Mapping Specification Language (MSL) definisce il mapping tra un'associazione nel modello concettuale e le colonne della tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="bb20b-168">Le associazioni del modello concettuale sono tipi le cui proprietà rappresentano colonne di chiavi primarie ed esterne nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="bb20b-169">L'elemento **AssociationSetMapping** utilizza due elementi EndProperty per definire i mapping tra le proprietà del tipo di associazione e le colonne nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="bb20b-170">È possibile definire condizioni su questi mapping con l'elemento Condition.</span><span class="sxs-lookup"><span data-stu-id="bb20b-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="bb20b-171">Per eseguire il mapping delle funzioni di inserimento, aggiornamento ed eliminazione per le associazioni alle stored procedure del database, utilizzare l'elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="bb20b-172">Definire mapping di sola lettura tra le associazioni e le colonne della tabella usando una stringa Entity SQL in un elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="bb20b-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-173">Se per un'associazione nel modello concettuale è definito un vincolo referenziale, non è necessario eseguire il mapping dell'associazione con un elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="bb20b-174">Se è presente un elemento **AssociationSetMapping** per un'associazione con un vincolo referenziale, i mapping definiti nell'elemento **AssociationSetMapping** verranno ignorati.</span><span class="sxs-lookup"><span data-stu-id="bb20b-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="bb20b-175">Per ulteriori informazioni, vedere elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="bb20b-176">L'elemento **AssociationSetMapping** può avere gli elementi figlio seguenti</span><span class="sxs-lookup"><span data-stu-id="bb20b-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="bb20b-177">QueryView (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="bb20b-178">EndProperty (zero o due elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="bb20b-179">Condition (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="bb20b-180">ModificationFunctionMapping (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-181">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-181">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-182">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="bb20b-183">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-183">Attribute Name</span></span>     | <span data-ttu-id="bb20b-184">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-184">Is Required</span></span> | <span data-ttu-id="bb20b-185">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-186">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-186">**Name**</span></span>           | <span data-ttu-id="bb20b-187">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-187">Yes</span></span>         | <span data-ttu-id="bb20b-188">Nome del set di associazioni del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="bb20b-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-189">**TypeName**</span></span>       | <span data-ttu-id="bb20b-190">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-190">No</span></span>          | <span data-ttu-id="bb20b-191">Nome completo dello spazio dei nomi del tipo di associazione del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="bb20b-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="bb20b-192">**StoreEntitySet**</span></span> | <span data-ttu-id="bb20b-193">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-193">No</span></span>          | <span data-ttu-id="bb20b-194">Nome della tabella di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="bb20b-195">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-195">Example</span></span>

<span data-ttu-id="bb20b-196">Nell'esempio seguente viene illustrato un elemento **AssociationSetMapping** in cui viene eseguito il mapping del **corso\_FK\_** set di associazioni Department nel modello concettuale alla tabella **Course** nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="bb20b-197">I mapping tra le proprietà del tipo di associazione e le colonne della tabella sono specificati negli elementi **EndProperty** figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="bb20b-198">Elemento ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="bb20b-199">Un elemento **ComplexProperty** in Mapping Specification Language (MSL) definisce il mapping tra una proprietà di tipo complesso in un tipo di entità del modello concettuale e le colonne della tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="bb20b-200">I mapping delle colonne delle proprietà sono specificati in elementi ScalarProperty figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="bb20b-201">L'elemento della proprietà **complexType** può presentare gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-202">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="bb20b-203">**ComplexProperty** (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="bb20b-204">ComplextTypeMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="bb20b-205">Condition (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-206">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-206">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-207">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="bb20b-208">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-208">Attribute Name</span></span> | <span data-ttu-id="bb20b-209">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-209">Is Required</span></span> | <span data-ttu-id="bb20b-210">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-211">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-211">**Name**</span></span>       | <span data-ttu-id="bb20b-212">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-212">Yes</span></span>         | <span data-ttu-id="bb20b-213">Nome della proprietà complessa di un tipo di entità nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="bb20b-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-214">**TypeName**</span></span>   | <span data-ttu-id="bb20b-215">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-215">No</span></span>          | <span data-ttu-id="bb20b-216">Nome qualificato di spazio dei nomi del tipo di proprietà del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="bb20b-217">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-217">Example</span></span>

<span data-ttu-id="bb20b-218">Di seguito viene riportato un esempio basato sul modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-218">The following example is based on the School model.</span></span> <span data-ttu-id="bb20b-219">Il tipo complesso seguente è stato aggiunto al modello concettuale:</span><span class="sxs-lookup"><span data-stu-id="bb20b-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="bb20b-220">Le proprietà **LastName** e **FirstName** del tipo di entità **Person** sono state sostituite con una proprietà complessa, **Name**:</span><span class="sxs-lookup"><span data-stu-id="bb20b-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="bb20b-221">Il MSL seguente mostra l'elemento **ComplexProperty** usato per eseguire il mapping della proprietà **Name** alle colonne nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="bb20b-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="bb20b-222">Elemento ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-223">L'elemento **ComplexTypeMapping** in Mapping Specification Language (MSL) è un elemento figlio dell'elemento ResultMapping e definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nel database sottostante quando sono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="bb20b-224">Un tipo complesso concettuale viene restituito dall'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="bb20b-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="bb20b-225">I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="bb20b-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="bb20b-226">Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo complesso è basato sui nomi delle colonne e delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="bb20b-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="bb20b-227">Se i nomi delle colonne non corrispondono esattamente ai nomi delle proprietà, è necessario utilizzare l'elemento **ComplexTypeMapping** per definire il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="bb20b-228">Per un esempio del mapping predefinito, vedere elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="bb20b-229">L'elemento **ComplexTypeMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-230">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-231">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-231">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-232">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="bb20b-233">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-233">Attribute Name</span></span> | <span data-ttu-id="bb20b-234">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-234">Is Required</span></span> | <span data-ttu-id="bb20b-235">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="bb20b-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-236">**TypeName**</span></span>   | <span data-ttu-id="bb20b-237">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-237">Yes</span></span>         | <span data-ttu-id="bb20b-238">Nome completo dello spazio dei nomi del tipo complesso di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-239">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-239">Example</span></span>

<span data-ttu-id="bb20b-240">Si consideri la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="bb20b-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="bb20b-241">Si consideri anche il tipo complesso del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="bb20b-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="bb20b-242">Per creare un'importazione di funzioni che restituisce istanze del tipo complesso precedente, il mapping tra le colonne restituite dal stored procedure e il tipo di entità deve essere definito in un elemento **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="bb20b-243">Elemento Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-243">Condition Element (MSL)</span></span>

<span data-ttu-id="bb20b-244">L'elemento **Condition** in Mapping Specification Language (MSL) posiziona le condizioni sui mapping tra il modello concettuale e il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="bb20b-245">Il mapping definito all'interno di un nodo XML è valido se vengono soddisfatte tutte le condizioni, come specificato negli elementi della **condizione** figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="bb20b-246">In caso contrario, il mapping non è valido.</span><span class="sxs-lookup"><span data-stu-id="bb20b-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="bb20b-247">Se, ad esempio, un elemento MappingFragment contiene uno o più elementi figlio **Condition** , il mapping definito all'interno del nodo **MappingFragment** sarà valido solo se vengono soddisfatte tutte le condizioni degli elementi della **condizione** figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="bb20b-248">Ogni condizione può essere applicata a un **nome** (il nome di una proprietà dell'entità del modello concettuale, specificata dall'attributo **Name** ) o a un **ColumnName** (il nome di una colonna nel database, specificato dall'attributo **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="bb20b-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="bb20b-249">Quando viene impostato l'attributo **Name** , la condizione viene verificata rispetto al valore della proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="bb20b-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="bb20b-250">Quando viene impostato l'attributo **ColumnName** , la condizione viene verificata rispetto a un valore di colonna.</span><span class="sxs-lookup"><span data-stu-id="bb20b-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="bb20b-251">È possibile specificare solo uno degli attributi **Name** o **ColumnName** in un elemento **Condition** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-252">Quando l'elemento **Condition** viene utilizzato all'interno di un elemento FunctionImportMapping, solo l'attributo **Name** non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="bb20b-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="bb20b-253">L'elemento **Condition** può essere figlio dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="bb20b-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="bb20b-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="bb20b-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-255">ComplexProperty</span></span>
-   <span data-ttu-id="bb20b-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-256">EntitySetMapping</span></span>
-   <span data-ttu-id="bb20b-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="bb20b-257">MappingFragment</span></span>
-   <span data-ttu-id="bb20b-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-258">EntityTypeMapping</span></span>

<span data-ttu-id="bb20b-259">L'elemento **Condition** non può avere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-260">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-260">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-261">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **Condition** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="bb20b-262">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-262">Attribute Name</span></span> | <span data-ttu-id="bb20b-263">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-263">Is Required</span></span> | <span data-ttu-id="bb20b-264">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-265">**ColumnName**</span></span> | <span data-ttu-id="bb20b-266">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-266">No</span></span>          | <span data-ttu-id="bb20b-267">Nome della colonna della tabella il cui valore viene utilizzato per valutare la condizione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="bb20b-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="bb20b-268">**IsNull**</span></span>     | <span data-ttu-id="bb20b-269">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-269">No</span></span>          | <span data-ttu-id="bb20b-270">**True** o **false**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-270">**True** or **False**.</span></span> <span data-ttu-id="bb20b-271">Se il valore è **true** e il valore della colonna è **null**o se il valore è **false** e il valore della colonna non è **null**, la condizione è true.</span><span class="sxs-lookup"><span data-stu-id="bb20b-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="bb20b-272">In caso contrario, la condizione è false.</span><span class="sxs-lookup"><span data-stu-id="bb20b-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="bb20b-273">Impossibile utilizzare contemporaneamente gli attributi **IsNull** e **value** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="bb20b-274">**Valore**</span><span class="sxs-lookup"><span data-stu-id="bb20b-274">**Value**</span></span>      | <span data-ttu-id="bb20b-275">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-275">No</span></span>          | <span data-ttu-id="bb20b-276">Valore con cui viene confrontato il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="bb20b-276">The value with which the column value is compared.</span></span> <span data-ttu-id="bb20b-277">Se i valori sono uguali, la condizione è true.</span><span class="sxs-lookup"><span data-stu-id="bb20b-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="bb20b-278">In caso contrario, la condizione è false.</span><span class="sxs-lookup"><span data-stu-id="bb20b-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="bb20b-279">Impossibile utilizzare contemporaneamente gli attributi **IsNull** e **value** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="bb20b-280">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-280">**Name**</span></span>       | <span data-ttu-id="bb20b-281">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-281">No</span></span>          | <span data-ttu-id="bb20b-282">Nome della proprietà dell'entità del modello concettuale il cui valore viene utilizzato per valutare la condizione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="bb20b-283">Questo attributo non è applicabile se l'elemento **Condition** viene utilizzato all'interno di un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="bb20b-284">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-284">Example</span></span>

<span data-ttu-id="bb20b-285">Nell'esempio seguente vengono mostrati gli elementi **Condition** come elementi figlio di elementi **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="bb20b-286">Quando **Hiret** non è null e **EnrollmentDate** è null, viene eseguito il mapping dei dati tra il tipo **SchoolModel. Instructor** e le colonne **PersonID** e **Hired** della tabella **Person** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="bb20b-287">Quando **EnrollmentDate** non è null e il valore di **Hiret** è null, viene eseguito il mapping dei dati tra il tipo **SchoolModel. Student** e le colonne **PersonID** e di **registrazione** della tabella **Person** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="bb20b-288">Elemento DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="bb20b-289">L'elemento **DeleteFunction** in Mapping Specification Language (MSL) esegue il mapping della funzione Delete di un tipo di entità o di un'associazione nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="bb20b-290">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-291">Per ulteriori informazioni, vedere elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-292">Se non si esegue il mapping di tutte e tre le operazioni di inserimento, aggiornamento o eliminazione di un tipo di entità alle stored procedure, le operazioni non mappate avranno esito negativo se eseguite in fase di esecuzione e viene generata un'eccezione UpdateException.</span><span class="sxs-lookup"><span data-stu-id="bb20b-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="bb20b-293">DeleteFunction applicato a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="bb20b-294">Quando applicato all'elemento EntityTypeMapping, l'elemento **DeleteFunction** esegue il mapping della funzione di eliminazione di un tipo di entità nel modello concettuale a un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="bb20b-295">Quando viene applicato a un elemento **EntityTypeMapping** , l'elemento **DeleteFunction** può includere i seguenti elementi figlio:</span><span class="sxs-lookup"><span data-stu-id="bb20b-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="bb20b-296">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="bb20b-297">ComplexProperty (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="bb20b-298">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-299">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-299">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-300">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **DeleteFunction** quando viene applicato a un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="bb20b-301">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-301">Attribute Name</span></span>            | <span data-ttu-id="bb20b-302">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-302">Is Required</span></span> | <span data-ttu-id="bb20b-303">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-304">**FunctionName**</span></span>          | <span data-ttu-id="bb20b-305">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-305">Yes</span></span>         | <span data-ttu-id="bb20b-306">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata.</span><span class="sxs-lookup"><span data-stu-id="bb20b-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="bb20b-307">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="bb20b-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="bb20b-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="bb20b-309">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-309">No</span></span>          | <span data-ttu-id="bb20b-310">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="bb20b-311">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-311">Example</span></span>

<span data-ttu-id="bb20b-312">L'esempio seguente è basato sul modello School e Mostra l'elemento **DeleteFunction** che mappa la funzione Delete del tipo di entità **Person** alla stored procedure **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="bb20b-313">Il stored procedure **DeletePerson** viene dichiarato nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="bb20b-314">DeleteFunction applicato ad AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="bb20b-315">Quando applicato all'elemento AssociationSetMapping, l'elemento **DeleteFunction** esegue il mapping della funzione Delete di un'associazione nel modello concettuale a un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="bb20b-316">L'elemento **DeleteFunction** può avere gli elementi figlio seguenti quando viene applicato all'elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="bb20b-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-318">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-319">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **DeleteFunction** quando viene applicato all'elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="bb20b-320">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-320">Attribute Name</span></span>            | <span data-ttu-id="bb20b-321">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-321">Is Required</span></span> | <span data-ttu-id="bb20b-322">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-323">**FunctionName**</span></span>          | <span data-ttu-id="bb20b-324">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-324">Yes</span></span>         | <span data-ttu-id="bb20b-325">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di eliminazione viene mappata.</span><span class="sxs-lookup"><span data-stu-id="bb20b-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="bb20b-326">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="bb20b-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="bb20b-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="bb20b-328">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-328">No</span></span>          | <span data-ttu-id="bb20b-329">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="bb20b-330">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-330">Example</span></span>

<span data-ttu-id="bb20b-331">L'esempio seguente è basato sul modello School e Mostra l'elemento **DeleteFunction** usato per eseguire il mapping della funzione Delete dell'associazione **CourseInstructor** a **DeleteCourseInstructor** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="bb20b-332">Il stored procedure **DeleteCourseInstructor** viene dichiarato nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="bb20b-333">Elemento EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="bb20b-334">L'elemento **EndProperty** in Mapping Specification Language (MSL) definisce il mapping tra un'entità finale o una funzione di modifica di un'associazione del modello concettuale e il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="bb20b-335">Il mapping delle colonne delle proprietà viene specificato in un elemento ScalarProperty figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="bb20b-336">Quando si usa un elemento **EndProperty** per definire il mapping per la fine di un'associazione del modello concettuale, è un elemento figlio di un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="bb20b-337">Quando l'elemento **EndProperty** viene usato per definire il mapping per una funzione di modifica di un'associazione del modello concettuale, è un elemento figlio di un elemento InsertFunction o DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="bb20b-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="bb20b-338">L'elemento **EndProperty** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-339">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-340">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-340">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-341">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="bb20b-342">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-342">Attribute Name</span></span> | <span data-ttu-id="bb20b-343">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-343">Is Required</span></span> | <span data-ttu-id="bb20b-344">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="bb20b-345">Name</span><span class="sxs-lookup"><span data-stu-id="bb20b-345">Name</span></span>           | <span data-ttu-id="bb20b-346">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-346">Yes</span></span>         | <span data-ttu-id="bb20b-347">Nome dell'entità finale dell'associazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-348">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-348">Example</span></span>

<span data-ttu-id="bb20b-349">Nell'esempio seguente viene illustrato un elemento **AssociationSetMapping** in cui viene eseguito il mapping della **\_FK\_Department** Association nel modello concettuale alla tabella **Course** nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="bb20b-350">I mapping tra le proprietà del tipo di associazione e le colonne della tabella sono specificati negli elementi **EndProperty** figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-351">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-351">Example</span></span>

<span data-ttu-id="bb20b-352">Nell'esempio seguente viene illustrato l'elemento **EndProperty** che consente di eseguire il mapping delle funzioni Insert e Delete di un'associazione (**CourseInstructor**) alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="bb20b-353">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="bb20b-354">Elemento EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-355">L'elemento **EntityContainerMapping** in Mapping Specification Language (MSL) esegue il mapping del contenitore di entità nel modello concettuale al contenitore di entità nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="bb20b-356">L'elemento **EntityContainerMapping** è un elemento figlio dell'elemento mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="bb20b-357">L'elemento **EntityContainerMapping** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="bb20b-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="bb20b-358">EntitySetMapping (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="bb20b-359">AssociationSetMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="bb20b-360">FunctionImportMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-361">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-361">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-362">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="bb20b-363">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-363">Attribute Name</span></span>            | <span data-ttu-id="bb20b-364">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-364">Is Required</span></span> | <span data-ttu-id="bb20b-365">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="bb20b-366">**StorageModelContainer**</span></span> | <span data-ttu-id="bb20b-367">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-367">Yes</span></span>         | <span data-ttu-id="bb20b-368">Nome del contenitore di entità del modello di archiviazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="bb20b-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="bb20b-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="bb20b-370">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-370">Yes</span></span>         | <span data-ttu-id="bb20b-371">Nome del contenitore di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="bb20b-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="bb20b-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="bb20b-373">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-373">No</span></span>          | <span data-ttu-id="bb20b-374">**True** o **false**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-374">**True** or **False**.</span></span> <span data-ttu-id="bb20b-375">Se **false**, non viene generata alcuna vista aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="bb20b-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="bb20b-376">Questo attributo deve essere impostato su **false** quando si dispone di un mapping di sola lettura che non sarebbe valido perché i dati non possono essere completati correttamente.</span><span class="sxs-lookup"><span data-stu-id="bb20b-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="bb20b-377">Il valore predefinito è **True**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-378">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-378">Example</span></span>

<span data-ttu-id="bb20b-379">Nell'esempio seguente viene illustrato un elemento **EntityContainerMapping** che esegue il mapping del contenitore **SchoolModelEntities** (il contenitore di entità del modello concettuale) al contenitore **SchoolModelStoreContainer** (il contenitore di entità del modello di archiviazione):</span><span class="sxs-lookup"><span data-stu-id="bb20b-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="bb20b-380">Elemento EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-381">L'elemento **EntitySetMapping** in Mapping Specification Language (MSL) esegue il mapping di tutti i tipi in un set di entità del modello concettuale ai set di entità nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="bb20b-382">Un set di entità nel modello concettuale è un contenitore logico per istanze di entità dello stesso tipo (e tipi derivati).</span><span class="sxs-lookup"><span data-stu-id="bb20b-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="bb20b-383">Un set di entità nel modello di archiviazione rappresenta una tabella o visualizzazione nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="bb20b-384">Il set di entità del modello concettuale viene specificato dal valore dell'attributo **Name** dell'elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="bb20b-385">La tabella o la vista di cui è stato eseguito il mapping viene specificata dall'attributo **StoreEntitySet** in ogni elemento MappingFragment figlio o nell'elemento **EntitySetMapping** stesso.</span><span class="sxs-lookup"><span data-stu-id="bb20b-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="bb20b-386">L'elemento **EntitySetMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-387">EntityTypeMapping (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="bb20b-388">QueryView (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="bb20b-389">MappingFragment (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-390">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-390">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-391">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="bb20b-392">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-392">Attribute Name</span></span>           | <span data-ttu-id="bb20b-393">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-393">Is Required</span></span> | <span data-ttu-id="bb20b-394">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-395">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-395">**Name**</span></span>                 | <span data-ttu-id="bb20b-396">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-396">Yes</span></span>         | <span data-ttu-id="bb20b-397">Nome del set di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="bb20b-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="bb20b-398">**TypeName** **1**</span></span>       | <span data-ttu-id="bb20b-399">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-399">No</span></span>          | <span data-ttu-id="bb20b-400">Nome del tipo di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="bb20b-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="bb20b-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="bb20b-402">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-402">No</span></span>          | <span data-ttu-id="bb20b-403">Nome del set di entità del modello di archiviazione a cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="bb20b-404">**Makecolumnsdistinct impostato**</span><span class="sxs-lookup"><span data-stu-id="bb20b-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="bb20b-405">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-405">No</span></span>          | <span data-ttu-id="bb20b-406">**True** o **false** , a seconda che vengano restituite solo righe distinte.</span><span class="sxs-lookup"><span data-stu-id="bb20b-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="bb20b-407">Se questo attributo è impostato su **true**, l'attributo **GenerateUpdateViews** dell'elemento EntityContainerMapping deve essere impostato su **false**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="bb20b-408">**1** gli attributi **typeName** e **StoreEntitySet** possono essere utilizzati al posto degli elementi figlio EntityTypeMapping e MappingFragment per eseguire il mapping di un singolo tipo di entità a una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="bb20b-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="bb20b-409">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-409">Example</span></span>

<span data-ttu-id="bb20b-410">Nell'esempio seguente viene illustrato un elemento **EntitySetMapping** che esegue il mapping di tre tipi (un tipo di base e due tipi derivati) nel set di entità **Courses** del modello concettuale a tre tabelle diverse del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="bb20b-411">Le tabelle vengono specificate dall'attributo **StoreEntitySet** in ogni elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="bb20b-412">Elemento EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-413">L'elemento **EntityTypeMapping** in Mapping Specification Language (MSL) definisce il mapping tra un tipo di entità nel modello concettuale e le tabelle o le viste nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="bb20b-414">Per informazioni sui tipi di entità del modello concettuale e sulle tabelle o visualizzazioni del database sottostante, vedere Elemento EntityType (CSDL) e Elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="bb20b-415">Il tipo di entità del modello concettuale di cui è in corso il mapping viene specificato dall'attributo **typeName** dell'elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="bb20b-416">La tabella o la vista di cui viene eseguito il mapping viene specificata dall'attributo **StoreEntitySet** dell'elemento MappingFragment figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="bb20b-417">L'elemento figlio ModificationFunctionMapping può essere utilizzato per eseguire il mapping delle funzioni di inserimento, aggiornamento o eliminazione dei tipi di entità alle stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="bb20b-418">L'elemento **EntityTypeMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-419">MappingFragment (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="bb20b-420">ModificationFunctionMapping (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="bb20b-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-421">ScalarProperty</span></span>
-   <span data-ttu-id="bb20b-422">Condizione</span><span class="sxs-lookup"><span data-stu-id="bb20b-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-423">Gli elementi **MappingFragment** e **ModificationFunctionMapping** non possono essere elementi figlio dell'elemento **EntityTypeMapping** nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="bb20b-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="bb20b-424">Gli elementi **ScalarProperty** e **Condition** possono essere solo elementi figlio dell'elemento **EntityTypeMapping** quando vengono usati all'interno di un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-425">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-425">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-426">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="bb20b-427">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-427">Attribute Name</span></span> | <span data-ttu-id="bb20b-428">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-428">Is Required</span></span> | <span data-ttu-id="bb20b-429">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-430">**TypeName**</span></span>   | <span data-ttu-id="bb20b-431">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-431">Yes</span></span>         | <span data-ttu-id="bb20b-432">Nome completo dello spazio dei nomi del tipo di entità del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="bb20b-433">Se il tipo è astratto o un tipo derivato, il valore deve essere `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="bb20b-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-434">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-434">Example</span></span>

<span data-ttu-id="bb20b-435">Nell'esempio seguente viene illustrato un elemento EntitySetMapping con due elementi **EntityTypeMapping** figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="bb20b-436">Nel primo elemento **EntityTypeMapping** , il tipo di entità **SchoolModel. Person** viene mappato alla tabella **Person** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="bb20b-437">Nel secondo elemento **EntityTypeMapping** , viene eseguito il mapping della funzionalità di aggiornamento del tipo **SchoolModel. Person** a un stored procedure, **UpdatePerson**, nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-438">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-438">Example</span></span>

<span data-ttu-id="bb20b-439">Nell'esempio successivo viene mostrato il mapping di una gerarchia di tipo in cui il tipo radice è astratto.</span><span class="sxs-lookup"><span data-stu-id="bb20b-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="bb20b-440">Si noti l'uso della sintassi `IsOfType` per gli attributi **typeName** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="bb20b-441">Elemento FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-442">L'elemento **FunctionImportMapping** in Mapping Specification Language (MSL) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure o una funzione nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="bb20b-443">Le importazioni di funzioni devono essere dichiarate nel modello concettuale mentre le stored procedure nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-444">Per ulteriori informazioni, vedere elemento FunctionImport (CSDL) e elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-445">Per impostazione predefinita, se un'importazione di funzioni restituisce un tipo di entità del modello concettuale o un tipo complesso, i nomi delle colonne restituiti dalla stored procedure sottostante devono corrispondere esattamente ai nomi delle proprietà nel tipo di modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="bb20b-446">Se i nomi delle colonne non corrispondono esattamente ai nomi delle proprietà, il mapping deve essere definito in un elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="bb20b-447">L'elemento **FunctionImportMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-448">ResultMapping (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-449">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-449">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-450">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="bb20b-451">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-451">Attribute Name</span></span>         | <span data-ttu-id="bb20b-452">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-452">Is Required</span></span> | <span data-ttu-id="bb20b-453">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-454">**FunctionImportName**</span></span> | <span data-ttu-id="bb20b-455">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-455">Yes</span></span>         | <span data-ttu-id="bb20b-456">Nome dell'importazione di funzioni nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="bb20b-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-457">**FunctionName**</span></span>       | <span data-ttu-id="bb20b-458">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-458">Yes</span></span>         | <span data-ttu-id="bb20b-459">Nome completo dello spazio dei nomi della funzione nel modello di archiviazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-460">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-460">Example</span></span>

<span data-ttu-id="bb20b-461">Di seguito viene riportato un esempio basato sul modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-461">The following example is based on the School model.</span></span> <span data-ttu-id="bb20b-462">Si consideri la funzione seguente nel modello di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="bb20b-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="bb20b-463">Si consideri anche questa importazione di funzioni nel modello concettuale:</span><span class="sxs-lookup"><span data-stu-id="bb20b-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="bb20b-464">Nell'esempio seguente viene illustrato un elemento **FunctionImportMapping** usato per eseguire il mapping tra la funzione e l'importazione di funzioni sopra elencate:</span><span class="sxs-lookup"><span data-stu-id="bb20b-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="bb20b-465">Elemento InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="bb20b-466">L'elemento **InsertFunction** in Mapping Specification Language (MSL) esegue il mapping della funzione di inserimento di un tipo di entità o di un'associazione nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="bb20b-467">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-468">Per ulteriori informazioni, vedere elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-469">Se non si esegue il mapping di tutte e tre le operazioni di inserimento, aggiornamento o eliminazione di un tipo di entità alle stored procedure, le operazioni non mappate avranno esito negativo se eseguite in fase di esecuzione e viene generata un'eccezione UpdateException.</span><span class="sxs-lookup"><span data-stu-id="bb20b-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="bb20b-470">L'elemento **InsertFunction** può essere un elemento figlio dell'elemento ModificationFunctionMapping e applicato all'elemento EntityTypeMapping o AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="bb20b-471">InsertFunction applicato a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="bb20b-472">Quando applicato all'elemento EntityTypeMapping, l'elemento **InsertFunction** esegue il mapping della funzione di inserimento di un tipo di entità nel modello concettuale a un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="bb20b-473">Quando viene applicato a un elemento **EntityTypeMapping** , l'elemento **InsertFunction** può includere i seguenti elementi figlio:</span><span class="sxs-lookup"><span data-stu-id="bb20b-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="bb20b-474">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="bb20b-475">ComplexProperty (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="bb20b-476">Risultante (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="bb20b-477">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-478">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-478">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-479">Nella tabella seguente vengono descritti gli attributi che possono essere applicati all'elemento **InsertFunction** quando vengono applicati a un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="bb20b-480">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-480">Attribute Name</span></span>            | <span data-ttu-id="bb20b-481">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-481">Is Required</span></span> | <span data-ttu-id="bb20b-482">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-483">**FunctionName**</span></span>          | <span data-ttu-id="bb20b-484">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-484">Yes</span></span>         | <span data-ttu-id="bb20b-485">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="bb20b-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="bb20b-486">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="bb20b-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="bb20b-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="bb20b-488">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-488">No</span></span>          | <span data-ttu-id="bb20b-489">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="bb20b-490">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-490">Example</span></span>

<span data-ttu-id="bb20b-491">L'esempio seguente è basato sul modello School e Mostra l'elemento **InsertFunction** usato per eseguire il mapping della funzione di inserimento del tipo di entità Person alla stored procedure **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="bb20b-492">Il stored procedure **InsertPerson** viene dichiarato nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="bb20b-493">InsertFunction applicato ad AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="bb20b-494">Quando applicato all'elemento AssociationSetMapping, l'elemento **InsertFunction** esegue il mapping della funzione di inserimento di un'associazione nel modello concettuale a un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="bb20b-495">L'elemento **InsertFunction** può avere gli elementi figlio seguenti quando viene applicato all'elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="bb20b-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-497">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-497">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-498">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **InsertFunction** quando viene applicato all'elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="bb20b-499">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-499">Attribute Name</span></span>            | <span data-ttu-id="bb20b-500">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-500">Is Required</span></span> | <span data-ttu-id="bb20b-501">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-502">**FunctionName**</span></span>          | <span data-ttu-id="bb20b-503">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-503">Yes</span></span>         | <span data-ttu-id="bb20b-504">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di inserimento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="bb20b-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="bb20b-505">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="bb20b-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="bb20b-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="bb20b-507">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-507">No</span></span>          | <span data-ttu-id="bb20b-508">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="bb20b-509">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-509">Example</span></span>

<span data-ttu-id="bb20b-510">L'esempio seguente è basato sul modello School e Mostra l'elemento **InsertFunction** usato per eseguire il mapping della funzione di inserimento dell'associazione **CourseInstructor** a **InsertCourseInstructor** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="bb20b-511">Il stored procedure **InsertCourseInstructor** viene dichiarato nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="bb20b-512">Elemento Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-513">L'elemento **mapping** in Mapping Specification Language (MSL) contiene informazioni per il mapping di oggetti definiti in un modello concettuale a un database (come descritto in un modello di archiviazione).</span><span class="sxs-lookup"><span data-stu-id="bb20b-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="bb20b-514">Per ulteriori informazioni, vedere Specification CSDL e SSDL Specification.</span><span class="sxs-lookup"><span data-stu-id="bb20b-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="bb20b-515">L'elemento **mapping** è l'elemento radice per una specifica di mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="bb20b-516">Lo spazio dei nomi XML per il mapping di specifiche https://schemas.microsoft.com/ado/2009/11/mapping/cs .</span><span class="sxs-lookup"><span data-stu-id="bb20b-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="bb20b-517">L'elemento Mapping può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="bb20b-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="bb20b-518">Alias (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="bb20b-519">EntityContainerMapping (esattamente uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="bb20b-520">I nomi di tipi di modelli concettuale e di archiviazione a cui viene fatto riferimento in MSL devono essere qualificati dai rispettivi nomi dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bb20b-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="bb20b-521">Per informazioni sul nome dello spazio dei nomi del modello concettuale, vedere elemento schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="bb20b-522">Per informazioni sul nome dello spazio dei nomi del modello di archiviazione, vedere elemento schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="bb20b-523">Gli alias per gli spazi dei nomi utilizzati in MSL possono essere definiti con l'elemento Alias.</span><span class="sxs-lookup"><span data-stu-id="bb20b-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-524">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-524">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-525">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento di **mapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="bb20b-526">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-526">Attribute Name</span></span> | <span data-ttu-id="bb20b-527">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-527">Is Required</span></span> | <span data-ttu-id="bb20b-528">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="bb20b-529">**Barra spaziatrice**</span><span class="sxs-lookup"><span data-stu-id="bb20b-529">**Space**</span></span>      | <span data-ttu-id="bb20b-530">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-530">Yes</span></span>         | <span data-ttu-id="bb20b-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-531">**C-S**.</span></span> <span data-ttu-id="bb20b-532">Si tratta di un valore fisso e non può essere modificato.</span><span class="sxs-lookup"><span data-stu-id="bb20b-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-533">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-533">Example</span></span>

<span data-ttu-id="bb20b-534">Nell'esempio seguente viene illustrato un elemento di **mapping** basato su una parte del modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="bb20b-535">Per ulteriori informazioni sul modello School, vedere Guida introduttiva (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="bb20b-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="bb20b-536">Elemento MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="bb20b-537">L'elemento **MappingFragment** in Mapping Specification Language (MSL) definisce il mapping tra le proprietà di un tipo di entità del modello concettuale e una tabella o una vista nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="bb20b-538">Per informazioni sui tipi di entità del modello concettuale e sulle tabelle o visualizzazioni del database sottostante, vedere Elemento EntityType (CSDL) e Elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="bb20b-539">**MappingFragment** può essere un elemento figlio dell'elemento EntityTypeMapping o EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="bb20b-540">L'elemento **MappingFragment** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-541">ComplexType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="bb20b-542">ScalarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="bb20b-543">Condition (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-544">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-544">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-545">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="bb20b-546">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-546">Attribute Name</span></span>          | <span data-ttu-id="bb20b-547">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-547">Is Required</span></span> | <span data-ttu-id="bb20b-548">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="bb20b-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="bb20b-550">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-550">Yes</span></span>         | <span data-ttu-id="bb20b-551">Nome della tabella o visualizzazione di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="bb20b-552">**Makecolumnsdistinct impostato**</span><span class="sxs-lookup"><span data-stu-id="bb20b-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="bb20b-553">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-553">No</span></span>          | <span data-ttu-id="bb20b-554">**True** o **false** , a seconda che vengano restituite solo righe distinte.</span><span class="sxs-lookup"><span data-stu-id="bb20b-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="bb20b-555">Se questo attributo è impostato su **true**, l'attributo **GenerateUpdateViews** dell'elemento EntityContainerMapping deve essere impostato su **false**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-556">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-556">Example</span></span>

<span data-ttu-id="bb20b-557">Nell'esempio seguente viene illustrato un elemento **MappingFragment** come figlio di un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="bb20b-558">In questo esempio, le proprietà del tipo **Course** nel modello concettuale vengono mappate alle colonne della tabella **Course** nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-559">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-559">Example</span></span>

<span data-ttu-id="bb20b-560">Nell'esempio seguente viene illustrato un elemento **MappingFragment** come figlio di un elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="bb20b-561">Come nell'esempio precedente, le proprietà del tipo **Course** nel modello concettuale vengono mappate alle colonne della tabella **Course** nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="bb20b-562">Elemento ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-563">L'elemento **ModificationFunctionMapping** in Mapping Specification Language (MSL) esegue il mapping delle funzioni di inserimento, aggiornamento ed eliminazione di un tipo di entità del modello concettuale alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="bb20b-564">L'elemento **ModificationFunctionMapping** può inoltre eseguire il mapping delle funzioni Insert e DELETE per le associazioni molti-a-molti nel modello concettuale alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="bb20b-565">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-566">Per ulteriori informazioni, vedere elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-567">Se non si esegue il mapping di tutte e tre le operazioni di inserimento, aggiornamento o eliminazione di un tipo di entità alle stored procedure, le operazioni non mappate avranno esito negativo se eseguite in fase di esecuzione e viene generata un'eccezione UpdateException.</span><span class="sxs-lookup"><span data-stu-id="bb20b-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="bb20b-568">Se esiste un mapping tra le funzioni di modifica per un'entità in una gerarchia di ereditarietà e stored procedure, è necessario eseguire il mapping delle funzioni di modifica per tutti i tipi nella gerarchia a stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="bb20b-569">L'elemento **ModificationFunctionMapping** può essere un elemento figlio dell'elemento EntityTypeMapping o AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="bb20b-570">L'elemento **ModificationFunctionMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-571">DeleteFunction (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="bb20b-572">InsertFunction (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="bb20b-573">UpdateFunction (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="bb20b-574">Nessun attributo applicabile all'elemento **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="bb20b-575">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-575">Example</span></span>

<span data-ttu-id="bb20b-576">Nell'esempio seguente viene illustrato il mapping del set di entità per il set di entità **people** nel modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="bb20b-577">Oltre al mapping delle colonne per il tipo di entità **Person** , viene visualizzato il mapping delle funzioni di inserimento, aggiornamento ed eliminazione del tipo **Person** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="bb20b-578">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-579">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-579">Example</span></span>

<span data-ttu-id="bb20b-580">Nell'esempio seguente viene illustrato il mapping del set di associazioni per il set di associazioni **CourseInstructor** nel modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="bb20b-581">Oltre al mapping delle colonne per l'associazione **CourseInstructor** , viene visualizzato il mapping delle funzioni di inserimento ed eliminazione dell'associazione **CourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="bb20b-582">Le funzioni a cui viene eseguito il mapping vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="bb20b-583">Elemento QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="bb20b-584">L'elemento **QueryView** in Mapping Specification Language (MSL) definisce un mapping di sola lettura tra un tipo di entità o un'associazione nel modello concettuale e una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="bb20b-585">Il mapping viene definito con una query Entity SQL valutata rispetto al modello di archiviazione e il set di risultati viene espresso in termini di entità o associazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="bb20b-586">Poiché le visualizzazioni di query sono di sola lettura, non è possibile utilizzare comandi di aggiornamento standard per aggiornare i tipi definiti da tali visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="bb20b-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="bb20b-587">A tale scopo è possibile utilizzare le funzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="bb20b-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="bb20b-588">Per altre informazioni, vedere Procedura: eseguire il mapping delle funzioni di modifica alle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-589">Nell'elemento **QueryView** , le espressioni Entity SQL che contengono **GroupBy**, aggregazioni di gruppo o proprietà di navigazione non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="bb20b-590">L'elemento **QueryView** può essere un elemento figlio dell'elemento EntitySetMapping o AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="bb20b-591">Nel primo caso, la visualizzazione di query definisce un mapping di sola lettura per un'entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="bb20b-592">Nel secondo caso, la visualizzazione di query definisce un mapping di sola lettura per un'associazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-593">Se l'elemento **AssociationSetMapping** è per un'associazione con un vincolo referenziale, l'elemento **AssociationSetMapping** viene ignorato.</span><span class="sxs-lookup"><span data-stu-id="bb20b-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="bb20b-594">Per ulteriori informazioni, vedere elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="bb20b-595">L'elemento **QueryView** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-596">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-596">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-597">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="bb20b-598">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-598">Attribute Name</span></span> | <span data-ttu-id="bb20b-599">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-599">Is Required</span></span> | <span data-ttu-id="bb20b-600">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-601">**TypeName**</span></span>   | <span data-ttu-id="bb20b-602">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-602">No</span></span>          | <span data-ttu-id="bb20b-603">Nome del tipo di modello concettuale mappato dalla visualizzazione di query.</span><span class="sxs-lookup"><span data-stu-id="bb20b-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-604">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-604">Example</span></span>

<span data-ttu-id="bb20b-605">Nell'esempio seguente viene illustrato l'elemento **QueryView** come figlio dell'elemento **EntitySetMapping** e viene definito un mapping della visualizzazione di query per il tipo di entità **Department** nel modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="bb20b-606">Poiché la query restituisce solo un subset dei membri del tipo **Department** nel modello di archiviazione, il tipo **Department** del modello School è stato modificato in base a questo mapping come segue:</span><span class="sxs-lookup"><span data-stu-id="bb20b-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-607">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-607">Example</span></span>

<span data-ttu-id="bb20b-608">Nell'esempio seguente viene illustrato l'elemento **QueryView** come figlio di un elemento **AssociationSetMapping** e viene definito un mapping di sola lettura per l'associazione `FK_Course_Department` nel modello School.</span><span class="sxs-lookup"><span data-stu-id="bb20b-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="bb20b-609">Comments</span><span class="sxs-lookup"><span data-stu-id="bb20b-609">Comments</span></span>

<span data-ttu-id="bb20b-610">È possibile definire le visualizzazioni di query per consentire gli scenari seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="bb20b-611">Definire un'entità nel modello concettuale che non includa tutte le proprietà dell'entità nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="bb20b-612">Sono incluse le proprietà che non dispongono di valori predefiniti e non supportano valori **null** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="bb20b-613">Eseguire il mapping delle colonne calcolate nel modello di archiviazione alle proprietà dei tipi di entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="bb20b-614">Definire un mapping in cui le condizioni utilizzate per partizionare le entità nel modello concettuale non siano basate sull'uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="bb20b-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="bb20b-615">Quando si specifica un mapping condizionale usando l'elemento **Condition** , la condizione fornita deve corrispondere al valore specificato.</span><span class="sxs-lookup"><span data-stu-id="bb20b-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="bb20b-616">Per ulteriori informazioni, vedere elemento Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="bb20b-617">Eseguire il mapping della stessa colonna nel modello di archiviazione a più tipi nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="bb20b-618">Eseguire il mapping di più tipi alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="bb20b-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="bb20b-619">Definire associazioni nel modello concettuale che non siano basate sulle chiavi esterne dello schema relazionale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="bb20b-620">Utilizzare la logica di business personalizzata per impostare il valore delle proprietà nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="bb20b-621">Ad esempio, è possibile eseguire il mapping del valore stringa "T" nell'origine dati a un valore **true**, un valore booleano, nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="bb20b-622">Definire filtri condizionali per i risultati della query.</span><span class="sxs-lookup"><span data-stu-id="bb20b-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="bb20b-623">Applicare ai dati del modello concettuale un numero di restrizioni minore di quelle applicate al modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="bb20b-624">Ad esempio, è possibile creare una proprietà nel modello concettuale Nullable anche se la colonna a cui è stato eseguito il mapping non supporta valori **null**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="bb20b-625">Le considerazioni seguenti riguardano la definizione delle visualizzazioni di query per le entità:</span><span class="sxs-lookup"><span data-stu-id="bb20b-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="bb20b-626">Le visualizzazioni di query sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="bb20b-626">Query views are read-only.</span></span> <span data-ttu-id="bb20b-627">È possibile applicare aggiornamenti alle entità solo utilizzando le funzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="bb20b-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="bb20b-628">Quando viene definito un tipo di entità da una visualizzazione di query, è necessario definire anche tutte le entità correlate dalle visualizzazioni di query.</span><span class="sxs-lookup"><span data-stu-id="bb20b-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="bb20b-629">Quando si esegue il mapping di un'associazione many-to-many a un'entità nel modello di archiviazione che rappresenta una tabella dei collegamenti nello schema relazionale, è necessario definire un elemento **QueryView** nell'elemento **AssociationSetMapping** per la tabella dei collegamenti.</span><span class="sxs-lookup"><span data-stu-id="bb20b-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="bb20b-630">È necessario definire le visualizzazioni di query per tutti i tipi di una gerarchia di tipi.</span><span class="sxs-lookup"><span data-stu-id="bb20b-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="bb20b-631">È possibile eseguire questa operazione nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="bb20b-632">Con un singolo elemento **QueryView** che specifica una singola query di Entity SQL che restituisce un'Unione di tutti i tipi di entità nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="bb20b-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="bb20b-633">Con un singolo elemento **QueryView** che specifica una singola query di Entity SQL che usa l'operatore case per restituire un tipo di entità specifico nella gerarchia in base a una condizione specifica.</span><span class="sxs-lookup"><span data-stu-id="bb20b-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="bb20b-634">Con un elemento **QueryView** aggiuntivo per un tipo specifico nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="bb20b-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="bb20b-635">In questo caso, usare l'attributo **typeName** dell'elemento **QueryView** per specificare il tipo di entità per ogni visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="bb20b-636">Quando viene definita una visualizzazione di query, non è possibile specificare l'attributo **StorageSetName** nell'elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="bb20b-637">Quando viene definita una visualizzazione di query, l'elemento **EntitySetMapping**non può contenere anche mapping di **Proprietà** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="bb20b-638">Elemento ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="bb20b-639">L'elemento **risultante** in Mapping Specification Language (MSL) esegue il mapping dei valori di colonna restituiti dalle stored procedure alle proprietà dell'entità nel modello concettuale quando viene eseguito il mapping delle funzioni di modifica del tipo di entità alle stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="bb20b-640">Quando, ad esempio, il valore di una colonna Identity viene restituito da un stored procedure di inserimento, l'elemento **risultante** esegue il mapping del valore restituito a una proprietà del tipo di entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="bb20b-641">L'elemento **risultante** può essere figlio dell'elemento InsertFunction o dell'elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="bb20b-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="bb20b-642">L'elemento **risultante** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-643">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-643">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-644">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **risultante** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="bb20b-645">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-645">Attribute Name</span></span> | <span data-ttu-id="bb20b-646">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-646">Is Required</span></span> | <span data-ttu-id="bb20b-647">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-648">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-648">**Name**</span></span>       | <span data-ttu-id="bb20b-649">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-649">Yes</span></span>         | <span data-ttu-id="bb20b-650">Nome della proprietà di entità nel modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="bb20b-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-651">**ColumnName**</span></span> | <span data-ttu-id="bb20b-652">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-652">Yes</span></span>         | <span data-ttu-id="bb20b-653">Nome della colonna di cui viene eseguito il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="bb20b-654">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-654">Example</span></span>

<span data-ttu-id="bb20b-655">L'esempio seguente è basato sul modello School e Mostra un elemento **InsertFunction** usato per eseguire il mapping della funzione di inserimento del tipo di entità **Person** alla stored procedure **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="bb20b-656">Il stored procedure **InsertPerson** è illustrato di seguito e viene dichiarato nel modello di archiviazione. Un elemento **risultante** viene utilizzato per eseguire il mapping di un valore di colonna restituito dal stored procedure (**NewPersonID**) a una proprietà del tipo di entità (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="bb20b-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="bb20b-657">L'istruzione Transact-SQL seguente descrive il stored procedure **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="bb20b-658">Elemento ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="bb20b-659">L'elemento **ResultMapping** in Mapping Specification Language (MSL) definisce il mapping tra un'importazione di funzioni nel modello concettuale e una stored procedure nel database sottostante quando sono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="bb20b-660">L'operazione di importazione di funzioni restituisce un tipo di entità del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="bb20b-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="bb20b-661">I nomi delle colonne restituite dalla stored procedure non corrispondono esattamente ai nomi delle proprietà del tipo di entità o del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="bb20b-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="bb20b-662">Per impostazione predefinita, il mapping tra le colonne restituite da una stored procedure e un tipo di entità o un tipo complesso è basato sui nomi delle colonne e delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="bb20b-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="bb20b-663">Se i nomi delle colonne non corrispondono esattamente ai nomi delle proprietà, è necessario utilizzare l'elemento **ResultMapping** per definire il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="bb20b-664">Per un esempio del mapping predefinito, vedere elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="bb20b-665">L'elemento **ResultMapping** è un elemento figlio dell'elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="bb20b-666">L'elemento **ResultMapping** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-667">EntityTypeMapping (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="bb20b-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-668">ComplexTypeMapping</span></span>

<span data-ttu-id="bb20b-669">Nessun attributo applicabile all'elemento **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="bb20b-670">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-670">Example</span></span>

<span data-ttu-id="bb20b-671">Si consideri la seguente stored procedure:</span><span class="sxs-lookup"><span data-stu-id="bb20b-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="bb20b-672">Si consideri inoltre il tipo di entità del modello concettuale seguente:</span><span class="sxs-lookup"><span data-stu-id="bb20b-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="bb20b-673">Per creare un'importazione di funzioni che restituisce istanze del tipo di entità precedente, il mapping tra le colonne restituite dal stored procedure e il tipo di entità deve essere definito in un elemento **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="bb20b-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="bb20b-674">Elemento ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="bb20b-675">L'elemento **ScalarProperty** in Mapping Specification Language (MSL) esegue il mapping di una proprietà su un tipo di entità del modello concettuale, un tipo complesso o un'associazione a una colonna della tabella o a un parametro di stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="bb20b-676">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-677">Per ulteriori informazioni, vedere elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="bb20b-678">L'elemento **ScalarProperty** può essere figlio dei seguenti elementi:</span><span class="sxs-lookup"><span data-stu-id="bb20b-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="bb20b-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="bb20b-679">MappingFragment</span></span>
-   <span data-ttu-id="bb20b-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="bb20b-680">InsertFunction</span></span>
-   <span data-ttu-id="bb20b-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="bb20b-681">UpdateFunction</span></span>
-   <span data-ttu-id="bb20b-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="bb20b-682">DeleteFunction</span></span>
-   <span data-ttu-id="bb20b-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-683">EndProperty</span></span>
-   <span data-ttu-id="bb20b-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="bb20b-684">ComplexProperty</span></span>
-   <span data-ttu-id="bb20b-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="bb20b-685">ResultMapping</span></span>

<span data-ttu-id="bb20b-686">Come figlio dell'elemento **MappingFragment**, **ComplexProperty**o **EndProperty** , l'elemento **ScalarProperty** esegue il mapping di una proprietà nel modello concettuale a una colonna nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="bb20b-687">Come figlio dell'elemento **InsertFunction**, **UpdateFunction**o **DeleteFunction** , l'elemento **ScalarProperty** esegue il mapping di una proprietà nel modello concettuale a un parametro stored procedure.</span><span class="sxs-lookup"><span data-stu-id="bb20b-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="bb20b-688">L'elemento **ScalarProperty** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="bb20b-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-689">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-689">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-690">Gli attributi che si applicano all'elemento **ScalarProperty** variano a seconda del ruolo dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="bb20b-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="bb20b-691">Nella tabella seguente vengono descritti gli attributi applicabili quando si utilizza l'elemento **ScalarProperty** per eseguire il mapping di una proprietà del modello concettuale a una colonna nel database:</span><span class="sxs-lookup"><span data-stu-id="bb20b-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="bb20b-692">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-692">Attribute Name</span></span> | <span data-ttu-id="bb20b-693">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-693">Is Required</span></span> | <span data-ttu-id="bb20b-694">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="bb20b-695">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-695">**Name**</span></span>       | <span data-ttu-id="bb20b-696">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-696">Yes</span></span>         | <span data-ttu-id="bb20b-697">Nome della proprietà del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="bb20b-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-698">**ColumnName**</span></span> | <span data-ttu-id="bb20b-699">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-699">Yes</span></span>         | <span data-ttu-id="bb20b-700">Nome della colonna della tabella di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="bb20b-701">Nella tabella seguente vengono descritti gli attributi applicabili all'elemento **ScalarProperty** quando viene utilizzato per eseguire il mapping di una proprietà del modello concettuale a un parametro stored procedure:</span><span class="sxs-lookup"><span data-stu-id="bb20b-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="bb20b-702">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-702">Attribute Name</span></span>    | <span data-ttu-id="bb20b-703">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-703">Is Required</span></span> | <span data-ttu-id="bb20b-704">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-705">**Nome**</span><span class="sxs-lookup"><span data-stu-id="bb20b-705">**Name**</span></span>          | <span data-ttu-id="bb20b-706">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-706">Yes</span></span>         | <span data-ttu-id="bb20b-707">Nome della proprietà del modello concettuale di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="bb20b-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-708">**ParameterName**</span></span> | <span data-ttu-id="bb20b-709">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-709">Yes</span></span>         | <span data-ttu-id="bb20b-710">Nome del parametro di cui è in corso il mapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="bb20b-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="bb20b-711">**Version**</span></span>       | <span data-ttu-id="bb20b-712">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-712">No</span></span>          | <span data-ttu-id="bb20b-713">**Corrente** o **originale** a seconda che il valore corrente o il valore originale della proprietà debba essere usato per i controlli di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="bb20b-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="bb20b-714">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-714">Example</span></span>

<span data-ttu-id="bb20b-715">Nell'esempio seguente viene illustrato l'elemento **ScalarProperty** usato in due modi:</span><span class="sxs-lookup"><span data-stu-id="bb20b-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="bb20b-716">Per eseguire il mapping delle proprietà del tipo di entità **Person** alle colonne della tabella **Person**.</span><span class="sxs-lookup"><span data-stu-id="bb20b-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="bb20b-717">Per eseguire il mapping delle proprietà del tipo di entità **Person** ai parametri del stored procedure **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="bb20b-718">Le stored procedure vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="bb20b-719">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-719">Example</span></span>

<span data-ttu-id="bb20b-720">Nell'esempio seguente viene illustrato l'elemento **ScalarProperty** utilizzato per eseguire il mapping delle funzioni Insert e Delete di un'associazione del modello concettuale alle stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="bb20b-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="bb20b-721">Le stored procedure vengono dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="bb20b-722">Elemento UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="bb20b-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="bb20b-723">L'elemento **UpdateFunction** in Mapping Specification Language (MSL) esegue il mapping della funzione di aggiornamento di un tipo di entità nel modello concettuale a una stored procedure nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb20b-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="bb20b-724">Le stored procedure a cui le funzioni di modifica sono mappate devono essere dichiarate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="bb20b-725">Per ulteriori informazioni, vedere elemento Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="bb20b-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="bb20b-726">Se non si esegue il mapping di tutte e tre le operazioni di inserimento, aggiornamento o eliminazione di un tipo di entità alle stored procedure, le operazioni non mappate avranno esito negativo se eseguite in fase di esecuzione e viene generata un'eccezione UpdateException.</span><span class="sxs-lookup"><span data-stu-id="bb20b-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="bb20b-727">L'elemento **UpdateFunction** può essere un elemento figlio dell'elemento ModificationFunctionMapping e applicato all'elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="bb20b-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="bb20b-728">L'elemento **UpdateFunction** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb20b-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="bb20b-729">AssociationEnd (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="bb20b-730">ComplexProperty (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="bb20b-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="bb20b-731">Risultante (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="bb20b-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="bb20b-732">ScarlarProperty (zero o più)</span><span class="sxs-lookup"><span data-stu-id="bb20b-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="bb20b-733">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="bb20b-733">Applicable Attributes</span></span>

<span data-ttu-id="bb20b-734">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="bb20b-735">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="bb20b-735">Attribute Name</span></span>            | <span data-ttu-id="bb20b-736">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="bb20b-736">Is Required</span></span> | <span data-ttu-id="bb20b-737">Valore</span><span class="sxs-lookup"><span data-stu-id="bb20b-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bb20b-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="bb20b-738">**FunctionName**</span></span>          | <span data-ttu-id="bb20b-739">Sì</span><span class="sxs-lookup"><span data-stu-id="bb20b-739">Yes</span></span>         | <span data-ttu-id="bb20b-740">Nome completo dello spazio dei nomi della stored procedure a cui la funzione di aggiornamento viene mappata.</span><span class="sxs-lookup"><span data-stu-id="bb20b-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="bb20b-741">La stored procedure deve essere dichiarata nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="bb20b-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="bb20b-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="bb20b-743">No</span><span class="sxs-lookup"><span data-stu-id="bb20b-743">No</span></span>          | <span data-ttu-id="bb20b-744">Nome del parametro di output che restituisce il numero di righe interessate.</span><span class="sxs-lookup"><span data-stu-id="bb20b-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="bb20b-745">Esempio</span><span class="sxs-lookup"><span data-stu-id="bb20b-745">Example</span></span>

<span data-ttu-id="bb20b-746">L'esempio seguente è basato sul modello School e Mostra l'elemento **UpdateFunction** usato per eseguire il mapping della funzione di aggiornamento del tipo di entità **Person** alla stored procedure **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="bb20b-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="bb20b-747">Il stored procedure **UpdatePerson** viene dichiarato nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="bb20b-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
