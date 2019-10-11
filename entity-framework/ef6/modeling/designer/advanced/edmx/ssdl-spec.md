---
title: Specifica SSDL-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182540"
---
# <a name="ssdl-specification"></a><span data-ttu-id="c1769-102">Specifica SSDL</span><span class="sxs-lookup"><span data-stu-id="c1769-102">SSDL Specification</span></span>
<span data-ttu-id="c1769-103">Store Schema Definition Language (SSDL) è un linguaggio basato su XML che descrive il modello di archiviazione di un'applicazione Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c1769-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="c1769-104">In un'applicazione Entity Framework, i metadati del modello di archiviazione vengono caricati da un file con estensione SSDL (scritto in SSDL) in un'istanza di System. Data. Metadata. Edm. StoreItemCollection ed è possibile accedervi tramite metodi nel Classe System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="c1769-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="c1769-105">Entity Framework usa i metadati del modello di archiviazione per tradurre le query sul modello concettuale in comandi specifici dell'archivio.</span><span class="sxs-lookup"><span data-stu-id="c1769-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="c1769-106">Il Entity Framework Designer (EF designer) archivia le informazioni sul modello di archiviazione in un file con estensione edmx in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="c1769-107">In fase di compilazione il Entity Designer utilizza le informazioni contenute in un file con estensione edmx per creare il file con estensione SSDL necessario per Entity Framework in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c1769-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="c1769-108">Le versioni di SSDL si differenziano tra loro per gli spazi dei nomi XML.</span><span class="sxs-lookup"><span data-stu-id="c1769-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="c1769-109">Versione SSDL</span><span class="sxs-lookup"><span data-stu-id="c1769-109">SSDL Version</span></span> | <span data-ttu-id="c1769-110">Spazio dei nomi XML</span><span class="sxs-lookup"><span data-stu-id="c1769-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="c1769-111">SSDL V1</span><span class="sxs-lookup"><span data-stu-id="c1769-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="c1769-112">SSDL V2</span><span class="sxs-lookup"><span data-stu-id="c1769-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="c1769-113">SSDL V3</span><span class="sxs-lookup"><span data-stu-id="c1769-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="c1769-114">Elemento Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-114">Association Element (SSDL)</span></span>

