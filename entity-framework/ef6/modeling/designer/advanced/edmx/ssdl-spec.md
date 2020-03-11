---
title: Specifica SSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418725"
---
# <a name="ssdl-specification"></a><span data-ttu-id="4f6d3-102">Specifica SSDL</span><span class="sxs-lookup"><span data-stu-id="4f6d3-102">SSDL Specification</span></span>
<span data-ttu-id="4f6d3-103">Store Schema Definition Language (SSDL) è un linguaggio basato su XML che descrive il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="4f6d3-104">In un'applicazione Entity Framework, i metadati del modello di archiviazione vengono caricati da un file con estensione SSDL (scritto in SSDL) in un'istanza di System. Data. Metadata. Edm. StoreItemCollection ed è possibile accedervi tramite metodi nel Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="4f6d3-105">Entity Framework usa i metadati del modello di archiviazione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="4f6d3-106">Il Entity Framework Designer (EF designer) archivia le informazioni sul modello di archiviazione in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="4f6d3-107">In fase di compilazione il Entity Designer utilizza le informazioni contenute in un file con estensione edmx per creare il file con estensione SSDL necessario per Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="4f6d3-108">Le versioni di SSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="4f6d3-109">Versione SSDL</span><span class="sxs-lookup"><span data-stu-id="4f6d3-109">SSDL Version</span></span> | <span data-ttu-id="4f6d3-110">Spazio dei nomi XML</span><span class="sxs-lookup"><span data-stu-id="4f6d3-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="4f6d3-111">SSDL V1</span><span class="sxs-lookup"><span data-stu-id="4f6d3-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="4f6d3-112">SSDL V2</span><span class="sxs-lookup"><span data-stu-id="4f6d3-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="4f6d3-113">SSDL V3</span><span class="sxs-lookup"><span data-stu-id="4f6d3-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="4f6d3-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-114">Association Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-115">Un elemento **Association** in Store Schema Definition Language (SSDL) specifica le colonne della tabella che fanno parte di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="4f6d3-116">Due elementi End figlio obbligatori specificano le tabelle che costituiscono le entità finali dell'associazione e la molteplicità di ciascuna entità finale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="4f6d3-117">Un elemento ReferentialConstraint facoltativo specifica le entità finali principale e dipendente dell'associazione, nonché le colonne coinvolte.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="4f6d3-118">Se non è presente alcun elemento **ReferentialConstraint** , è necessario utilizzare un elemento AssociationSetMapping per specificare i mapping delle colonne per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="4f6d3-119">L'elemento **Association** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-120">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-121">End (esattamente due)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-121">End (exactly two)</span></span>
-   <span data-ttu-id="4f6d3-122">ReferentialConstraint (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-123">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-124">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-125">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="4f6d3-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-126">Attribute Name</span></span> | <span data-ttu-id="4f6d3-127">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-127">Is Required</span></span> | <span data-ttu-id="4f6d3-128">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-129">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-129">**Name**</span></span>       | <span data-ttu-id="4f6d3-130">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-130">Yes</span></span>         | <span data-ttu-id="4f6d3-131">Il nome del vincolo di chiave esterna corrispondente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-132">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="4f6d3-133">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-134">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-135">Example</span></span>

<span data-ttu-id="4f6d3-136">Nell'esempio seguente viene illustrato un elemento **Association** che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **\_FK CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="4f6d3-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="4f6d3-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-138">L'elemento **associativo** in Store Schema Definition Language (SSDL) rappresenta un vincolo di chiave esterna tra due tabelle nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="4f6d3-139">Le colonne della tabella che fanno parte del vincolo di chiave esterna vengono specificate in un elemento Association.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="4f6d3-140">L'elemento **Association** che corrisponde a **un elemento di associazione specificato è** specificato nell'attributo **Association** dell'elemento associationname.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="4f6d3-141">Il mapping dei set di associazioni SSDL viene eseguito in set di associazioni CSDL da un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="4f6d3-142">Tuttavia, se l'associazione CSDL per un determinato set di associazioni CSDL viene definita utilizzando un elemento ReferentialConstraint, non è necessario alcun elemento **AssociationSetMapping** corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="4f6d3-143">In questo caso, se è presente un elemento **AssociationSetMapping** , i mapping che definisce verranno sottoposti a override dall'elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="4f6d3-144">L'elemento **associationname** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-145">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-146">End (zero o due elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-146">End (zero or two)</span></span>
-   <span data-ttu-id="4f6d3-147">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-148">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-148">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-149">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="4f6d3-150">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-150">Attribute Name</span></span>  | <span data-ttu-id="4f6d3-151">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-151">Is Required</span></span> | <span data-ttu-id="4f6d3-152">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-153">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-153">**Name**</span></span>        | <span data-ttu-id="4f6d3-154">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-154">Yes</span></span>         | <span data-ttu-id="4f6d3-155">Nome del vincolo di chiave esterna rappresentato dal set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="4f6d3-156">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-156">**Association**</span></span> | <span data-ttu-id="4f6d3-157">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-157">Yes</span></span>         | <span data-ttu-id="4f6d3-158">Nome dell'associazione che definisce le colonne che fanno parte del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-159">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="4f6d3-160">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-161">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-162">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-162">Example</span></span>

<span data-ttu-id="4f6d3-163">Nell'esempio seguente viene illustrato un elemento di **associazione** che rappresenta il vincolo di chiave esterna `FK_CustomerOrders` nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="4f6d3-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-165">L'elemento **CollectionType** in Store Schema Definition Language (SSDL) specifica che il tipo restituito di una funzione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="4f6d3-166">L'elemento **CollectionType** è figlio dell'elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="4f6d3-167">Il tipo di raccolta viene specificato tramite l'elemento figlio RowType:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="4f6d3-168">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="4f6d3-169">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-170">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-171">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-171">Example</span></span>

<span data-ttu-id="4f6d3-172">Nell'esempio seguente viene illustrata una funzione che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="4f6d3-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-174">L'elemento **CommandText** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento Function che consente di definire un'istruzione SQL che viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="4f6d3-175">L'elemento **CommandText** consente di aggiungere una funzionalità simile a una stored procedure nel database, ma l'elemento **CommandText** viene definito nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="4f6d3-176">L'elemento **CommandText** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="4f6d3-177">Il corpo dell'elemento **CommandText** deve essere un'istruzione SQL valida per il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="4f6d3-178">Nessun attributo applicabile all'elemento **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-179">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-179">Example</span></span>

<span data-ttu-id="4f6d3-180">Nell'esempio seguente viene illustrato un elemento **Function** con un elemento **CommandText** figlio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="4f6d3-181">Esporre la funzione **UpdateProductInOrder** come metodo in ObjectContext mediante l'importazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="4f6d3-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-183">L'elemento **DefiningQuery** in Store Schema Definition Language (SSDL) consente di eseguire un'istruzione SQL direttamente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="4f6d3-184">L'elemento **DefiningQuery** viene comunemente utilizzato come una vista di database, ma la vista è definita nel modello di archiviazione anziché nel database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="4f6d3-185">La vista definita in un elemento **DefiningQuery** può essere mappata a un tipo di entità nel modello concettuale tramite un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="4f6d3-186">Questi mapping sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-186">These mappings are read-only.</span></span>  

<span data-ttu-id="4f6d3-187">Nella sintassi SSDL seguente viene illustrata la dichiarazione di un oggetto **EntitySet** seguito dall'elemento **DefiningQuery** che contiene una query utilizzata per recuperare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="4f6d3-188">È possibile utilizzare le stored procedure nel Entity Framework per abilitare gli scenari di lettura e scrittura sulle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="4f6d3-189"> È possibile utilizzare una vista origine dati o una vista Entity SQL come tabella di base per il recupero dei dati e per l'elaborazione delle modifiche da parte delle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="4f6d3-190">È possibile usare l'elemento **DefiningQuery** per la destinazione Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="4f6d3-191">Sebbene SQL Server Compact 3,5 non supporti stored procedure, è possibile implementare una funzionalità simile con l'elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="4f6d3-192">Un'altra situazione in cui può essere utile è nella creazione di stored procedure per risolvere una mancata corrispondenza tra i tipi di dati utilizzati nel linguaggio di programmazione e quelli dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="4f6d3-193">È possibile scrivere un **DefiningQuery** che accetta un determinato set di parametri e quindi chiama un stored procedure con un set di parametri diverso, ad esempio un stored procedure che elimina i dati.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="4f6d3-194">Elemento Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-195">L'elemento **dipendente** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento ReferentialConstraint che definisce l'entità finale dipendente di un vincolo di chiave esterna (detto anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="4f6d3-196">L'elemento **dipendente** specifica la colonna (o le colonne) in una tabella che fa riferimento a una colonna di chiave primaria (o a colonne).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="4f6d3-197">Gli elementi **PropertyRef** specificano le colonne a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="4f6d3-198">L'elemento Principal specifica le colonne chiave primaria a cui fanno riferimento le colonne specificate nell'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="4f6d3-199">L'elemento **dipendente** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-200">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="4f6d3-201">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-202">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-202">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-203">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="4f6d3-204">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-204">Attribute Name</span></span> | <span data-ttu-id="4f6d3-205">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-205">Is Required</span></span> | <span data-ttu-id="4f6d3-206">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-207">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-207">**Role**</span></span>       | <span data-ttu-id="4f6d3-208">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-208">Yes</span></span>         | <span data-ttu-id="4f6d3-209">Lo stesso valore dell'attributo **Role** (se utilizzato) dell'elemento End corrispondente; in caso contrario, il nome della tabella che contiene la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-210">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="4f6d3-211">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-212">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-213">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-213">Example</span></span>

<span data-ttu-id="4f6d3-214">Nell'esempio seguente viene illustrato un elemento Association che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **\_FK CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="4f6d3-215">L'elemento **dipendente** specifica la colonna **CustomerID** della tabella **Order** come entità finale dipendente del vincolo.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="4f6d3-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-217">L'elemento **Documentation** in Store Schema Definition Language (SSDL) può essere utilizzato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="4f6d3-218">L'elemento **Documentation** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-219">**Riepilogo**: breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="4f6d3-220">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-220">(zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-221">**LongDescription**: Descrizione completa dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="4f6d3-222">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-223">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-223">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-224">All'elemento **Documentation** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="4f6d3-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-227">Example</span></span>

<span data-ttu-id="4f6d3-228">Nell'esempio seguente viene illustrato l'elemento **Documentation** come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="4f6d3-229">Elemento End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-229">End Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-230">L'elemento **end** in Store Schema Definition Language (SSDL) specifica la tabella e il numero di righe a un'estremità di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="4f6d3-231">L'elemento **end** può essere un elemento figlio dell'elemento Association o dell'elemento associationname.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="4f6d3-232">In entrambi i casi, gli elementi figlio possibili e gli attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="4f6d3-233">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="4f6d3-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="4f6d3-234">Un elemento **end** (come figlio dell'elemento **Association** ) specifica la tabella e il numero di righe alla fine di un vincolo FOREIGN KEY con rispettivamente il **tipo** e gli attributi di **molteplicità** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="4f6d3-235">Le entità finali del vincolo di chiave esterna sono definite come parte dell'associazione SSDL. L'associazione SSDL deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="4f6d3-236">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-237">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-238">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-239">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-240">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-240">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-241">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **finale** quando è figlio di un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="4f6d3-242">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-242">Attribute Name</span></span>   | <span data-ttu-id="4f6d3-243">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-243">Is Required</span></span> | <span data-ttu-id="4f6d3-244">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-245">**Type**</span></span>         | <span data-ttu-id="4f6d3-246">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-246">Yes</span></span>         | <span data-ttu-id="4f6d3-247">Il nome completo del set di entità SSDL che si trova in corrispondenza dell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="4f6d3-248">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-248">**Role**</span></span>         | <span data-ttu-id="4f6d3-249">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-249">No</span></span>          | <span data-ttu-id="4f6d3-250">Valore dell'attributo **Role** nell'elemento Principal o dipendente dell'elemento ReferentialConstraint corrispondente (se usato).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="4f6d3-251">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-251">**Multiplicity**</span></span> | <span data-ttu-id="4f6d3-252">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-252">Yes</span></span>         | <span data-ttu-id="4f6d3-253">**1**, **0.. 1**o **\*** in base al numero di righe che possono trovarsi alla fine del vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="4f6d3-254">**1** indica che esiste esattamente una riga nell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="4f6d3-255">**0.. 1** indica che l'entità finale del vincolo di chiave esterna è pari a zero o a una riga.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="4f6d3-256">**\*** indica che sono presenti zero, una o più righe nell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-257">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="4f6d3-258">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-259">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="4f6d3-260">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-260">Example</span></span>

<span data-ttu-id="4f6d3-261">Nell'esempio seguente viene illustrato un elemento **Association** che definisce il vincolo di chiave esterna **\_FK CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="4f6d3-262">I valori di **molteplicità** specificati in ogni elemento **end** indicano che molte righe della tabella **Orders** possono essere associate a una riga nella tabella **Customers** , ma solo una riga nella tabella **Customers** può essere associata a una riga nella tabella **Orders** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="4f6d3-263">Inoltre, l'elemento **OnDelete** indica che tutte le righe della tabella **Orders** che fanno riferimento a una determinata riga nella tabella **Customers** verranno eliminate se la riga della tabella **Customers** viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="4f6d3-264">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="4f6d3-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="4f6d3-265">L'elemento **end** (come figlio dell'elemento **associationname** ) specifica una tabella in corrispondenza di un'entità finale di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="4f6d3-266">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-267">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-268">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-269">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-269">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-270">Nella tabella seguente vengono descritti gli attributi che possono essere applicati all'elemento **finale** quando è figlio di un elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="4f6d3-271">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-271">Attribute Name</span></span> | <span data-ttu-id="4f6d3-272">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-272">Is Required</span></span> | <span data-ttu-id="4f6d3-273">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-274">**EntitySet**</span></span>  | <span data-ttu-id="4f6d3-275">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-275">Yes</span></span>         | <span data-ttu-id="4f6d3-276">Il nome del set di entità SSDL in corrispondenza dell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="4f6d3-277">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-277">**Role**</span></span>       | <span data-ttu-id="4f6d3-278">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-278">No</span></span>          | <span data-ttu-id="4f6d3-279">Valore di uno degli attributi **Role** specificati in un elemento **end** dell'elemento Association corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-280">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="4f6d3-281">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-282">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="4f6d3-283">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-283">Example</span></span>

<span data-ttu-id="4f6d3-284">Nell'esempio seguente viene illustrato un elemento **EntityContainer** con un elemento di **associazione** con due elementi **end** :</span><span class="sxs-lookup"><span data-stu-id="4f6d3-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="4f6d3-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-286">Un elemento **EntityContainer** in Store Schema Definition Language (SSDL) descrive la struttura dell'origine dati sottostante in un'applicazione Entity Framework: i set di entità SSDL (definiti negli elementi EntitySet) rappresentano le tabelle in un database, i tipi di entità SSDL (definiti negli elementi EntityType) rappresentano le righe in una tabella e i set di associazioni (definiti in elementi di associazioni) rappresentano vincoli di chiave esterna in un database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="4f6d3-287">Un contenitore di entità del modello di archiviazione esegue il mapping a un contenitore di entità del modello concettuale attraverso l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="4f6d3-288">Un elemento **EntityContainer** può avere uno o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="4f6d3-289">Se è presente un elemento di **documentazione** , deve precedere tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="4f6d3-290">Un elemento **EntityContainer** può avere zero o più degli elementi figlio seguenti (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="4f6d3-291">EntitySet</span></span>
-   <span data-ttu-id="4f6d3-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="4f6d3-292">AssociationSet</span></span>
-   <span data-ttu-id="4f6d3-293">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="4f6d3-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-294">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-294">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-295">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="4f6d3-296">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-296">Attribute Name</span></span> | <span data-ttu-id="4f6d3-297">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-297">Is Required</span></span> | <span data-ttu-id="4f6d3-298">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-299">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-299">**Name**</span></span>       | <span data-ttu-id="4f6d3-300">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-300">Yes</span></span>         | <span data-ttu-id="4f6d3-301">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-301">The name of the entity container.</span></span> <span data-ttu-id="4f6d3-302">Il nome non può contenere caratteri punto (.).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-303">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="4f6d3-304">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-305">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-306">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-306">Example</span></span>

<span data-ttu-id="4f6d3-307">Nell'esempio seguente viene illustrato un elemento **EntityContainer** che definisce due set di entità e un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="4f6d3-308">I nomi dei tipi di entità e associazione sono qualificati dal nome dello spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="4f6d3-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-310">Un elemento **EntitySet** in Store Schema Definition Language (SSDL) rappresenta una tabella o una vista nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="4f6d3-311">Un elemento EntityType in SSDL rappresenta una riga nella tabella o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="4f6d3-312">L'attributo **EntityType** di un elemento **EntitySet** specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="4f6d3-313">Il mapping tra un set di entità CSDL e un set di entità SSDL viene specificato in un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="4f6d3-314">L'elemento **EntitySet** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-315">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-316">DefiningQuery (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-317">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="4f6d3-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-318">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-319">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="4f6d3-320">Alcuni attributi (non elencati qui) possono essere qualificati con l'alias del **negozio** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="4f6d3-321">Questi attributi vengono utilizzati dalla procedura guidata Aggiorna modello in caso di aggiornamento di un modello.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="4f6d3-322">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-322">Attribute Name</span></span> | <span data-ttu-id="4f6d3-323">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-323">Is Required</span></span> | <span data-ttu-id="4f6d3-324">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-325">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-325">**Name**</span></span>       | <span data-ttu-id="4f6d3-326">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-326">Yes</span></span>         | <span data-ttu-id="4f6d3-327">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="4f6d3-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-328">**EntityType**</span></span> | <span data-ttu-id="4f6d3-329">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-329">Yes</span></span>         | <span data-ttu-id="4f6d3-330">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="4f6d3-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-331">**Schema**</span></span>     | <span data-ttu-id="4f6d3-332">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-332">No</span></span>          | <span data-ttu-id="4f6d3-333">Schema del database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="4f6d3-334">**Tabella**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-334">**Table**</span></span>      | <span data-ttu-id="4f6d3-335">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-335">No</span></span>          | <span data-ttu-id="4f6d3-336">Tabella del database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="4f6d3-337">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="4f6d3-338">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-339">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-340">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-340">Example</span></span>

<span data-ttu-id="4f6d3-341">Nell'esempio seguente viene illustrato un elemento **EntityContainer** che dispone di due elementi **EntitySet** e di un elemento di **associazione** :</span><span class="sxs-lookup"><span data-stu-id="4f6d3-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="4f6d3-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-343">Un elemento **EntityType** in Store Schema Definition Language (SSDL) rappresenta una riga in una tabella o vista del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="4f6d3-344">Un elemento EntitySet in SSDL rappresenta la tabella o la vista in cui si trova la riga.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="4f6d3-345">L'attributo **EntityType** di un elemento **EntitySet** specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="4f6d3-346">Il mapping tra un tipo di entità SSDL e un tipo di entità CSDL viene specificato in un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="4f6d3-347">L'elemento **EntityType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-348">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-349">Key (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="4f6d3-350">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="4f6d3-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-351">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-351">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-352">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="4f6d3-353">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-353">Attribute Name</span></span> | <span data-ttu-id="4f6d3-354">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-354">Is Required</span></span> | <span data-ttu-id="4f6d3-355">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-356">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-356">**Name**</span></span>       | <span data-ttu-id="4f6d3-357">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-357">Yes</span></span>         | <span data-ttu-id="4f6d3-358">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-358">The name of the entity type.</span></span> <span data-ttu-id="4f6d3-359">Questo valore corrisponde generalmente al nome della tabella in cui il tipo di entità rappresenta una riga.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="4f6d3-360">Questo valore non può contenere punti (.).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-361">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="4f6d3-362">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-363">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-364">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-364">Example</span></span>

<span data-ttu-id="4f6d3-365">Nell'esempio seguente viene illustrato un elemento **EntityType** con due proprietà:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="4f6d3-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-366">Function Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-367">L'elemento **Function** in Store Schema Definition Language (SSDL) specifica una stored procedure esistente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="4f6d3-368">L'elemento **Function** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-369">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-370">Parameter (zero o più)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="4f6d3-371">CommandText (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-372">ReturnType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="4f6d3-373">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="4f6d3-374">Un tipo restituito per una funzione deve essere specificato con l'elemento **returnType** o l'attributo **returnType** (vedere di seguito), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="4f6d3-375">È possibile importare stored procedure specificate nel modello di archiviazione nel modello concettuale di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="4f6d3-376">Per ulteriori informazioni, vedere [esecuzione di query con stored procedure](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="4f6d3-377">L'elemento **Function** può essere utilizzato anche per definire funzioni personalizzate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-378">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-378">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-379">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="4f6d3-380">Alcuni attributi (non elencati qui) possono essere qualificati con l'alias del **negozio** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="4f6d3-381">Questi attributi vengono utilizzati dalla procedura guidata Aggiorna modello in caso di aggiornamento di un modello.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="4f6d3-382">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-382">Attribute Name</span></span>             | <span data-ttu-id="4f6d3-383">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-383">Is Required</span></span> | <span data-ttu-id="4f6d3-384">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-385">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-385">**Name**</span></span>                   | <span data-ttu-id="4f6d3-386">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-386">Yes</span></span>         | <span data-ttu-id="4f6d3-387">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="4f6d3-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-388">**ReturnType**</span></span>             | <span data-ttu-id="4f6d3-389">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-389">No</span></span>          | <span data-ttu-id="4f6d3-390">Tipo restituito della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="4f6d3-391">**Aggregata**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-391">**Aggregate**</span></span>              | <span data-ttu-id="4f6d3-392">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-392">No</span></span>          | <span data-ttu-id="4f6d3-393">**True** se il stored procedure restituisce un valore di aggregazione. in caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="4f6d3-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-394">**BuiltIn**</span></span>                | <span data-ttu-id="4f6d3-395">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-395">No</span></span>          | <span data-ttu-id="4f6d3-396">**True** se la funzione è una funzione incorporata<sup>1</sup> . in caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="4f6d3-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="4f6d3-398">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-398">No</span></span>          | <span data-ttu-id="4f6d3-399">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="4f6d3-400">**Attributo NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-400">**NiladicFunction**</span></span>        | <span data-ttu-id="4f6d3-401">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-401">No</span></span>          | <span data-ttu-id="4f6d3-402">**True** se la funzione è una funzione senza parametri<sup>2</sup> . In caso contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="4f6d3-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-403">**IsComposable**</span></span>           | <span data-ttu-id="4f6d3-404">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-404">No</span></span>          | <span data-ttu-id="4f6d3-405">**True** se la funzione è una funzione componibile<sup>3</sup> . In caso contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="4f6d3-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="4f6d3-407">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-407">No</span></span>          | <span data-ttu-id="4f6d3-408">Enumerazione che definisce la semantica dei tipi utilizzata per risolvere gli overload della funzione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="4f6d3-409">L'enumerazione è definita nel file manifesto del provider per ogni definizione di funzione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="4f6d3-410">Il valore predefinito è **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="4f6d3-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-411">**Schema**</span></span>                 | <span data-ttu-id="4f6d3-412">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-412">No</span></span>          | <span data-ttu-id="4f6d3-413">Nome dello schema in cui viene definita la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="4f6d3-414"><sup>1</sup> una funzione predefinita è una funzione definita nel database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="4f6d3-415">Per informazioni sulle funzioni definite nel modello di archiviazione, vedere elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="4f6d3-416"><sup>2</sup> una funzione senza parametri è una funzione che non accetta parametri e, quando viene chiamato, non richiede le parentesi.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="4f6d3-417"><sup>3</sup> due funzioni sono componibili se l'output di una funzione può essere l'input per l'altra funzione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="4f6d3-418">All'elemento **Function** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="4f6d3-419">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-420">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-421">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-421">Example</span></span>

<span data-ttu-id="4f6d3-422">Nell'esempio seguente viene illustrato un elemento **Function** che corrisponde alla stored procedure **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="4f6d3-423">La stored procedure accetta due parametri e non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="4f6d3-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-424">Key Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-425">L'elemento **Key** in Store Schema Definition Language (SSDL) rappresenta la chiave primaria di una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="4f6d3-426">**Key** è un elemento figlio di un elemento EntityType, che rappresenta una riga in una tabella.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="4f6d3-427">La chiave primaria è definita nell'elemento **chiave** facendo riferimento a uno o più elementi Property definiti nell'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="4f6d3-428">L'elemento **Key** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-429">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="4f6d3-430">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="4f6d3-430">Annotation elements</span></span>

<span data-ttu-id="4f6d3-431">Nessun attributo applicabile all'elemento **chiave** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-432">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-432">Example</span></span>

<span data-ttu-id="4f6d3-433">Nell'esempio seguente viene illustrato un elemento **EntityType** con una chiave che fa riferimento a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="4f6d3-434">Elemento OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-435">L'elemento **OnDelete** in Store Schema Definition Language (SSDL) riflette il comportamento del database quando viene eliminata una riga che fa parte di un vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="4f6d3-436">Se l'azione è impostata su **Cascade**, verranno eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="4f6d3-437">Se l'azione è impostata su **None**, non vengono eliminate anche le righe che fanno riferimento a una riga eliminata.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="4f6d3-438">Un elemento **OnDelete** è un elemento figlio di un elemento End.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="4f6d3-439">Un elemento **OnDelete** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-440">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-441">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-442">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-442">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-443">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="4f6d3-444">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-444">Attribute Name</span></span> | <span data-ttu-id="4f6d3-445">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-445">Is Required</span></span> | <span data-ttu-id="4f6d3-446">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-447">**Azione**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-447">**Action**</span></span>     | <span data-ttu-id="4f6d3-448">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-448">Yes</span></span>         | <span data-ttu-id="4f6d3-449">**Cascade** o **None**.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-449">**Cascade** or **None**.</span></span> <span data-ttu-id="4f6d3-450">(Il valore con **restrizioni** è valido ma ha lo stesso comportamento di **None**).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-451">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="4f6d3-452">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-453">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-454">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-454">Example</span></span>

<span data-ttu-id="4f6d3-455">Nell'esempio seguente viene illustrato un elemento **Association** che definisce il vincolo di chiave esterna **\_FK CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="4f6d3-456">L'elemento **OnDelete** indica che tutte le righe della tabella **Orders** che fanno riferimento a una determinata riga della tabella **Customers** verranno eliminate se la riga della tabella **Customers** viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="4f6d3-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-458">L'elemento **Parameter** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento Function che specifica i parametri per un stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="4f6d3-459">L'elemento **Parameter** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-460">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-461">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-462">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-462">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-463">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="4f6d3-464">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-464">Attribute Name</span></span> | <span data-ttu-id="4f6d3-465">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-465">Is Required</span></span> | <span data-ttu-id="4f6d3-466">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-467">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-467">**Name**</span></span>       | <span data-ttu-id="4f6d3-468">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-468">Yes</span></span>         | <span data-ttu-id="4f6d3-469">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="4f6d3-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-470">**Type**</span></span>       | <span data-ttu-id="4f6d3-471">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-471">Yes</span></span>         | <span data-ttu-id="4f6d3-472">Tipo di parametro.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="4f6d3-473">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-473">**Mode**</span></span>       | <span data-ttu-id="4f6d3-474">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-474">No</span></span>          | <span data-ttu-id="4f6d3-475">**In**, **out**o **InOut** a seconda che il parametro sia un parametro di input, di output o di input/output.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="4f6d3-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-476">**MaxLength**</span></span>  | <span data-ttu-id="4f6d3-477">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-477">No</span></span>          | <span data-ttu-id="4f6d3-478">Lunghezza massima del parametro.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="4f6d3-479">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-479">**Precision**</span></span>  | <span data-ttu-id="4f6d3-480">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-480">No</span></span>          | <span data-ttu-id="4f6d3-481">Precisione del parametro</span><span class="sxs-lookup"><span data-stu-id="4f6d3-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="4f6d3-482">**Ridimensionare**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-482">**Scale**</span></span>      | <span data-ttu-id="4f6d3-483">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-483">No</span></span>          | <span data-ttu-id="4f6d3-484">Scalabilità del parametro</span><span class="sxs-lookup"><span data-stu-id="4f6d3-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="4f6d3-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-485">**SRID**</span></span>       | <span data-ttu-id="4f6d3-486">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-486">No</span></span>          | <span data-ttu-id="4f6d3-487">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="4f6d3-488">Valido solo per i parametri dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="4f6d3-489">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-490">All'elemento **Parameter** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="4f6d3-491">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-492">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-493">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-493">Example</span></span>

<span data-ttu-id="4f6d3-494">Nell'esempio seguente viene illustrato un elemento **Function** con due elementi **Parameter** che specificano i parametri di input:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="4f6d3-495">Elemento Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-496">L'elemento **Principal** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento ReferentialConstraint che definisce l'entità finale principale di un vincolo FOREIGN KEY (detto anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="4f6d3-497">L'elemento **Principal** specifica la colonna o le colonne di chiave primaria in una tabella a cui fa riferimento un'altra colonna o colonne.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="4f6d3-498">Gli elementi **PropertyRef** specificano le colonne a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="4f6d3-499">L'elemento dipendente specifica le colonne che fanno riferimento alle colonne di chiave primaria specificate nell'elemento **Principal** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="4f6d3-500">L'elemento **Principal** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="4f6d3-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="4f6d3-501">PropertyRef (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="4f6d3-502">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-503">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-503">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-504">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Principal** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="4f6d3-505">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-505">Attribute Name</span></span> | <span data-ttu-id="4f6d3-506">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-506">Is Required</span></span> | <span data-ttu-id="4f6d3-507">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-508">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-508">**Role**</span></span>       | <span data-ttu-id="4f6d3-509">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-509">Yes</span></span>         | <span data-ttu-id="4f6d3-510">Lo stesso valore dell'attributo **Role** (se utilizzato) dell'elemento End corrispondente; in caso contrario, il nome della tabella che contiene la colonna a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-511">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **principale** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="4f6d3-512">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-513">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-514">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-514">Example</span></span>

<span data-ttu-id="4f6d3-515">Nell'esempio seguente viene illustrato un elemento Association che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **\_FK CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="4f6d3-516">L'elemento **Principal** specifica la colonna **CustomerID** della tabella **Customer** come entità finale principale del vincolo.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="4f6d3-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-517">Property Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-518">L'elemento **Property** in Store Schema Definition Language (SSDL) rappresenta una colonna in una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="4f6d3-519">Gli elementi **Proprietà** sono elementi figlio degli elementi EntityType, che rappresentano le righe in una tabella.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="4f6d3-520">Ogni elemento **Property** definito in un elemento **EntityType** rappresenta una colonna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="4f6d3-521">Un elemento **Property** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-522">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-522">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-523">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="4f6d3-524">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-524">Attribute Name</span></span>            | <span data-ttu-id="4f6d3-525">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-525">Is Required</span></span> | <span data-ttu-id="4f6d3-526">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-527">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-527">**Name**</span></span>                  | <span data-ttu-id="4f6d3-528">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-528">Yes</span></span>         | <span data-ttu-id="4f6d3-529">Nome della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="4f6d3-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-530">**Type**</span></span>                  | <span data-ttu-id="4f6d3-531">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-531">Yes</span></span>         | <span data-ttu-id="4f6d3-532">Tipo della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="4f6d3-533">**Ammette i valori Null**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-533">**Nullable**</span></span>              | <span data-ttu-id="4f6d3-534">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-534">No</span></span>          | <span data-ttu-id="4f6d3-535">**True** (valore predefinito) o **false** a seconda che la colonna corrispondente possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="4f6d3-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-536">**DefaultValue**</span></span>          | <span data-ttu-id="4f6d3-537">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-537">No</span></span>          | <span data-ttu-id="4f6d3-538">Valore predefinito della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="4f6d3-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-539">**MaxLength**</span></span>             | <span data-ttu-id="4f6d3-540">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-540">No</span></span>          | <span data-ttu-id="4f6d3-541">Lunghezza massima della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="4f6d3-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-542">**FixedLength**</span></span>           | <span data-ttu-id="4f6d3-543">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-543">No</span></span>          | <span data-ttu-id="4f6d3-544">**True** o **false** a seconda che il valore della colonna corrispondente venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="4f6d3-545">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-545">**Precision**</span></span>             | <span data-ttu-id="4f6d3-546">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-546">No</span></span>          | <span data-ttu-id="4f6d3-547">Precisione della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="4f6d3-548">**Ridimensionare**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-548">**Scale**</span></span>                 | <span data-ttu-id="4f6d3-549">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-549">No</span></span>          | <span data-ttu-id="4f6d3-550">Scala della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="4f6d3-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-551">**Unicode**</span></span>               | <span data-ttu-id="4f6d3-552">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-552">No</span></span>          | <span data-ttu-id="4f6d3-553">**True** o **false** a seconda che il valore della colonna corrispondente venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="4f6d3-554">**Regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-554">**Collation**</span></span>             | <span data-ttu-id="4f6d3-555">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-555">No</span></span>          | <span data-ttu-id="4f6d3-556">Stringa che specifica la sequenza di collazione da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="4f6d3-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-557">**SRID**</span></span>                  | <span data-ttu-id="4f6d3-558">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-558">No</span></span>          | <span data-ttu-id="4f6d3-559">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="4f6d3-560">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="4f6d3-561">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="4f6d3-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="4f6d3-563">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-563">No</span></span>          | <span data-ttu-id="4f6d3-564">**None**, **Identity** (se il valore della colonna corrispondente è un'identità generata nel database) o **calcolata** (se il valore della colonna corrispondente viene calcolato nel database).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="4f6d3-565">Non valido per le proprietà RowType.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-566">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="4f6d3-567">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-568">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-569">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-569">Example</span></span>

<span data-ttu-id="4f6d3-570">Nell'esempio seguente viene illustrato un elemento **EntityType** con due elementi **Proprietà** figlio:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="4f6d3-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-572">L'elemento **PropertyRef** in Store Schema Definition Language (SSDL) fa riferimento a una proprietà definita in un elemento EntityType per indicare che la proprietà eseguirà uno dei ruoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="4f6d3-573">Far parte della chiave primaria della tabella rappresentata da **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="4f6d3-574">Per definire una chiave primaria, è possibile usare uno o più elementi **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="4f6d3-575">Per ulteriori informazioni, vedere Elemento Key.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="4f6d3-576">Rappresenterà l'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="4f6d3-577">Per ulteriori informazioni, vedere Elemento ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="4f6d3-578">L'elemento **PropertyRef** può avere solo gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="4f6d3-579">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-580">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="4f6d3-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-581">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-581">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-582">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="4f6d3-583">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-583">Attribute Name</span></span> | <span data-ttu-id="4f6d3-584">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-584">Is Required</span></span> | <span data-ttu-id="4f6d3-585">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="4f6d3-586">**Nome**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-586">**Name**</span></span>       | <span data-ttu-id="4f6d3-587">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-587">Yes</span></span>         | <span data-ttu-id="4f6d3-588">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="4f6d3-589">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="4f6d3-590">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="4f6d3-591">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-592">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-592">Example</span></span>

<span data-ttu-id="4f6d3-593">Nell'esempio seguente viene illustrato un elemento **PropertyRef** utilizzato per definire una chiave primaria facendo riferimento a una proprietà definita in un elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="4f6d3-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-595">L'elemento **ReferentialConstraint** in Store Schema Definition Language (SSDL) rappresenta un vincolo di chiave esterna (detto anche vincolo di integrità referenziale) nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="4f6d3-596">Le estremità principale e dipendente del vincolo vengono specificate rispettivamente dagli elementi figlio Principal e Dependent.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="4f6d3-597">Alle colonne che fanno parte delle estremità principale e dipendente viene fatto riferimento tramite gli elementi PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="4f6d3-598">L'elemento **ReferentialConstraint** è un elemento figlio facoltativo dell'elemento Association.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="4f6d3-599">Se non viene utilizzato un elemento **ReferentialConstraint** per eseguire il mapping del vincolo foreign key specificato nell'elemento **Association** , a tale scopo è necessario utilizzare un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="4f6d3-600">L'elemento **ReferentialConstraint** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="4f6d3-601">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="4f6d3-602">Principal (esattamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="4f6d3-603">Dipendente (esattamente uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="4f6d3-604">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-605">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-605">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-606">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="4f6d3-607">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-608">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-609">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-609">Example</span></span>

<span data-ttu-id="4f6d3-610">Nell'esempio seguente viene illustrato un elemento **Association** che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **\_FK CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="4f6d3-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="4f6d3-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-612">L'elemento **returnType** in Store Schema Definition Language (SSDL) specifica il tipo restituito per una funzione definita in un elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="4f6d3-613">Un tipo restituito di funzione può essere specificato anche con un attributo **returnType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="4f6d3-614">Il tipo restituito di una funzione viene specificato con l'attributo **Type** o l'elemento **returnType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="4f6d3-615">L'elemento **returnType** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="4f6d3-616">CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="4f6d3-617">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **returnType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="4f6d3-618">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-619">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-620">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-620">Example</span></span>

<span data-ttu-id="4f6d3-621">Nell'esempio seguente viene utilizzata una **funzione** che restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="4f6d3-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-623">Un elemento **RowType** in Store Schema Definition Language (SSDL) definisce una struttura senza nome come tipo restituito per una funzione definita nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="4f6d3-624">Un elemento **RowType** è l'elemento figlio dell'elemento **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="4f6d3-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="4f6d3-625">Un elemento **RowType** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="4f6d3-626">Property (uno o più elementi)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="4f6d3-627">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-627">Example</span></span>

<span data-ttu-id="4f6d3-628">Nell'esempio seguente viene illustrata una funzione di archiviazione che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe, come specificato nell'elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="4f6d3-629">Elemento Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="4f6d3-630">L'elemento **schema** nel Store Schema Definition Language (SSDL) è l'elemento radice di una definizione del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="4f6d3-631">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="4f6d3-632">L'elemento **schema** può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="4f6d3-633">Association Rules</span><span class="sxs-lookup"><span data-stu-id="4f6d3-633">Association</span></span>
-   <span data-ttu-id="4f6d3-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="4f6d3-634">EntityType</span></span>
-   <span data-ttu-id="4f6d3-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="4f6d3-635">EntityContainer</span></span>
-   <span data-ttu-id="4f6d3-636">Funzione</span><span class="sxs-lookup"><span data-stu-id="4f6d3-636">Function</span></span>

<span data-ttu-id="4f6d3-637">L'elemento **schema** utilizza l'attributo **namespace** per definire lo spazio dei nomi per il tipo di entità e gli oggetti di associazione in un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="4f6d3-638">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="4f6d3-639">Uno spazio dei nomi del modello di archiviazione è diverso dallo spazio dei nomi XML dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="4f6d3-640">Uno spazio dei nomi del modello di archiviazione (definito dall'attributo **namespace** ) è un contenitore logico per tipi di entità e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="4f6d3-641">Lo spazio dei nomi XML, indicato dall'attributo **xmlns** , di un elemento **schema** è lo spazio dei nomi predefinito per gli elementi figlio e gli attributi dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="4f6d3-642">Gli spazi dei nomi XML del form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (dove aaaa e MM rappresentano rispettivamente l'anno e il mese) sono riservati per SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="4f6d3-643">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="4f6d3-644">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="4f6d3-644">Applicable Attributes</span></span>

<span data-ttu-id="4f6d3-645">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="4f6d3-646">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="4f6d3-646">Attribute Name</span></span>            | <span data-ttu-id="4f6d3-647">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-647">Is Required</span></span> | <span data-ttu-id="4f6d3-648">valore</span><span class="sxs-lookup"><span data-stu-id="4f6d3-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-649">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-649">**Namespace**</span></span>             | <span data-ttu-id="4f6d3-650">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-650">Yes</span></span>         | <span data-ttu-id="4f6d3-651">Spazio dei nomi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-651">The namespace of the storage model.</span></span> <span data-ttu-id="4f6d3-652">Il valore dell'attributo **namespace** viene utilizzato per formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="4f6d3-653">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi ExampleModel. Store, il nome completo di **EntityType** sarà ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="4f6d3-654">Non è possibile utilizzare le seguenti stringhe come valore per l'attributo **namespace** : **System**, **Transient**o **EDM**.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="4f6d3-655">Il valore dell'attributo **namespace** non può corrispondere al valore dell'attributo **namespace** nell'elemento schema CSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="4f6d3-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-656">**Alias**</span></span>                 | <span data-ttu-id="4f6d3-657">No</span><span class="sxs-lookup"><span data-stu-id="4f6d3-657">No</span></span>          | <span data-ttu-id="4f6d3-658">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="4f6d3-659">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi ExampleModel. Store e il valore dell'attributo **alias** è *StorageModel*, è possibile utilizzare StorageModel. Customer come nome completo dell'elemento **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="4f6d3-660">**Provider**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-660">**Provider**</span></span>              | <span data-ttu-id="4f6d3-661">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-661">Yes</span></span>         | <span data-ttu-id="4f6d3-662">Provider di dati.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="4f6d3-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="4f6d3-664">Sì</span><span class="sxs-lookup"><span data-stu-id="4f6d3-664">Yes</span></span>         | <span data-ttu-id="4f6d3-665">Token che indica al provider quale manifesto del provider restituire.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="4f6d3-666">Non è definito alcun formato per il token.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-666">No format for the token is defined.</span></span> <span data-ttu-id="4f6d3-667">I valori per il token sono definiti dal provider.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="4f6d3-668">Per informazioni sui token del manifesto del provider SQL Server, vedere SqlClient per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="4f6d3-669">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-669">Example</span></span>

<span data-ttu-id="4f6d3-670">Nell'esempio seguente viene illustrato un elemento **dello schema** che contiene un elemento **EntityContainer** , due elementi **EntityType** e un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="4f6d3-671">Attributi di annotazione</span><span class="sxs-lookup"><span data-stu-id="4f6d3-671">Annotation Attributes</span></span>

<span data-ttu-id="4f6d3-672">Gli attributi di annotazione in Store Schema Definition Language (SSDL) sono attributi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sugli elementi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="4f6d3-673">Oltre ad avere una struttura XML valida, agli attributi di annotazione si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="4f6d3-674">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="4f6d3-675">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="4f6d3-676">È possibile applicare più attributi di annotazione a un determinato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="4f6d3-677">È possibile accedere ai metadati contenuti negli elementi Annotation in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-678">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-678">Example</span></span>

<span data-ttu-id="4f6d3-679">Nell'esempio seguente viene illustrato un elemento EntityType con un attributo annotation applicato alla proprietà **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="4f6d3-680">Nell'esempio viene inoltre mostrato un elemento Annotation aggiunto all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="4f6d3-681">Elementi Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="4f6d3-682">Gli elementi Annotation in SSDL (Store Schema Definition Language) sono elementi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sul modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="4f6d3-683">Oltre ad avere una struttura XML valida, agli elementi Annotation si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="4f6d3-684">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="4f6d3-685">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="4f6d3-686">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="4f6d3-687">Più elementi Annotation possono essere figli di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="4f6d3-688">A partire da .NET Framework versione 4, è possibile accedere ai metadati contenuti negli elementi di annotazione in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="4f6d3-689">Esempio</span><span class="sxs-lookup"><span data-stu-id="4f6d3-689">Example</span></span>

<span data-ttu-id="4f6d3-690">Nell'esempio seguente viene illustrato un elemento EntityType con un elemento Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="4f6d3-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="4f6d3-691">Nell'esempio viene inoltre illustrato un attributo di annotazione applicato alla proprietà **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="4f6d3-692">Facet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="4f6d3-692">Facets (SSDL)</span></span>

<span data-ttu-id="4f6d3-693">I facet in Store Schema Definition Language (SSDL) rappresentano dei vincoli sui tipi di colonna specificati in elementi Property.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="4f6d3-694">I facet vengono implementati come attributi XML sugli elementi della **Proprietà** .</span><span class="sxs-lookup"><span data-stu-id="4f6d3-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="4f6d3-695">Nella tabella seguente vengono descritti i facet supportati in SSDL:</span><span class="sxs-lookup"><span data-stu-id="4f6d3-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="4f6d3-696">Facet</span><span class="sxs-lookup"><span data-stu-id="4f6d3-696">Facet</span></span>           | <span data-ttu-id="4f6d3-697">Descrizione</span><span class="sxs-lookup"><span data-stu-id="4f6d3-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4f6d3-698">**Regole di confronto**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-698">**Collation**</span></span>   | <span data-ttu-id="4f6d3-699">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="4f6d3-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-700">**FixedLength**</span></span> | <span data-ttu-id="4f6d3-701">Specifica se la lunghezza del valore della colonna può variare.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="4f6d3-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-702">**MaxLength**</span></span>   | <span data-ttu-id="4f6d3-703">Specifica la lunghezza massima del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="4f6d3-704">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-704">**Precision**</span></span>   | <span data-ttu-id="4f6d3-705">Per le proprietà di tipo **Decimal**, specifica il numero di cifre che un valore della proprietà può avere.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="4f6d3-706">Per le proprietà di tipo **Time**, **DateTime**e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria dei secondi del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="4f6d3-707">**Ridimensionare**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-707">**Scale**</span></span>       | <span data-ttu-id="4f6d3-708">Specifica il numero di cifre a destra del separatore decimale per il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="4f6d3-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="4f6d3-709">**Unicode**</span></span>     | <span data-ttu-id="4f6d3-710">Indica se il valore della colonna viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="4f6d3-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