<span data-ttu-id="c1769-115">Un elemento **Association** in Store Schema Definition Language (SSDL) specifica le colonne della tabella che fanno parte di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="c1769-116">Due elementi End figlio obbligatori specificano le tabelle alle estremità dell'associazione e la molteplicità a ogni estremità.</span><span class="sxs-lookup"><span data-stu-id="c1769-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="c1769-117">Un elemento ReferentialConstraint facoltativo specifica le estremità principale e dipendente dell'associazione, nonché le colonne partecipanti.</span><span class="sxs-lookup"><span data-stu-id="c1769-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="c1769-118">Se non è presente alcun elemento **ReferentialConstraint** , è necessario utilizzare un elemento AssociationSetMapping per specificare i mapping delle colonne per l'associazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="c1769-119">L'elemento **Association** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-120">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-121">End (esattamente due)</span><span class="sxs-lookup"><span data-stu-id="c1769-121">End (exactly two)</span></span>
-   <span data-ttu-id="c1769-122">ReferentialConstraint (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="c1769-123">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-124">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-124">Applicable Attributes</span></span>

<span data-ttu-id="c1769-125">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="c1769-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="c1769-126">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-126">Attribute Name</span></span> | <span data-ttu-id="c1769-127">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-127">Is Required</span></span> | <span data-ttu-id="c1769-128">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-129">**Name**</span></span>       | <span data-ttu-id="c1769-130">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-130">Yes</span></span>         | <span data-ttu-id="c1769-131">Il nome del vincolo di chiave esterna corrispondente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-132">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="c1769-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="c1769-133">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-134">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-135">Example</span></span>

<span data-ttu-id="c1769-136">Nell'esempio seguente viene illustrato un elemento **Association** che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **FK @ no__t-3CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="c1769-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="c1769-137">Elemento AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="c1769-138">L'elemento **associativo** in Store Schema Definition Language (SSDL) rappresenta un vincolo di chiave esterna tra due tabelle nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="c1769-139">Le colonne della tabella che fanno parte del vincolo FOREIGN KEY vengono specificate in un elemento Association.</span><span class="sxs-lookup"><span data-stu-id="c1769-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="c1769-140">L'elemento **Association** che corrisponde a **un elemento di associazione specificato è** specificato nell'attributo **Association** dell'elemento associationname.</span><span class="sxs-lookup"><span data-stu-id="c1769-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="c1769-141">I set di associazioni SSDL vengono mappati ai set di associazioni CSDL da un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="c1769-142">Tuttavia, se l'associazione CSDL per un determinato set di associazioni CSDL viene definita utilizzando un elemento ReferentialConstraint, non è necessario alcun elemento **AssociationSetMapping** corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="c1769-143">In questo caso, se è presente un elemento **AssociationSetMapping** , i mapping che definisce verranno sottoposti a override dall'elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="c1769-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="c1769-144">L'elemento **associationname** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-145">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-146">End (zero o due)</span><span class="sxs-lookup"><span data-stu-id="c1769-146">End (zero or two)</span></span>
-   <span data-ttu-id="c1769-147">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-148">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-148">Applicable Attributes</span></span>

<span data-ttu-id="c1769-149">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="c1769-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="c1769-150">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-150">Attribute Name</span></span>  | <span data-ttu-id="c1769-151">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-151">Is Required</span></span> | <span data-ttu-id="c1769-152">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-153">**Name**</span></span>        | <span data-ttu-id="c1769-154">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-154">Yes</span></span>         | <span data-ttu-id="c1769-155">Nome del vincolo di chiave esterna rappresentato dal set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="c1769-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="c1769-156">**Associazione**</span><span class="sxs-lookup"><span data-stu-id="c1769-156">**Association**</span></span> | <span data-ttu-id="c1769-157">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-157">Yes</span></span>         | <span data-ttu-id="c1769-158">Nome dell'associazione che definisce le colonne che fanno parte del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="c1769-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-159">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="c1769-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="c1769-160">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-161">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-162">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-162">Example</span></span>

<span data-ttu-id="c1769-163">Nell'esempio seguente viene illustrato un elemento di **associazione** che rappresenta il vincolo di chiave esterna `FK_CustomerOrders` nel database sottostante:</span><span class="sxs-lookup"><span data-stu-id="c1769-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="c1769-164">Elemento CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="c1769-165">L'elemento **CollectionType** in Store Schema Definition Language (SSDL) specifica che il tipo restituito di una funzione è una raccolta.</span><span class="sxs-lookup"><span data-stu-id="c1769-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="c1769-166">L'elemento **CollectionType** è figlio dell'elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="c1769-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="c1769-167">Il tipo di raccolta viene specificato tramite l'elemento figlio RowType:</span><span class="sxs-lookup"><span data-stu-id="c1769-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="c1769-168">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="c1769-169">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-170">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-171">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-171">Example</span></span>

<span data-ttu-id="c1769-172">Nell'esempio seguente viene illustrata una funzione che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="c1769-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="c1769-173">Elemento CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="c1769-174">L'elemento **CommandText** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento Function che consente di definire un'istruzione SQL che viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="c1769-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="c1769-175">L'elemento **CommandText** consente di aggiungere una funzionalità simile a una stored procedure nel database, ma l'elemento **CommandText** viene definito nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="c1769-176">L'elemento **CommandText** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c1769-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="c1769-177">Il corpo dell'elemento **CommandText** deve essere un'istruzione SQL valida per il database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="c1769-178">Nessun attributo applicabile all'elemento **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="c1769-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-179">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-179">Example</span></span>

<span data-ttu-id="c1769-180">Nell'esempio seguente viene illustrato un elemento **Function** con un elemento **CommandText** figlio.</span><span class="sxs-lookup"><span data-stu-id="c1769-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="c1769-181">Esporre la funzione **UpdateProductInOrder** come metodo in ObjectContext mediante l'importazione nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="c1769-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="c1769-182">Elemento DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="c1769-183">L'elemento **DefiningQuery** in Store Schema Definition Language (SSDL) consente di eseguire un'istruzione SQL direttamente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="c1769-184">L'elemento **DefiningQuery** viene comunemente utilizzato come una vista di database, ma la vista è definita nel modello di archiviazione anziché nel database.</span><span class="sxs-lookup"><span data-stu-id="c1769-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="c1769-185">La vista definita in un elemento **DefiningQuery** può essere mappata a un tipo di entità nel modello concettuale tramite un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="c1769-186">Questi mapping sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="c1769-186">These mappings are read-only.</span></span>  

<span data-ttu-id="c1769-187">Nella sintassi SSDL seguente viene illustrata la dichiarazione di un oggetto **EntitySet** seguito dall'elemento **DefiningQuery** che contiene una query utilizzata per recuperare la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="c1769-188">È possibile utilizzare le stored procedure nel Entity Framework per abilitare gli scenari di lettura e scrittura sulle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c1769-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="c1769-189"> È possibile utilizzare una vista origine dati o una vista Entity SQL come tabella di base per il recupero dei dati e per l'elaborazione delle modifiche da parte delle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c1769-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="c1769-190">È possibile usare l'elemento **DefiningQuery** per la destinazione Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="c1769-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="c1769-191">Sebbene SQL Server Compact 3,5 non supporti stored procedure, è possibile implementare una funzionalità simile con l'elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="c1769-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="c1769-192">Un'altra situazione in cui può essere utile è nella creazione di stored procedure per risolvere una mancata corrispondenza tra i tipi di dati utilizzati nel linguaggio di programmazione e quelli dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="c1769-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="c1769-193">È possibile scrivere un **DefiningQuery** che accetta un determinato set di parametri e quindi chiama un stored procedure con un set di parametri diverso, ad esempio un stored procedure che elimina i dati.</span><span class="sxs-lookup"><span data-stu-id="c1769-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="c1769-194">Elemento Dependent (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="c1769-195">L'elemento **dipendente** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento ReferentialConstraint che definisce l'entità finale dipendente di un vincolo di chiave esterna (detto anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="c1769-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="c1769-196">L'elemento **dipendente** specifica la colonna (o le colonne) in una tabella che fa riferimento a una colonna di chiave primaria (o a colonne).</span><span class="sxs-lookup"><span data-stu-id="c1769-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="c1769-197">Gli elementi **PropertyRef** specificano le colonne a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="c1769-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="c1769-198">L'elemento Principal specifica le colonne chiave primaria a cui fanno riferimento le colonne specificate nell'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="c1769-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="c1769-199">L'elemento **dipendente** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-200">PropertyRef (uno o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c1769-201">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-202">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-202">Applicable Attributes</span></span>

<span data-ttu-id="c1769-203">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="c1769-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="c1769-204">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-204">Attribute Name</span></span> | <span data-ttu-id="c1769-205">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-205">Is Required</span></span> | <span data-ttu-id="c1769-206">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-207">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="c1769-207">**Role**</span></span>       | <span data-ttu-id="c1769-208">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-208">Yes</span></span>         | <span data-ttu-id="c1769-209">Lo stesso valore dell'attributo **Role** (se utilizzato) dell'elemento End corrispondente; in caso contrario, il nome della tabella che contiene la colonna di riferimento.</span><span class="sxs-lookup"><span data-stu-id="c1769-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-210">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **dipendente** .</span><span class="sxs-lookup"><span data-stu-id="c1769-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="c1769-211">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-212">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-213">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-213">Example</span></span>

<span data-ttu-id="c1769-214">Nell'esempio seguente viene illustrato un elemento Association che utilizza un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="c1769-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c1769-215">L'elemento **dipendente** specifica la colonna **CustomerID** della tabella **Order** come entità finale dipendente del vincolo.</span><span class="sxs-lookup"><span data-stu-id="c1769-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="c1769-216">Elemento Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="c1769-217">L'elemento **Documentation** in Store Schema Definition Language (SSDL) può essere utilizzato per fornire informazioni su un oggetto definito in un elemento padre.</span><span class="sxs-lookup"><span data-stu-id="c1769-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="c1769-218">L'elemento **Documentation** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-219">**Riepilogo**: Breve descrizione dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="c1769-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="c1769-220">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="c1769-220">(zero or one element)</span></span>
-   <span data-ttu-id="c1769-221">**LongDescription**: Descrizione completa dell'elemento padre.</span><span class="sxs-lookup"><span data-stu-id="c1769-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="c1769-222">Zero o un elemento.</span><span class="sxs-lookup"><span data-stu-id="c1769-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-223">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-223">Applicable Attributes</span></span>

<span data-ttu-id="c1769-224">All'elemento **Documentation** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="c1769-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="c1769-225">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-226">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-227">Example</span></span>

<span data-ttu-id="c1769-228">Nell'esempio seguente viene illustrato l'elemento **Documentation** come elemento figlio di un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="c1769-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="c1769-229">Elemento End (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-229">End Element (SSDL)</span></span>

<span data-ttu-id="c1769-230">L'elemento **end** in Store Schema Definition Language (SSDL) specifica la tabella e il numero di righe a un'estremità di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="c1769-231">L'elemento **end** può essere un elemento figlio dell'elemento Association o dell'elemento associationname.</span><span class="sxs-lookup"><span data-stu-id="c1769-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="c1769-232">In entrambi i casi, gli elementi figlio possibili e gli attributi applicabili sono diversi.</span><span class="sxs-lookup"><span data-stu-id="c1769-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="c1769-233">Elemento End come figlio dell'elemento Association</span><span class="sxs-lookup"><span data-stu-id="c1769-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="c1769-234">Un elemento **end** (come figlio dell'elemento **Association** ) specifica la tabella e il numero di righe alla fine di un vincolo FOREIGN KEY con rispettivamente il **tipo** e gli attributi di **molteplicità** .</span><span class="sxs-lookup"><span data-stu-id="c1769-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="c1769-235">Le entità finali del vincolo di chiave esterna sono definite come parte dell'associazione SSDL. L'associazione SSDL deve disporre esattamente di due entità finali.</span><span class="sxs-lookup"><span data-stu-id="c1769-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="c1769-236">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-237">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c1769-238">OnDelete (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="c1769-239">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="c1769-240">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-240">Applicable Attributes</span></span>

<span data-ttu-id="c1769-241">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **finale** quando è figlio di un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="c1769-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="c1769-242">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-242">Attribute Name</span></span>   | <span data-ttu-id="c1769-243">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-243">Is Required</span></span> | <span data-ttu-id="c1769-244">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="c1769-245">**Type**</span></span>         | <span data-ttu-id="c1769-246">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-246">Yes</span></span>         | <span data-ttu-id="c1769-247">Nome completo del set di entità SSDL che si trova alla fine del vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="c1769-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="c1769-248">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="c1769-248">**Role**</span></span>         | <span data-ttu-id="c1769-249">No</span><span class="sxs-lookup"><span data-stu-id="c1769-249">No</span></span>          | <span data-ttu-id="c1769-250">Valore dell'attributo **Role** nell'elemento Principal o dipendente dell'elemento ReferentialConstraint corrispondente (se usato).</span><span class="sxs-lookup"><span data-stu-id="c1769-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="c1769-251">**Molteplicità**</span><span class="sxs-lookup"><span data-stu-id="c1769-251">**Multiplicity**</span></span> | <span data-ttu-id="c1769-252">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-252">Yes</span></span>         | <span data-ttu-id="c1769-253">**1**, **0.. 1**o **\*** a seconda del numero di righe che possono trovarsi alla fine del vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="c1769-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="c1769-254">**1** indica che esiste esattamente una riga nell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="c1769-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="c1769-255">**0.. 1** indica che l'entità finale del vincolo di chiave esterna è pari a zero o a una riga.</span><span class="sxs-lookup"><span data-stu-id="c1769-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="c1769-256">**\*** indica che sono presenti zero, una o più righe nell'entità finale del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="c1769-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-257">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="c1769-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="c1769-258">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-259">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="c1769-260">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-260">Example</span></span>

<span data-ttu-id="c1769-261">Nell'esempio seguente viene illustrato un elemento **Association** che definisce il vincolo FOREIGN KEY di **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="c1769-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c1769-262">I valori di **molteplicità** specificati in ogni elemento **end** indicano che molte righe della tabella **Orders** possono essere associate a una riga nella tabella **Customers** , ma è possibile associare una sola riga della tabella **Customers** a una riga nella tabella **Orders** .</span><span class="sxs-lookup"><span data-stu-id="c1769-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="c1769-263">Inoltre, l'elemento **OnDelete** indica che tutte le righe della tabella **Orders** che fanno riferimento a una determinata riga nella tabella **Customers** verranno eliminate se la riga della tabella **Customers** viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="c1769-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="c1769-264">Elemento End come figlio dell'elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="c1769-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="c1769-265">L'elemento **end** (come figlio dell'elemento **associationname** ) specifica una tabella in corrispondenza di un'entità finale di un vincolo FOREIGN KEY nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="c1769-266">Un elemento **end** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-267">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-268">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="c1769-269">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-269">Applicable Attributes</span></span>

<span data-ttu-id="c1769-270">Nella tabella seguente vengono descritti gli attributi che possono essere applicati all'elemento **finale** quando è figlio di un elemento di **associazione** .</span><span class="sxs-lookup"><span data-stu-id="c1769-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="c1769-271">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-271">Attribute Name</span></span> | <span data-ttu-id="c1769-272">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-272">Is Required</span></span> | <span data-ttu-id="c1769-273">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="c1769-274">**EntitySet**</span></span>  | <span data-ttu-id="c1769-275">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-275">Yes</span></span>         | <span data-ttu-id="c1769-276">Nome del set di entità SSDL che si trova alla fine del vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="c1769-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="c1769-277">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="c1769-277">**Role**</span></span>       | <span data-ttu-id="c1769-278">No</span><span class="sxs-lookup"><span data-stu-id="c1769-278">No</span></span>          | <span data-ttu-id="c1769-279">Valore di uno degli attributi **Role** specificati in un elemento **end** dell'elemento Association corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-280">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **finale** .</span><span class="sxs-lookup"><span data-stu-id="c1769-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="c1769-281">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-282">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="c1769-283">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-283">Example</span></span>

<span data-ttu-id="c1769-284">Nell'esempio seguente viene illustrato un elemento **EntityContainer** con un elemento di **associazione** con due elementi **end** :</span><span class="sxs-lookup"><span data-stu-id="c1769-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="c1769-285">Elemento EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="c1769-286">Un elemento **EntityContainer** in Store Schema Definition Language (SSDL) descrive la struttura dell'origine dati sottostante in un'applicazione Entity Framework: I set di entità SSDL (definiti negli elementi EntitySet) rappresentano le tabelle in un database, i tipi di entità SSDL (definiti negli elementi EntityType) rappresentano le righe in una tabella e i set di associazioni (definiti in elementi di associazioni) rappresentano vincoli di chiave esterna in una database.</span><span class="sxs-lookup"><span data-stu-id="c1769-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="c1769-287">Un contenitore di entità del modello di archiviazione esegue il mapping a un contenitore di entità del modello concettuale tramite l'elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="c1769-288">Un elemento **EntityContainer** può avere uno o più elementi di documentazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="c1769-289">Se è presente un elemento di **documentazione** , deve precedere tutti gli altri elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c1769-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="c1769-290">Un elemento **EntityContainer** può avere zero o più degli elementi figlio seguenti (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="c1769-291">EntitySet</span></span>
-   <span data-ttu-id="c1769-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="c1769-292">AssociationSet</span></span>
-   <span data-ttu-id="c1769-293">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="c1769-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-294">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-294">Applicable Attributes</span></span>

<span data-ttu-id="c1769-295">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="c1769-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="c1769-296">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-296">Attribute Name</span></span> | <span data-ttu-id="c1769-297">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-297">Is Required</span></span> | <span data-ttu-id="c1769-298">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="c1769-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-299">**Name**</span></span>       | <span data-ttu-id="c1769-300">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-300">Yes</span></span>         | <span data-ttu-id="c1769-301">Nome del contenitore di entità.</span><span class="sxs-lookup"><span data-stu-id="c1769-301">The name of the entity container.</span></span> <span data-ttu-id="c1769-302">Il nome non può contenere caratteri punto (.).</span><span class="sxs-lookup"><span data-stu-id="c1769-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-303">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="c1769-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="c1769-304">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-305">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-306">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-306">Example</span></span>

<span data-ttu-id="c1769-307">Nell'esempio seguente viene illustrato un elemento **EntityContainer** che definisce due set di entità e un set di associazioni.</span><span class="sxs-lookup"><span data-stu-id="c1769-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="c1769-308">I nomi dei tipi di entità e associazione sono qualificati dal nome dello spazio dei nomi del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="c1769-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="c1769-309">Elemento EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="c1769-310">Un elemento **EntitySet** in Store Schema Definition Language (SSDL) rappresenta una tabella o una vista nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="c1769-311">Un elemento EntityType in SSDL rappresenta una riga nella tabella o nella vista.</span><span class="sxs-lookup"><span data-stu-id="c1769-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="c1769-312">L'attributo **EntityType** di un elemento **EntitySet** specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="c1769-313">Il mapping tra un set di entità CSDL e un set di entità SSDL viene specificato in un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="c1769-314">L'elemento **EntitySet** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-315">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c1769-316">DefiningQuery (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="c1769-317">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="c1769-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-318">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-318">Applicable Attributes</span></span>

<span data-ttu-id="c1769-319">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="c1769-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="c1769-320">Alcuni attributi (non elencati qui) possono essere qualificati con l'alias del **negozio** .</span><span class="sxs-lookup"><span data-stu-id="c1769-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="c1769-321">Questi attributi vengono utilizzati dalla procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="c1769-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="c1769-322">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-322">Attribute Name</span></span> | <span data-ttu-id="c1769-323">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-323">Is Required</span></span> | <span data-ttu-id="c1769-324">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-325">**Name**</span></span>       | <span data-ttu-id="c1769-326">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-326">Yes</span></span>         | <span data-ttu-id="c1769-327">Nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="c1769-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="c1769-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="c1769-328">**EntityType**</span></span> | <span data-ttu-id="c1769-329">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-329">Yes</span></span>         | <span data-ttu-id="c1769-330">Nome completo del tipo di entità per il quale il set di entità contiene delle istanze.</span><span class="sxs-lookup"><span data-stu-id="c1769-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="c1769-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="c1769-331">**Schema**</span></span>     | <span data-ttu-id="c1769-332">No</span><span class="sxs-lookup"><span data-stu-id="c1769-332">No</span></span>          | <span data-ttu-id="c1769-333">Schema del database.</span><span class="sxs-lookup"><span data-stu-id="c1769-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="c1769-334">**Tabella**</span><span class="sxs-lookup"><span data-stu-id="c1769-334">**Table**</span></span>      | <span data-ttu-id="c1769-335">No</span><span class="sxs-lookup"><span data-stu-id="c1769-335">No</span></span>          | <span data-ttu-id="c1769-336">Tabella del database.</span><span class="sxs-lookup"><span data-stu-id="c1769-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="c1769-337">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="c1769-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="c1769-338">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-339">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-340">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-340">Example</span></span>

<span data-ttu-id="c1769-341">Nell'esempio seguente viene illustrato un elemento **EntityContainer** che dispone di due elementi **EntitySet** e di un elemento di **associazione** :</span><span class="sxs-lookup"><span data-stu-id="c1769-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="c1769-342">Elemento EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="c1769-343">Un elemento **EntityType** in Store Schema Definition Language (SSDL) rappresenta una riga in una tabella o vista del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="c1769-344">Un elemento EntitySet in SSDL rappresenta la tabella o la vista in cui si verificano le righe.</span><span class="sxs-lookup"><span data-stu-id="c1769-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="c1769-345">L'attributo **EntityType** di un elemento **EntitySet** specifica il particolare tipo di entità SSDL che rappresenta le righe in un set di entità SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="c1769-346">Il mapping tra un tipo di entità SSDL e un tipo di entità CSDL viene specificato in un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="c1769-347">L'elemento **EntityType** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-348">Documentazione (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="c1769-349">Key (zero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="c1769-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="c1769-350">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="c1769-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-351">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-351">Applicable Attributes</span></span>

<span data-ttu-id="c1769-352">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="c1769-353">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-353">Attribute Name</span></span> | <span data-ttu-id="c1769-354">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-354">Is Required</span></span> | <span data-ttu-id="c1769-355">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-356">**Name**</span></span>       | <span data-ttu-id="c1769-357">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-357">Yes</span></span>         | <span data-ttu-id="c1769-358">Nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="c1769-358">The name of the entity type.</span></span> <span data-ttu-id="c1769-359">Questo valore corrisponde generalmente al nome della tabella in cui il tipo di entità rappresenta una riga.</span><span class="sxs-lookup"><span data-stu-id="c1769-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="c1769-360">Questo valore non può contenere punti (.).</span><span class="sxs-lookup"><span data-stu-id="c1769-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-361">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="c1769-362">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-363">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-364">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-364">Example</span></span>

<span data-ttu-id="c1769-365">Nell'esempio seguente viene illustrato un elemento **EntityType** con due proprietà:</span><span class="sxs-lookup"><span data-stu-id="c1769-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="c1769-366">Elemento Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-366">Function Element (SSDL)</span></span>

<span data-ttu-id="c1769-367">L'elemento **Function** in Store Schema Definition Language (SSDL) specifica una stored procedure esistente nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="c1769-368">L'elemento **Function** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-369">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-370">Parameter (zero o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="c1769-371">CommandText (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="c1769-372">ReturnType (zero o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="c1769-373">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="c1769-374">Un tipo restituito per una funzione deve essere specificato con l'elemento **returnType** o l'attributo **returnType** (vedere di seguito), ma non entrambi.</span><span class="sxs-lookup"><span data-stu-id="c1769-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="c1769-375">È possibile importare stored procedure specificate nel modello di archiviazione nel modello concettuale di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="c1769-376">Per ulteriori informazioni, vedere [esecuzione di query con stored procedure](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="c1769-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="c1769-377">L'elemento **Function** può essere utilizzato anche per definire funzioni personalizzate nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-378">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-378">Applicable Attributes</span></span>

<span data-ttu-id="c1769-379">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="c1769-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="c1769-380">Alcuni attributi (non elencati qui) possono essere qualificati con l'alias del **negozio** .</span><span class="sxs-lookup"><span data-stu-id="c1769-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="c1769-381">Questi attributi vengono utilizzati dalla procedura guidata Aggiorna modello quando si aggiorna un modello.</span><span class="sxs-lookup"><span data-stu-id="c1769-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="c1769-382">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-382">Attribute Name</span></span>             | <span data-ttu-id="c1769-383">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-383">Is Required</span></span> | <span data-ttu-id="c1769-384">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-385">**Name**</span></span>                   | <span data-ttu-id="c1769-386">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-386">Yes</span></span>         | <span data-ttu-id="c1769-387">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c1769-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c1769-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="c1769-388">**ReturnType**</span></span>             | <span data-ttu-id="c1769-389">No</span><span class="sxs-lookup"><span data-stu-id="c1769-389">No</span></span>          | <span data-ttu-id="c1769-390">Tipo restituito della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c1769-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="c1769-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="c1769-391">**Aggregate**</span></span>              | <span data-ttu-id="c1769-392">No</span><span class="sxs-lookup"><span data-stu-id="c1769-392">No</span></span>          | <span data-ttu-id="c1769-393">**True** se il stored procedure restituisce un valore di aggregazione. in caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="c1769-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="c1769-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="c1769-394">**BuiltIn**</span></span>                | <span data-ttu-id="c1769-395">No</span><span class="sxs-lookup"><span data-stu-id="c1769-395">No</span></span>          | <span data-ttu-id="c1769-396">**True** se la funzione è una funzione incorporata<sup>1</sup> . in caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="c1769-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="c1769-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="c1769-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="c1769-398">No</span><span class="sxs-lookup"><span data-stu-id="c1769-398">No</span></span>          | <span data-ttu-id="c1769-399">Nome della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c1769-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c1769-400">**Attributo NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="c1769-400">**NiladicFunction**</span></span>        | <span data-ttu-id="c1769-401">No</span><span class="sxs-lookup"><span data-stu-id="c1769-401">No</span></span>          | <span data-ttu-id="c1769-402">**True** se la funzione è una funzione senza parametri<sup>2</sup> . In caso contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="c1769-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="c1769-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="c1769-403">**IsComposable**</span></span>           | <span data-ttu-id="c1769-404">No</span><span class="sxs-lookup"><span data-stu-id="c1769-404">No</span></span>          | <span data-ttu-id="c1769-405">**True** se la funzione è una funzione componibile<sup>3</sup> . In caso contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="c1769-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="c1769-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="c1769-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="c1769-407">No</span><span class="sxs-lookup"><span data-stu-id="c1769-407">No</span></span>          | <span data-ttu-id="c1769-408">Enumerazione che definisce la semantica dei tipi utilizzata per risolvere gli overload della funzione.</span><span class="sxs-lookup"><span data-stu-id="c1769-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="c1769-409">L'enumerazione è definita nel file manifesto del provider per ogni definizione di funzione.</span><span class="sxs-lookup"><span data-stu-id="c1769-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="c1769-410">Il valore predefinito è **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="c1769-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="c1769-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="c1769-411">**Schema**</span></span>                 | <span data-ttu-id="c1769-412">No</span><span class="sxs-lookup"><span data-stu-id="c1769-412">No</span></span>          | <span data-ttu-id="c1769-413">Nome dello schema in cui viene definita la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c1769-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="c1769-414"><sup>1</sup> una funzione predefinita è una funzione definita nel database.</span><span class="sxs-lookup"><span data-stu-id="c1769-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="c1769-415">Per informazioni sulle funzioni definite nel modello di archiviazione, vedere elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="c1769-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="c1769-416"><sup>2</sup> una funzione senza parametri è una funzione che non accetta parametri e, quando viene chiamato, non richiede le parentesi.</span><span class="sxs-lookup"><span data-stu-id="c1769-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="c1769-417"><sup>3</sup> due funzioni sono componibili se l'output di una funzione può essere l'input per l'altra funzione.</span><span class="sxs-lookup"><span data-stu-id="c1769-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="c1769-418">All'elemento **Function** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="c1769-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="c1769-419">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-420">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-421">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-421">Example</span></span>

<span data-ttu-id="c1769-422">Nell'esempio seguente viene illustrato un elemento **Function** che corrisponde alla stored procedure **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="c1769-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="c1769-423">La stored procedure accetta due parametri e non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="c1769-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="c1769-424">Elemento Key (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-424">Key Element (SSDL)</span></span>

<span data-ttu-id="c1769-425">L'elemento **Key** in Store Schema Definition Language (SSDL) rappresenta la chiave primaria di una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="c1769-426">**Key** è un elemento figlio di un elemento EntityType, che rappresenta una riga in una tabella.</span><span class="sxs-lookup"><span data-stu-id="c1769-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="c1769-427">La chiave primaria è definita nell'elemento **chiave** facendo riferimento a uno o più elementi Property definiti nell'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="c1769-428">L'elemento **Key** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-429">PropertyRef (uno o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c1769-430">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="c1769-430">Annotation elements</span></span>

<span data-ttu-id="c1769-431">Nessun attributo applicabile all'elemento **chiave** .</span><span class="sxs-lookup"><span data-stu-id="c1769-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-432">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-432">Example</span></span>

<span data-ttu-id="c1769-433">Nell'esempio seguente viene illustrato un elemento **EntityType** con una chiave che fa riferimento a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="c1769-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="c1769-434">Elemento OnDelete (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="c1769-435">L'elemento **OnDelete** in Store Schema Definition Language (SSDL) riflette il comportamento del database quando viene eliminata una riga che fa parte di un vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="c1769-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="c1769-436">Se l'azione è impostata su **Cascade**, verranno eliminate anche le righe che fanno riferimento a una riga in fase di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="c1769-437">Se l'azione è impostata su **None**, non vengono eliminate anche le righe che fanno riferimento a una riga eliminata.</span><span class="sxs-lookup"><span data-stu-id="c1769-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="c1769-438">Un elemento **OnDelete** è un elemento figlio di un elemento End.</span><span class="sxs-lookup"><span data-stu-id="c1769-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="c1769-439">Un elemento **OnDelete** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-440">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-441">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-442">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-442">Applicable Attributes</span></span>

<span data-ttu-id="c1769-443">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="c1769-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="c1769-444">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-444">Attribute Name</span></span> | <span data-ttu-id="c1769-445">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-445">Is Required</span></span> | <span data-ttu-id="c1769-446">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-447">**Azione**</span><span class="sxs-lookup"><span data-stu-id="c1769-447">**Action**</span></span>     | <span data-ttu-id="c1769-448">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-448">Yes</span></span>         | <span data-ttu-id="c1769-449">**Cascade** o **None**.</span><span class="sxs-lookup"><span data-stu-id="c1769-449">**Cascade** or **None**.</span></span> <span data-ttu-id="c1769-450">(Il valore con **restrizioni** è valido ma ha lo stesso comportamento di **None**).</span><span class="sxs-lookup"><span data-stu-id="c1769-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-451">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="c1769-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="c1769-452">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-453">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-454">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-454">Example</span></span>

<span data-ttu-id="c1769-455">Nell'esempio seguente viene illustrato un elemento **Association** che definisce il vincolo FOREIGN KEY di **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="c1769-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c1769-456">L'elemento **OnDelete** indica che tutte le righe della tabella **Orders** che fanno riferimento a una determinata riga della tabella **Customers** verranno eliminate se la riga della tabella **Customers** viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="c1769-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="c1769-457">Elemento Parameter (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="c1769-458">L'elemento **Parameter** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento Function che specifica i parametri per un stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="c1769-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="c1769-459">L'elemento **Parameter** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-460">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-461">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-462">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-462">Applicable Attributes</span></span>

<span data-ttu-id="c1769-463">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="c1769-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="c1769-464">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-464">Attribute Name</span></span> | <span data-ttu-id="c1769-465">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-465">Is Required</span></span> | <span data-ttu-id="c1769-466">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-467">**Name**</span></span>       | <span data-ttu-id="c1769-468">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-468">Yes</span></span>         | <span data-ttu-id="c1769-469">Nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="c1769-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="c1769-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="c1769-470">**Type**</span></span>       | <span data-ttu-id="c1769-471">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-471">Yes</span></span>         | <span data-ttu-id="c1769-472">Tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="c1769-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="c1769-473">**Modalità**</span><span class="sxs-lookup"><span data-stu-id="c1769-473">**Mode**</span></span>       | <span data-ttu-id="c1769-474">No</span><span class="sxs-lookup"><span data-stu-id="c1769-474">No</span></span>          | <span data-ttu-id="c1769-475">**In**, **out**o **InOut** a seconda che il parametro sia un parametro di input, di output o di input/output.</span><span class="sxs-lookup"><span data-stu-id="c1769-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="c1769-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c1769-476">**MaxLength**</span></span>  | <span data-ttu-id="c1769-477">No</span><span class="sxs-lookup"><span data-stu-id="c1769-477">No</span></span>          | <span data-ttu-id="c1769-478">Lunghezza massima del parametro.</span><span class="sxs-lookup"><span data-stu-id="c1769-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="c1769-479">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="c1769-479">**Precision**</span></span>  | <span data-ttu-id="c1769-480">No</span><span class="sxs-lookup"><span data-stu-id="c1769-480">No</span></span>          | <span data-ttu-id="c1769-481">Precisione del parametro.</span><span class="sxs-lookup"><span data-stu-id="c1769-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="c1769-482">**Scala**</span><span class="sxs-lookup"><span data-stu-id="c1769-482">**Scale**</span></span>      | <span data-ttu-id="c1769-483">No</span><span class="sxs-lookup"><span data-stu-id="c1769-483">No</span></span>          | <span data-ttu-id="c1769-484">Scala del parametro.</span><span class="sxs-lookup"><span data-stu-id="c1769-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="c1769-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="c1769-485">**SRID**</span></span>       | <span data-ttu-id="c1769-486">No</span><span class="sxs-lookup"><span data-stu-id="c1769-486">No</span></span>          | <span data-ttu-id="c1769-487">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="c1769-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="c1769-488">Valido solo per i parametri dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="c1769-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="c1769-489">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1769-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-490">All'elemento **Parameter** è possibile applicare qualsiasi numero di attributi di annotazione (attributi XML personalizzati).</span><span class="sxs-lookup"><span data-stu-id="c1769-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="c1769-491">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-492">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-493">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-493">Example</span></span>

<span data-ttu-id="c1769-494">Nell'esempio seguente viene illustrato un elemento **Function** con due elementi **Parameter** che specificano i parametri di input:</span><span class="sxs-lookup"><span data-stu-id="c1769-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="c1769-495">Elemento Principal (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="c1769-496">L'elemento **Principal** in Store Schema Definition Language (SSDL) è un elemento figlio dell'elemento ReferentialConstraint che definisce l'entità finale principale di un vincolo FOREIGN KEY (detto anche vincolo referenziale).</span><span class="sxs-lookup"><span data-stu-id="c1769-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="c1769-497">L'elemento **Principal** specifica la colonna o le colonne di chiave primaria in una tabella a cui fa riferimento un'altra colonna o colonne.</span><span class="sxs-lookup"><span data-stu-id="c1769-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="c1769-498">Gli elementi **PropertyRef** specificano le colonne a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="c1769-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="c1769-499">L'elemento dipendente specifica le colonne che fanno riferimento alle colonne di chiave primaria specificate nell'elemento **Principal** .</span><span class="sxs-lookup"><span data-stu-id="c1769-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="c1769-500">L'elemento **Principal** può includere i seguenti elementi figlio (nell'ordine elencato):</span><span class="sxs-lookup"><span data-stu-id="c1769-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="c1769-501">PropertyRef (uno o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="c1769-502">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-503">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-503">Applicable Attributes</span></span>

<span data-ttu-id="c1769-504">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Principal** .</span><span class="sxs-lookup"><span data-stu-id="c1769-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="c1769-505">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-505">Attribute Name</span></span> | <span data-ttu-id="c1769-506">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-506">Is Required</span></span> | <span data-ttu-id="c1769-507">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-508">**Ruolo**</span><span class="sxs-lookup"><span data-stu-id="c1769-508">**Role**</span></span>       | <span data-ttu-id="c1769-509">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-509">Yes</span></span>         | <span data-ttu-id="c1769-510">Lo stesso valore dell'attributo **Role** (se utilizzato) dell'elemento End corrispondente; in caso contrario, il nome della tabella che contiene la colonna a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="c1769-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-511">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **principale** .</span><span class="sxs-lookup"><span data-stu-id="c1769-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="c1769-512">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-513">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-514">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-514">Example</span></span>

<span data-ttu-id="c1769-515">Nell'esempio seguente viene illustrato un elemento Association che utilizza un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="c1769-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="c1769-516">L'elemento **Principal** specifica la colonna **CustomerID** della tabella **Customer** come entità finale principale del vincolo.</span><span class="sxs-lookup"><span data-stu-id="c1769-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="c1769-517">Elemento Property (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-517">Property Element (SSDL)</span></span>

<span data-ttu-id="c1769-518">L'elemento **Property** in Store Schema Definition Language (SSDL) rappresenta una colonna in una tabella nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="c1769-519">Gli elementi **Proprietà** sono elementi figlio degli elementi EntityType, che rappresentano le righe in una tabella.</span><span class="sxs-lookup"><span data-stu-id="c1769-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="c1769-520">Ogni elemento **Property** definito in un elemento **EntityType** rappresenta una colonna.</span><span class="sxs-lookup"><span data-stu-id="c1769-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="c1769-521">Un elemento **Property** non può contenere elementi figlio.</span><span class="sxs-lookup"><span data-stu-id="c1769-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-522">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-522">Applicable Attributes</span></span>

<span data-ttu-id="c1769-523">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="c1769-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="c1769-524">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-524">Attribute Name</span></span>            | <span data-ttu-id="c1769-525">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-525">Is Required</span></span> | <span data-ttu-id="c1769-526">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-527">**Name**</span></span>                  | <span data-ttu-id="c1769-528">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-528">Yes</span></span>         | <span data-ttu-id="c1769-529">Nome della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="c1769-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="c1769-530">**Type**</span></span>                  | <span data-ttu-id="c1769-531">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-531">Yes</span></span>         | <span data-ttu-id="c1769-532">Tipo della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="c1769-533">**Nullable**</span><span class="sxs-lookup"><span data-stu-id="c1769-533">**Nullable**</span></span>              | <span data-ttu-id="c1769-534">No</span><span class="sxs-lookup"><span data-stu-id="c1769-534">No</span></span>          | <span data-ttu-id="c1769-535">**True** (valore predefinito) o **false** a seconda che la colonna corrispondente possa avere un valore null.</span><span class="sxs-lookup"><span data-stu-id="c1769-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="c1769-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="c1769-536">**DefaultValue**</span></span>          | <span data-ttu-id="c1769-537">No</span><span class="sxs-lookup"><span data-stu-id="c1769-537">No</span></span>          | <span data-ttu-id="c1769-538">Valore predefinito della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="c1769-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c1769-539">**MaxLength**</span></span>             | <span data-ttu-id="c1769-540">No</span><span class="sxs-lookup"><span data-stu-id="c1769-540">No</span></span>          | <span data-ttu-id="c1769-541">Lunghezza massima della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="c1769-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="c1769-542">**FixedLength**</span></span>           | <span data-ttu-id="c1769-543">No</span><span class="sxs-lookup"><span data-stu-id="c1769-543">No</span></span>          | <span data-ttu-id="c1769-544">**True** o **false** a seconda che il valore della colonna corrispondente venga archiviato come stringa a lunghezza fissa.</span><span class="sxs-lookup"><span data-stu-id="c1769-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="c1769-545">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="c1769-545">**Precision**</span></span>             | <span data-ttu-id="c1769-546">No</span><span class="sxs-lookup"><span data-stu-id="c1769-546">No</span></span>          | <span data-ttu-id="c1769-547">Precisione della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="c1769-548">**Scala**</span><span class="sxs-lookup"><span data-stu-id="c1769-548">**Scale**</span></span>                 | <span data-ttu-id="c1769-549">No</span><span class="sxs-lookup"><span data-stu-id="c1769-549">No</span></span>          | <span data-ttu-id="c1769-550">Scala della colonna corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c1769-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="c1769-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="c1769-551">**Unicode**</span></span>               | <span data-ttu-id="c1769-552">No</span><span class="sxs-lookup"><span data-stu-id="c1769-552">No</span></span>          | <span data-ttu-id="c1769-553">**True** o **false** a seconda che il valore della colonna corrispondente venga archiviato come stringa Unicode.</span><span class="sxs-lookup"><span data-stu-id="c1769-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="c1769-554">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="c1769-554">**Collation**</span></span>             | <span data-ttu-id="c1769-555">No</span><span class="sxs-lookup"><span data-stu-id="c1769-555">No</span></span>          | <span data-ttu-id="c1769-556">Stringa che specifica la sequenza di confronto da utilizzare nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="c1769-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="c1769-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="c1769-557">**SRID**</span></span>                  | <span data-ttu-id="c1769-558">No</span><span class="sxs-lookup"><span data-stu-id="c1769-558">No</span></span>          | <span data-ttu-id="c1769-559">Identificatore di riferimento del sistema spaziale.</span><span class="sxs-lookup"><span data-stu-id="c1769-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="c1769-560">Valido solo per le proprietà dei tipi spaziali.</span><span class="sxs-lookup"><span data-stu-id="c1769-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="c1769-561">Per ulteriori informazioni, vedere [SRID](https://en.wikipedia.org/wiki/SRID) e [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1769-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="c1769-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="c1769-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="c1769-563">No</span><span class="sxs-lookup"><span data-stu-id="c1769-563">No</span></span>          | <span data-ttu-id="c1769-564">**None**, **Identity** (se il valore della colonna corrispondente è un'identità generata nel database) o **calcolata** (se il valore della colonna corrispondente viene calcolato nel database).</span><span class="sxs-lookup"><span data-stu-id="c1769-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="c1769-565">Non valido per le proprietà RowType.</span><span class="sxs-lookup"><span data-stu-id="c1769-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-566">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="c1769-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="c1769-567">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-568">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-569">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-569">Example</span></span>

<span data-ttu-id="c1769-570">Nell'esempio seguente viene illustrato un elemento **EntityType** con due elementi **Proprietà** figlio:</span><span class="sxs-lookup"><span data-stu-id="c1769-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="c1769-571">Elemento PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="c1769-572">L'elemento **PropertyRef** in Store Schema Definition Language (SSDL) fa riferimento a una proprietà definita in un elemento EntityType per indicare che la proprietà eseguirà uno dei ruoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="c1769-573">Far parte della chiave primaria della tabella rappresentata da **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="c1769-574">Per definire una chiave primaria, è possibile usare uno o più elementi **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="c1769-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="c1769-575">Per ulteriori informazioni, vedere elemento Key.</span><span class="sxs-lookup"><span data-stu-id="c1769-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="c1769-576">Rappresenterà l'entità finale dipendente o principale di un vincolo referenziale.</span><span class="sxs-lookup"><span data-stu-id="c1769-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="c1769-577">Per altre informazioni, vedere elemento ReferentialConstraint.</span><span class="sxs-lookup"><span data-stu-id="c1769-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="c1769-578">L'elemento **PropertyRef** può avere solo gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="c1769-579">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-580">Elementi Annotation</span><span class="sxs-lookup"><span data-stu-id="c1769-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-581">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-581">Applicable Attributes</span></span>

<span data-ttu-id="c1769-582">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="c1769-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="c1769-583">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-583">Attribute Name</span></span> | <span data-ttu-id="c1769-584">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-584">Is Required</span></span> | <span data-ttu-id="c1769-585">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="c1769-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="c1769-586">**Name**</span></span>       | <span data-ttu-id="c1769-587">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-587">Yes</span></span>         | <span data-ttu-id="c1769-588">Nome della proprietà alla quale viene fatto riferimento.</span><span class="sxs-lookup"><span data-stu-id="c1769-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1769-589">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="c1769-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="c1769-590">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="c1769-591">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-592">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-592">Example</span></span>

<span data-ttu-id="c1769-593">Nell'esempio seguente viene illustrato un elemento **PropertyRef** utilizzato per definire una chiave primaria facendo riferimento a una proprietà definita in un elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="c1769-594">Elemento ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="c1769-595">L'elemento **ReferentialConstraint** in Store Schema Definition Language (SSDL) rappresenta un vincolo di chiave esterna (detto anche vincolo di integrità referenziale) nel database sottostante.</span><span class="sxs-lookup"><span data-stu-id="c1769-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="c1769-596">Le entità finali principali e dipendenti del vincolo sono specificate rispettivamente dagli elementi figlio Principal e dependent.</span><span class="sxs-lookup"><span data-stu-id="c1769-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="c1769-597">Alle colonne che fanno parte delle entità finali principali e dipendenti viene fatto riferimento con gli elementi PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="c1769-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="c1769-598">L'elemento **ReferentialConstraint** è un elemento figlio facoltativo dell'elemento Association.</span><span class="sxs-lookup"><span data-stu-id="c1769-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="c1769-599">Se non viene utilizzato un elemento **ReferentialConstraint** per eseguire il mapping del vincolo foreign key specificato nell'elemento **Association** , a tale scopo è necessario utilizzare un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="c1769-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="c1769-600">L'elemento **ReferentialConstraint** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="c1769-601">Documentazione (zero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="c1769-602">Principal (esattamente uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="c1769-603">Dipendente (esattamente uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="c1769-604">Elementi Annotation (zero o più elementi)</span><span class="sxs-lookup"><span data-stu-id="c1769-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-605">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-605">Applicable Attributes</span></span>

<span data-ttu-id="c1769-606">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="c1769-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="c1769-607">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-608">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-609">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-609">Example</span></span>

<span data-ttu-id="c1769-610">Nell'esempio seguente viene illustrato un elemento **Association** che usa un elemento **ReferentialConstraint** per specificare le colonne che fanno parte del vincolo di chiave esterna **FK @ no__t-3CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="c1769-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="c1769-611">Elemento ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="c1769-612">L'elemento **returnType** in Store Schema Definition Language (SSDL) specifica il tipo restituito per una funzione definita in un elemento **Function** .</span><span class="sxs-lookup"><span data-stu-id="c1769-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="c1769-613">Un tipo restituito di funzione può essere specificato anche con un attributo **returnType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="c1769-614">Il tipo restituito di una funzione viene specificato con l'attributo **Type** o l'elemento **returnType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="c1769-615">L'elemento **returnType** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="c1769-616">CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="c1769-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="c1769-617">Qualsiasi numero di attributi di annotazione (attributi XML personalizzati) può essere applicato all'elemento **returnType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="c1769-618">Tuttavia, gli attributi personalizzati non possono appartenere ad alcuno spazio dei nomi XML riservato a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="c1769-619">I nomi completi per due attributi personalizzati qualsiasi non possono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-620">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-620">Example</span></span>

<span data-ttu-id="c1769-621">Nell'esempio seguente viene utilizzata una **funzione** che restituisce una raccolta di righe.</span><span class="sxs-lookup"><span data-stu-id="c1769-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="c1769-622">Elemento RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="c1769-623">Un elemento **RowType** in Store Schema Definition Language (SSDL) definisce una struttura senza nome come tipo restituito per una funzione definita nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="c1769-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="c1769-624">Un elemento **RowType** è l'elemento figlio dell'elemento **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="c1769-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="c1769-625">Un elemento **RowType** può avere gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="c1769-626">Property (uno o più)</span><span class="sxs-lookup"><span data-stu-id="c1769-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="c1769-627">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-627">Example</span></span>

<span data-ttu-id="c1769-628">Nell'esempio seguente viene illustrata una funzione di archiviazione che utilizza un elemento **CollectionType** per specificare che la funzione restituisce una raccolta di righe, come specificato nell'elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="c1769-629">Elemento Schema (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="c1769-630">L'elemento **schema** nel Store Schema Definition Language (SSDL) è l'elemento radice di una definizione del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="c1769-631">Contiene definizioni per gli oggetti, le funzioni e i contenitori che costituiscono un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="c1769-632">L'elemento **schema** può contenere zero o più degli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="c1769-633">Associazione</span><span class="sxs-lookup"><span data-stu-id="c1769-633">Association</span></span>
-   <span data-ttu-id="c1769-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="c1769-634">EntityType</span></span>
-   <span data-ttu-id="c1769-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="c1769-635">EntityContainer</span></span>
-   <span data-ttu-id="c1769-636">Funzione</span><span class="sxs-lookup"><span data-stu-id="c1769-636">Function</span></span>

<span data-ttu-id="c1769-637">L'elemento **schema** utilizza l'attributo **namespace** per definire lo spazio dei nomi per il tipo di entità e gli oggetti di associazione in un modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="c1769-638">All'interno di uno spazio dei nomi due oggetti non possono avere lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="c1769-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="c1769-639">Uno spazio dei nomi del modello di archiviazione è diverso dallo spazio dei nomi XML dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="c1769-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="c1769-640">Uno spazio dei nomi del modello di archiviazione (definito dall'attributo **namespace** ) è un contenitore logico per tipi di entità e tipi di associazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="c1769-641">Lo spazio dei nomi XML, indicato dall'attributo **xmlns** , di un elemento **schema** è lo spazio dei nomi predefinito per gli elementi figlio e gli attributi dell'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="c1769-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="c1769-642">Gli spazi dei nomi XML nel formato https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (dove aaaa e MM rappresentano rispettivamente l'anno e il mese) sono riservati per SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="c1769-643">Gli elementi e gli attributi personalizzati non possono essere in spazi dei nomi che hanno questo form.</span><span class="sxs-lookup"><span data-stu-id="c1769-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="c1769-644">Attributi applicabili</span><span class="sxs-lookup"><span data-stu-id="c1769-644">Applicable Attributes</span></span>

<span data-ttu-id="c1769-645">Nella tabella seguente vengono descritti gli attributi che è possibile applicare all'elemento **schema** .</span><span class="sxs-lookup"><span data-stu-id="c1769-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="c1769-646">Nome attributo</span><span class="sxs-lookup"><span data-stu-id="c1769-646">Attribute Name</span></span>            | <span data-ttu-id="c1769-647">È obbligatorio</span><span class="sxs-lookup"><span data-stu-id="c1769-647">Is Required</span></span> | <span data-ttu-id="c1769-648">Value</span><span class="sxs-lookup"><span data-stu-id="c1769-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-649">**Spazio dei nomi**</span><span class="sxs-lookup"><span data-stu-id="c1769-649">**Namespace**</span></span>             | <span data-ttu-id="c1769-650">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-650">Yes</span></span>         | <span data-ttu-id="c1769-651">Spazio dei nomi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-651">The namespace of the storage model.</span></span> <span data-ttu-id="c1769-652">Il valore dell'attributo **namespace** viene utilizzato per formare il nome completo di un tipo.</span><span class="sxs-lookup"><span data-stu-id="c1769-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="c1769-653">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi ExampleModel. Store, il nome completo di **EntityType** sarà ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="c1769-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="c1769-654">Non è possibile usare le stringhe seguenti come valore per l'attributo **namespace** : **Sistema**, **temporaneo**o **EDM**.</span><span class="sxs-lookup"><span data-stu-id="c1769-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="c1769-655">Il valore dell'attributo **namespace** non può corrispondere al valore dell'attributo **namespace** nell'elemento schema CSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="c1769-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="c1769-656">**Alias**</span></span>                 | <span data-ttu-id="c1769-657">No</span><span class="sxs-lookup"><span data-stu-id="c1769-657">No</span></span>          | <span data-ttu-id="c1769-658">Identificatore utilizzato al posto del nome dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="c1769-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="c1769-659">Se, ad esempio, un elemento **EntityType** denominato *Customer* si trova nello spazio dei nomi ExampleModel. Store e il valore dell'attributo **alias** è *StorageModel*, è possibile utilizzare StorageModel. Customer come nome completo del  **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="c1769-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="c1769-660">**Provider**</span><span class="sxs-lookup"><span data-stu-id="c1769-660">**Provider**</span></span>              | <span data-ttu-id="c1769-661">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-661">Yes</span></span>         | <span data-ttu-id="c1769-662">Provider di dati.</span><span class="sxs-lookup"><span data-stu-id="c1769-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="c1769-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="c1769-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="c1769-664">Yes</span><span class="sxs-lookup"><span data-stu-id="c1769-664">Yes</span></span>         | <span data-ttu-id="c1769-665">Token che indica al provider quale manifesto del provider restituire.</span><span class="sxs-lookup"><span data-stu-id="c1769-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="c1769-666">Non è definito alcun formato per il token.</span><span class="sxs-lookup"><span data-stu-id="c1769-666">No format for the token is defined.</span></span> <span data-ttu-id="c1769-667">I valori per il token sono definiti dal provider.</span><span class="sxs-lookup"><span data-stu-id="c1769-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="c1769-668">Per informazioni sui token del manifesto del provider SQL Server, vedere SqlClient per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c1769-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="c1769-669">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-669">Example</span></span>

<span data-ttu-id="c1769-670">Nell'esempio seguente viene illustrato un elemento **dello schema** che contiene un elemento **EntityContainer** , due elementi **EntityType** e un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="c1769-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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

## <a name="annotation-attributes"></a><span data-ttu-id="c1769-671">Attributi di annotazione</span><span class="sxs-lookup"><span data-stu-id="c1769-671">Annotation Attributes</span></span>

<span data-ttu-id="c1769-672">Gli attributi di annotazione in Store Schema Definition Language (SSDL) sono attributi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sugli elementi del modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="c1769-673">Oltre ad avere una struttura XML valida, agli attributi di annotazione si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="c1769-674">Gli attributi di annotazione non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="c1769-675">I nomi completi di due attributi di annotazione non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="c1769-676">È possibile applicare più attributi di annotazione a un determinato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="c1769-677">È possibile accedere ai metadati contenuti negli elementi Annotation in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="c1769-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-678">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-678">Example</span></span>

<span data-ttu-id="c1769-679">Nell'esempio seguente viene illustrato un elemento EntityType con un attributo annotation applicato alla proprietà **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="c1769-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="c1769-680">Nell'esempio viene inoltre mostrato un elemento Annotation aggiunto all'elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="c1769-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="c1769-681">Elementi Annotation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="c1769-682">Gli elementi Annotation in SSDL (Store Schema Definition Language) sono elementi XML personalizzati nel modello di archiviazione che forniscono metadati aggiuntivi sul modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c1769-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="c1769-683">Oltre ad avere una struttura XML valida, agli elementi Annotation si applicano i vincoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1769-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="c1769-684">Gli elementi Annotation non devono trovarsi in spazi dei nomi XML riservati a SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="c1769-685">I nomi completi di due elementi Annotation non devono essere uguali.</span><span class="sxs-lookup"><span data-stu-id="c1769-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="c1769-686">Gli elementi Annotation devono apparire dopo tutti gli altri elementi figlio di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="c1769-687">Più elementi Annotation possono essere figli di un dato elemento SSDL.</span><span class="sxs-lookup"><span data-stu-id="c1769-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="c1769-688">A partire da .NET Framework versione 4, è possibile accedere ai metadati contenuti negli elementi di annotazione in fase di esecuzione usando le classi nello spazio dei nomi System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="c1769-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="c1769-689">Esempio</span><span class="sxs-lookup"><span data-stu-id="c1769-689">Example</span></span>

<span data-ttu-id="c1769-690">Nell'esempio seguente viene illustrato un elemento EntityType con un elemento Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="c1769-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="c1769-691">Nell'esempio viene inoltre illustrato un attributo di annotazione applicato alla proprietà **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="c1769-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="c1769-692">Facet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="c1769-692">Facets (SSDL)</span></span>

<span data-ttu-id="c1769-693">I facet in Store Schema Definition Language (SSDL) rappresentano vincoli sui tipi di colonna specificati negli elementi della proprietà.</span><span class="sxs-lookup"><span data-stu-id="c1769-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="c1769-694">I facet vengono implementati come attributi XML sugli elementi della **Proprietà** .</span><span class="sxs-lookup"><span data-stu-id="c1769-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="c1769-695">Nella tabella seguente vengono descritti i facet supportati in SSDL:</span><span class="sxs-lookup"><span data-stu-id="c1769-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="c1769-696">Facet</span><span class="sxs-lookup"><span data-stu-id="c1769-696">Facet</span></span>           | <span data-ttu-id="c1769-697">Descrizione</span><span class="sxs-lookup"><span data-stu-id="c1769-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c1769-698">**Confronto**</span><span class="sxs-lookup"><span data-stu-id="c1769-698">**Collation**</span></span>   | <span data-ttu-id="c1769-699">Specifica la sequenza di ordinamento da usare quando si eseguono operazioni di confronto e di ordinamento su valori della proprietà.</span><span class="sxs-lookup"><span data-stu-id="c1769-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="c1769-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="c1769-700">**FixedLength**</span></span> | <span data-ttu-id="c1769-701">Specifica se la lunghezza del valore della colonna può variare.</span><span class="sxs-lookup"><span data-stu-id="c1769-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="c1769-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="c1769-702">**MaxLength**</span></span>   | <span data-ttu-id="c1769-703">Specifica la lunghezza massima del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="c1769-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="c1769-704">**Precisione**</span><span class="sxs-lookup"><span data-stu-id="c1769-704">**Precision**</span></span>   | <span data-ttu-id="c1769-705">Per le proprietà di tipo **Decimal**, specifica il numero di cifre che un valore della proprietà può avere.</span><span class="sxs-lookup"><span data-stu-id="c1769-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="c1769-706">Per le proprietà di tipo **Time**, **DateTime**e **DateTimeOffset**, specifica il numero di cifre per la parte frazionaria dei secondi del valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="c1769-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="c1769-707">**Scala**</span><span class="sxs-lookup"><span data-stu-id="c1769-707">**Scale**</span></span>       | <span data-ttu-id="c1769-708">Specifica il numero di cifre a destra del separatore decimale per il valore della colonna.</span><span class="sxs-lookup"><span data-stu-id="c1769-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="c1769-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="c1769-709">**Unicode**</span></span>     | <span data-ttu-id="c1769-710">Indica se il valore della colonna viene archiviato come Unicode.</span><span class="sxs-lookup"><span data-stu-id="c1769-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
